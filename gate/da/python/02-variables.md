# VARIABLES — GATE DA QUICK REVISION

# variable = name referring to object

x = 10
name = "AI"

Rules:
- start with letter or _
- cannot start with digit
- no spaces
- case sensitive
- keywords not allowed

Valid:
a=1
_a=2
a1=3

Invalid:
1a=10
my var=5

Dynamic typing:
x=10
x="hi"

Type check:
type(x)

Multiple assignment:
a,b=1,2
a=b=c=5

Swap:
a,b=b,a

Input:
x=input()          # string
x=int(input())     # integer

Type conversion:
int("5")
float(3)
str(10)

Important:
x=5
y=x
# both refer same object

Identity:
id(x)

Operators:
+ - * / // % **
= += -= *=

Comparison:
== != > < >= <=

Logical:
and or not

Membership:
in , not in

Naming styles:
snake_case
camelCase

Mutable vs Immutable:
int,str,tuple -> immutable
list,set,dict -> mutable

Delete variable:
del x

None:
x=None

Common GATE traps:

x=10
y=10
x is y   # may be True

== -> value compare
is -> identity compare

Example:
a=[1,2]
b=[1,2]

a==b  # True
a is b # False

Memory:
variable stores reference, not actual object