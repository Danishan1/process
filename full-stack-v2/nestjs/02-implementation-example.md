Yes. Let's build a **complete but interview-sized NestJS CRUD API** so you can see how all the pieces fit together.

We'll implement a **User API** with:

* Module
* Controller
* Service
* DTO
* Validation
* Dependency Injection
* Middleware
* Guard
* Custom decorator
* Pipe
* Interceptor
* Exception handling
* PostgreSQL-style repository layer
* Configuration
* Authentication concept

The goal is not just to copy code—you should be able to explain **why each layer exists**.

---

# 1. Project structure

A realistic structure:

```text
src/
│
├── main.ts
├── app.module.ts
│
├── common/
│   ├── middleware/
│   │   └── logger.middleware.ts
│   │
│   ├── guards/
│   │   └── auth.guard.ts
│   │
│   ├── interceptors/
│   │   └── logging.interceptor.ts
│   │
│   ├── filters/
│   │   └── http-exception.filter.ts
│   │
│   └── decorators/
│       └── user.decorator.ts
│
├── users/
│   ├── users.module.ts
│   ├── users.controller.ts
│   ├── users.service.ts
│   ├── users.repository.ts
│   │
│   └── dto/
│       ├── create-user.dto.ts
│       └── update-user.dto.ts
│
└── database/
    └── database.module.ts
```

Compare this to Express:

```text
Express

route
  ↓
controller
  ↓
service
  ↓
database
```

Nest:

```text
Module
  │
  ├── Controller
  │      ↓
  ├── Service
  │      ↓
  └── Repository
         ↓
      Database
```

---

# 2. Install NestJS

```bash
npm i -g @nestjs/cli

nest new nest-demo
```

Then:

```bash
cd nest-demo
```

Install validation:

```bash
npm install class-validator class-transformer
```

---

# 3. `main.ts`

This is the entry point.

```ts
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      transform: true,
    }),
  );

  app.setGlobalPrefix('api');

  await app.listen(3000);
}

bootstrap();
```

Now:

```text
GET /users
```

becomes:

```text
GET /api/users
```

### Important interview point

`ValidationPipe` automatically validates DTOs.

`whitelist: true` removes properties that aren't defined in the DTO.

`transform: true` allows transformation of incoming values into expected types.

---

# 4. `app.module.ts`

The root module:

```ts
import { Module } from '@nestjs/common';
import { UsersModule } from './users/users.module';

@Module({
  imports: [
    UsersModule,
  ],
})
export class AppModule {}
```

Think of this as the application's module composition.

```text
AppModule
    │
    └── UsersModule
```

---

# 5. Users module

`users/users.module.ts`

```ts
import { Module } from '@nestjs/common';

import { UsersController } from './users.controller';
import { UsersService } from './users.service';
import { UsersRepository } from './users.repository';

@Module({
  controllers: [
    UsersController,
  ],

  providers: [
    UsersService,
    UsersRepository,
  ],

  exports: [
    UsersService,
  ],
})
export class UsersModule {}
```

This is extremely important.

The module tells Nest:

> "These are the components that belong to the Users feature."

---

# 6. DTO

Create:

`users/dto/create-user.dto.ts`

```ts
import {
  IsEmail,
  IsNotEmpty,
  IsString,
  MinLength,
} from 'class-validator';

export class CreateUserDto {

  @IsString()
  @IsNotEmpty()
  name: string;

  @IsEmail()
  email: string;

  @IsString()
  @MinLength(8)
  password: string;
}
```

Now if someone sends:

```json
{
  "name": "Danishan",
  "email": "invalid",
  "password": "123"
}
```

validation fails before reaching the controller method.

That's the power of:

```text
DTO + ValidationPipe
```

---

# 7. Update DTO

`users/dto/update-user.dto.ts`

```ts
import {
  IsEmail,
  IsOptional,
  IsString,
  MinLength,
} from 'class-validator';

export class UpdateUserDto {

  @IsOptional()
  @IsString()
  name?: string;

  @IsOptional()
  @IsEmail()
  email?: string;

  @IsOptional()
  @IsString()
  @MinLength(8)
  password?: string;
}
```

---

# 8. Repository

`users/users.repository.ts`

For simplicity, we'll use an in-memory array.

In a real application, this would call PostgreSQL through Prisma/TypeORM/etc.

```ts
import { Injectable } from '@nestjs/common';

export interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}

@Injectable()
export class UsersRepository {

  private users: User[] = [];

  private idCounter = 1;

  async findAll(): Promise<User[]> {
    return this.users;
  }

  async findById(id: number): Promise<User | undefined> {
    return this.users.find(user => user.id === id);
  }

  async findByEmail(email: string): Promise<User | undefined> {
    return this.users.find(user => user.email === email);
  }

  async create(
    data: Omit<User, 'id'>,
  ): Promise<User> {

    const user: User = {
      id: this.idCounter++,
      ...data,
    };

    this.users.push(user);

    return user;
  }

  async update(
    id: number,
    data: Partial<Omit<User, 'id'>>,
  ): Promise<User | undefined> {

    const user = await this.findById(id);

    if (!user) {
      return undefined;
    }

    Object.assign(user, data);

    return user;
  }

  async delete(id: number): Promise<boolean> {

    const index = this.users.findIndex(
      user => user.id === id,
    );

    if (index === -1) {
      return false;
    }

    this.users.splice(index, 1);

    return true;
  }
}
```

Notice:

```ts
@Injectable()
```

This tells Nest that this class can participate in **Dependency Injection**.

---

# 9. Service

`users/users.service.ts`

```ts
import {
  ConflictException,
  Injectable,
  NotFoundException,
} from '@nestjs/common';

import { UsersRepository } from './users.repository';

import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';

@Injectable()
export class UsersService {

  constructor(
    private readonly usersRepository: UsersRepository,
  ) {}

  async findAll() {
    return this.usersRepository.findAll();
  }

  async findOne(id: number) {

    const user =
      await this.usersRepository.findById(id);

    if (!user) {
      throw new NotFoundException(
        `User ${id} not found`,
      );
    }

    return user;
  }

  async create(dto: CreateUserDto) {

    const existingUser =
      await this.usersRepository.findByEmail(
        dto.email,
      );

    if (existingUser) {
      throw new ConflictException(
        'Email already exists',
      );
    }

    return this.usersRepository.create({
      name: dto.name,
      email: dto.email,
      password: dto.password,
    });
  }

  async update(
    id: number,
    dto: UpdateUserDto,
  ) {

    await this.findOne(id);

    return this.usersRepository.update(
      id,
      dto,
    );
  }

  async remove(id: number) {

    await this.findOne(id);

    await this.usersRepository.delete(id);

    return {
      message: 'User deleted successfully',
    };
  }
}
```

---

# 10. Dependency Injection

This line:

```ts
constructor(
  private readonly usersRepository: UsersRepository,
) {}
```

is extremely important.

You didn't write:

```ts
new UsersRepository()
```

Nest creates and injects it.

The dependency graph is:

```text
UsersController
       │
       │ needs
       ↓
UsersService
       │
       │ needs
       ↓
UsersRepository
```

Nest's IoC container manages those dependencies.

### Interview answer

If asked:

> How does dependency injection work in NestJS?

You can say:

> "NestJS has an IoC container that manages providers. When a class declares a dependency through its constructor, Nest resolves that dependency from the module's provider registry and injects it. This reduces tight coupling and makes components easier to test and replace."

That's a strong SDE-II answer.

---

# 11. Controller

`users/users.controller.ts`

```ts
import {
  Body,
  Controller,
  Delete,
  Get,
  Param,
  ParseIntPipe,
  Patch,
  Post,
} from '@nestjs/common';

import { UsersService } from './users.service';

import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';

@Controller('users')
export class UsersController {

  constructor(
    private readonly usersService: UsersService,
  ) {}

  @Get()
  findAll() {
    return this.usersService.findAll();
  }

  @Get(':id')
  findOne(
    @Param('id', ParseIntPipe) id: number,
  ) {
    return this.usersService.findOne(id);
  }

  @Post()
  create(
    @Body() dto: CreateUserDto,
  ) {
    return this.usersService.create(dto);
  }

  @Patch(':id')
  update(
    @Param('id', ParseIntPipe) id: number,
    @Body() dto: UpdateUserDto,
  ) {
    return this.usersService.update(id, dto);
  }

  @Delete(':id')
  remove(
    @Param('id', ParseIntPipe) id: number,
  ) {
    return this.usersService.remove(id);
  }
}
```

Now we have:

```text
POST   /api/users
GET    /api/users
GET    /api/users/:id
PATCH  /api/users/:id
DELETE /api/users/:id
```

---

# 12. Compare with Express

You probably know this:

```ts
router.get('/users/:id', async (req, res) => {

  const id = Number(req.params.id);

  const user = await userService.findOne(id);

  res.json(user);
});
```

Nest:

```ts
@Get(':id')
findOne(
  @Param('id', ParseIntPipe) id: number,
) {
  return this.usersService.findOne(id);
}
```

Nest takes care of:

```text
routing
request extraction
validation/transformation
response serialization
exception handling
```

So you write less plumbing code.

---

# 13. Middleware

Create:

`common/middleware/logger.middleware.ts`

```ts
import {
  Injectable,
  NestMiddleware,
} from '@nestjs/common';

import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware
  implements NestMiddleware {

  use(
    req: Request,
    res: Response,
    next: NextFunction,
  ) {

    const start = Date.now();

    res.on('finish', () => {

      const duration =
        Date.now() - start;

      console.log(
        `${req.method} ${req.originalUrl} ${res.statusCode} ${duration}ms`,
      );
    });

    next();
  }
}
```

---

# 14. Register middleware

In `users.module.ts`:

```ts
import {
  MiddlewareConsumer,
  Module,
  NestModule,
} from '@nestjs/common';

import { LoggerMiddleware } from '../common/middleware/logger.middleware';

import { UsersController } from './users.controller';
import { UsersService } from './users.service';
import { UsersRepository } from './users.repository';

@Module({
  controllers: [
    UsersController,
  ],

  providers: [
    UsersService,
    UsersRepository,
  ],

  exports: [
    UsersService,
  ],
})
export class UsersModule
  implements NestModule {

  configure(
    consumer: MiddlewareConsumer,
  ) {

    consumer
      .apply(LoggerMiddleware)
      .forRoutes(UsersController);
  }
}
```

Now the middleware runs for the Users controller.

---

# 15. Guard

Suppose we want to protect our API.

`common/guards/auth.guard.ts`

```ts
import {
  CanActivate,
  ExecutionContext,
  Injectable,
  UnauthorizedException,
} from '@nestjs/common';

@Injectable()
export class AuthGuard implements CanActivate {

  canActivate(
    context: ExecutionContext,
  ): boolean {

    const request =
      context.switchToHttp().getRequest();

    const token =
      request.headers.authorization;

    if (!token) {
      throw new UnauthorizedException(
        'Authorization token required',
      );
    }

    return true;
  }
}
```

Apply it:

```ts
import { UseGuards } from '@nestjs/common';

@UseGuards(AuthGuard)
@Get()
findAll() {
  return this.usersService.findAll();
}
```

Now:

```text
Request
   ↓
AuthGuard
   ↓
Controller
```

---

# 16. Middleware vs Guard

This is a classic interview question.

### Middleware

```text
Request
 ↓
Middleware
 ↓
...
```

Middleware is generic request processing.

### Guard

```text
Request
 ↓
Guard
 ↓
Controller
```

Guard decides whether the request is allowed to execute.

For example:

```text
Middleware
→ logging

Guard
→ authentication / authorization
```

A very good interview phrase:

> **"Middleware is primarily for request preprocessing, while guards are designed to make authorization/access-control decisions based on the execution context."**

---

# 17. Custom decorator

Suppose authentication adds:

```ts
request.user
```

We can create:

`common/decorators/user.decorator.ts`

```ts
import {
  createParamDecorator,
  ExecutionContext,
} from '@nestjs/common';

export const CurrentUser = createParamDecorator(
  (
    data: unknown,
    ctx: ExecutionContext,
  ) => {

    const request =
      ctx.switchToHttp().getRequest();

    return request.user;
  },
);
```

Then:

```ts
@Get('profile')
getProfile(
  @CurrentUser() user: any,
) {
  return user;
}
```

Instead of repeatedly writing:

```ts
@Req() req
```

and:

```ts
req.user
```

---

# 18. Interceptor

Create:

`common/interceptors/logging.interceptor.ts`

```ts
import {
  CallHandler,
  ExecutionContext,
  Injectable,
  NestInterceptor,
} from '@nestjs/common';

import { Observable } from 'rxjs';

import { tap } from 'rxjs/operators';

@Injectable()
export class LoggingInterceptor
  implements NestInterceptor {

  intercept(
    context: ExecutionContext,
    next: CallHandler,
  ): Observable<any> {

    const start = Date.now();

    return next.handle().pipe(

      tap(() => {

        const duration =
          Date.now() - start;

        console.log(
          `Request completed in ${duration}ms`,
        );
      }),

    );
  }
}
```

Apply:

```ts
@UseInterceptors(LoggingInterceptor)
@Get()
findAll() {
  return this.usersService.findAll();
}
```

The important concept:

```text
Interceptor
    ↓
  before
    ↓
Controller
    ↓
  after
    ↓
Response
```

That's why interceptors are useful for:

```text
logging
metrics
response transformation
caching
timeouts
```

---

# 19. Exception handling

Nest provides built-in exceptions:

```ts
throw new NotFoundException();

throw new BadRequestException();

throw new UnauthorizedException();

throw new ForbiddenException();

throw new ConflictException();

throw new InternalServerErrorException();
```

For example:

```ts
if (!user) {
  throw new NotFoundException(
    'User not found',
  );
}
```

Nest converts this into an HTTP response.

For example:

```json
{
  "statusCode": 404,
  "message": "User not found",
  "error": "Not Found"
}
```

---

# 20. Custom Exception Filter

If you want centralized error formatting:

```ts
import {
  ArgumentsHost,
  Catch,
  ExceptionFilter,
  HttpException,
} from '@nestjs/common';

import { Request, Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter
  implements ExceptionFilter {

  catch(
    exception: HttpException,
    host: ArgumentsHost,
  ) {

    const ctx =
      host.switchToHttp();

    const response =
      ctx.getResponse<Response>();

    const request =
      ctx.getRequest<Request>();

    const status =
      exception.getStatus();

    const exceptionResponse =
      exception.getResponse();

    response.status(status).json({

      success: false,

      statusCode: status,

      path: request.url,

      timestamp: new Date().toISOString(),

      error: exceptionResponse,
    });
  }
}
```

Register globally:

```ts
app.useGlobalFilters(
  new HttpExceptionFilter(),
);
```

---

# 21. Complete request flow

Now our request can look like:

```text
POST /api/users
        │
        ▼
┌─────────────────┐
│    Middleware   │
│     Logging     │
└────────┬────────┘
         ▼
┌─────────────────┐
│      Guard      │
│ Authentication  │
└────────┬────────┘
         ▼
┌─────────────────┐
│   Interceptor   │
│   Before        │
└────────┬────────┘
         ▼
┌─────────────────┐
│      Pipe       │
│ DTO Validation  │
└────────┬────────┘
         ▼
┌─────────────────┐
│    Controller   │
│ POST /users     │
└────────┬────────┘
         ▼
┌─────────────────┐
│     Service     │
│ Business Logic  │
└────────┬────────┘
         ▼
┌─────────────────┐
│    Repository   │
│    Database     │
└────────┬────────┘
         ▼
┌─────────────────┐
│   Interceptor   │
│     After       │
└────────┬────────┘
         ▼
      Response
```

That diagram alone is worth remembering for your interview.

---

# 22. Where PostgreSQL fits

Our example currently has:

```text
UsersRepository
    ↓
Array
```

In a real application:

```text
UsersRepository
    ↓
Prisma / TypeORM
    ↓
PostgreSQL
```

For example, with Prisma your service could remain almost identical:

```ts
@Injectable()
export class UsersService {

  constructor(
    private readonly prisma: PrismaService,
  ) {}

  async findAll() {
    return this.prisma.user.findMany();
  }

  async findOne(id: number) {

    const user =
      await this.prisma.user.findUnique({
        where: { id },
      });

    if (!user) {
      throw new NotFoundException(
        'User not found',
      );
    }

    return user;
  }
}
```

This is another benefit of DI.

Your controller doesn't care whether the service uses:

```text
PostgreSQL
MongoDB
Redis
External API
Mock
```

---

# 23. The complete architecture

For an actual production application, I'd think about it as:

```text
                    Client
                      │
                      ▼
                 Nginx / LB
                      │
                      ▼
                NestJS API
                      │
          ┌───────────┴───────────┐
          │                       │
       Middleware              Guards
          │                       │
          └───────────┬───────────┘
                      ▼
                  Controller
                      │
                      ▼
                   Service
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
      Repository    Redis       Kafka
          │
          ▼
      PostgreSQL
```

This should look very familiar to you as an Express backend developer.

The **business architecture isn't radically different**.

NestJS mainly gives you a standardized way to structure it.

---

# 24. Express → NestJS mapping to memorize

For your interview, I'd memorize this table:

| If interviewer asks about... | Think                |
| ---------------------------- | -------------------- |
| Express Router               | Nest Controller      |
| `router.get()`               | `@Get()`             |
| `router.post()`              | `@Post()`            |
| `req.params`                 | `@Param()`           |
| `req.body`                   | `@Body()`            |
| `req.query`                  | `@Query()`           |
| `req.headers`                | `@Headers()`         |
| `res.json()`                 | `return data`        |
| Middleware                   | Middleware           |
| Authentication middleware    | Guard                |
| Validation middleware        | Pipe                 |
| Response middleware          | Interceptor          |
| Error middleware             | Exception Filter     |
| Service class                | Provider             |
| `new Service()`              | Dependency Injection |
| Router grouping              | Module               |
| Environment variables        | ConfigModule         |
| Express app                  | Nest Application     |

---

# 25. What you should be able to explain in the interview

Given your **3.5 years of Express experience**, I wouldn't try to memorize hundreds of NestJS APIs.

You should be able to confidently answer these:

### Core

1. What is NestJS?
2. Why use NestJS over Express?
3. How is NestJS different from Express?
4. What is a module?
5. What is a controller?
6. What is a provider?
7. What is dependency injection?
8. What is IoC?

### Request lifecycle

9. Middleware vs Guard?
10. Guard vs Interceptor?
11. Pipe vs Middleware?
12. Interceptor vs Middleware?
13. What is an exception filter?
14. Explain NestJS request lifecycle.

### Backend

15. How do you validate request bodies?
16. What is a DTO?
17. How do you implement JWT authentication?
18. How do you implement RBAC?
19. How do you connect PostgreSQL?
20. How would you integrate Redis?
21. How would you integrate Kafka?
22. How do NestJS microservices work?

### SDE-II

23. How would you structure a large NestJS application?
24. How would you handle centralized logging?
25. How would you implement global error handling?
26. How would you implement rate limiting?
27. How would you implement caching?
28. How would you handle graceful shutdown?
29. How would you scale a NestJS service?
30. How would you monitor a production NestJS application?

---

## The most important thing for you

Don't approach NestJS like a beginner:

> ❌ "I need to learn NestJS."

Your actual position is:

> **"I already know Node.js + Express + REST + databases + Redis + Kafka + backend architecture. I need to learn how NestJS organizes and implements those concepts."**

That's a **much smaller learning gap**.

The next useful exercise would be to take this exact User API and make it **production-grade**: **PostgreSQL + Prisma + JWT + RBAC + Redis caching + rate limiting + Kafka + Docker**, because that is much closer to the kind of NestJS architecture an SDE-II interviewer is likely to probe.
