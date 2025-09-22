
# Data Structures Supported by Bash

### 1. **Scalar Variables**

* The simplest type: holds a single value (string or number).

```bash
NAME="Danishan"
AGE=25
```

* Everything is essentially a string, even numbers. Arithmetic requires `$(( ))`.



### 2. **Indexed Arrays (Lists)**

* A list of values, accessed by numeric indices.

```bash
FRUITS=("apple" "banana" "cherry")
echo ${FRUITS[0]}   # apple
echo ${FRUITS[@]}   # all elements
```

* Supports: iteration, slicing, length:

```bash
echo ${#FRUITS[@]}      # 3
echo ${FRUITS[@]:1:2}   # banana cherry
```


### 3. **Associative Arrays (Dictionaries / Maps)**

* Key-value pairs (bash ≥ 4):

```bash
declare -A COLORS
COLORS[apple]="red"
COLORS[banana]="yellow"
echo ${COLORS[apple]}    # red
```

* Limitations:

  * Only strings as keys/values.
  * No natural nesting (need workarounds).


### 4. **Read-Only Variables**

* Not strictly a data structure, but useful to protect values.

```bash
readonly PI=3.1416
PI=4        # error
```


### 5. **Emulated / Advanced Structures**

Bash **cannot natively handle nested objects or complex structures**, but you can emulate:

* **Nested dictionaries using key patterns**

```bash
declare -A USERS
USERS["alice:age"]=25
USERS["alice:city"]="NY"
echo ${USERS["alice:city"]}
```

* **Arrays of arrays** (via string separators)

```bash
ARR1=("a,b" "c,d")
IFS=',' read -ra FIRST <<< "${ARR1[0]}"
echo ${FIRST[0]}   # a
```

* **External formats for structured data**

  * JSON → `jq`
  * YAML → `yq`
  * CSV → `awk`, `csvkit`


### Summary Table

| Data Structure      | Native in Bash? | Notes                            |
| ------------------- | --------------- | -------------------------------- |
| Scalar variable     | Yes             | String or number                 |
| Indexed array       | Yes             | List of values, numeric indices  |
| Associative array   | Yes (bash ≥4)   | Key-value pairs, strings only    |
| Nested arrays       | No (emulated)   | Use separators or keys           |
| Nested dictionaries | No (emulated)   | Use key patterns like `user:age` |
| Objects / Structs   | No              | Not supported                    |
| Sets / Tuples       | No              | Can emulate with arrays          |
| Read-only variable  | Yes             | Immutable scalar                 |



---

# Handling Real-World Data & Data Structures in Scripting

### 1. **Tabular Data (CSV / Logs / Reports)**

* Real world: log files, CSV exports, monitoring data.
* Tools:

  * `awk` → column-based extraction & processing
  * `cut`, `sort`, `uniq`, `wc` → simple tabular manipulation
  * `join`, `comm` → relational joins on text
* Example: get top 5 most common IPs in a log:

  ```bash
  awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -5
  ```


### 2. **Structured Data (JSON / YAML / XML)**

* Modern systems talk in JSON/YAML (APIs, configs, Kubernetes).
* Tools:

  * `jq` → JSON processor
  * `yq` → YAML processor
  * `xmlstarlet` → XML processor
* Example: extract user IDs from JSON:

  ```bash
  jq '.users[].id' data.json
  ```

### 3. **Key-Value Data (Configs / Env Files)**

* Many configs are simple `KEY=VALUE`.
* Bash associative arrays or `source` can handle these.
* Example: `.env` file:

  ```env
  DB_HOST=localhost
  DB_PORT=5432
  ```

  Usage:

  ```bash
  source .env
  echo "Connecting to $DB_HOST:$DB_PORT"
  ```

### 4. **Hierarchical Data (Nested Structures)**

* Bash doesn’t support nested arrays/dicts natively → emulate or parse into external tools.
* Example: handle YAML config:

  ```yaml
  service:
    name: myapp
    replicas: 3
  ```

  With `yq`:

  ```bash
  yq '.service.replicas' config.yaml
  ```

### 5. **Streaming Data (Pipelines)**

* Real-world data is often huge → process as streams, not in memory.
* Example: monitor logs in real-time for errors:

  ```bash
  tail -f /var/log/syslog | grep "ERROR"
  ```

### 6. **Relational / Multi-Dimensional Data**

* Example: CSV with multiple columns (user, department, salary).
* Handle via `awk` (like SQL):

  ```bash
  awk -F, '{dept[$2]+=$3} END {for (d in dept) print d, dept[d]}' employees.csv
  ```

  → sums salary by department.

### 7. **Hybrid Approach (Bash + Other Languages)**

* Often, scripts call **Python, Perl, or Go utilities** for heavy lifting:

  ```bash
  data=$(python3 -c 'import json,sys; print(json.load(sys.stdin)["users"][0]["id"])' < data.json)
  echo "First user ID: $data"
  ```

# Summary

Real-world scripting data handling = **Bash basics + external power tools**

| Data Type      | Bash Tool/Structure          | External Helper |
| -------------- | ---------------------------- | --------------- |
| Logs / CSV     | arrays, `awk`, `cut`         | `csvkit`        |
| JSON           | associative arrays (limited) | `jq`            |
| YAML           | emulate w/ arrays            | `yq`            |
| XML            | not native                   | `xmlstarlet`    |
| Key=Value      | `source`, arrays             | env parsers     |
| Streaming data | pipes, `tail`, `grep`        | log shippers    |



# Mapping Table: Best-Fit Tools for Data Handling in Scripting

| **Data Type**         | **Best-Fit Tool(s)**                   | **Use Case**                                                    |                                         |
| --------------------- | -------------------------------------- | --------------------------------------------------------------- | --------------------------------------- |
| **Plain text**        | `grep`, `sed`, `awk`                   | Searching logs, replacing patterns, extracting specific lines   |                                         |
| **Log files**         | `awk`, `cut`, `uniq`, `sort`, `wc`     | Counting errors, top IPs, frequency analysis                    |                                         |
| **CSV / Tabular**     | `awk`, `cut`, `csvkit`, `mlr (Miller)` | Summarizing salaries, grouping by department, filtering columns |                                         |
| **Key=Value Configs** | Bash vars, `source`, `awk`             | Loading `.env` files, system config parsing                     |                                         |
| **JSON**              | `jq`                                   | API responses, Kubernetes manifests, Terraform output parsing   |                                         |
| **YAML**              | `yq`                                   | Kubernetes configs, Ansible playbooks, CI/CD pipelines          |                                         |
| **XML**               | `xmlstarlet`, `xmllint`                | Config files, SOAP responses, build pipelines (Maven/Ant)       |                                         |
| **HTML / Web data**   | `xmllint --html`, `pup`, `htmlq`       | Scraping tags, extracting links                                 |                                         |
| **Numeric data**      | `bc`, `awk`                            | Math in scripts, statistics, currency conversion                |                                         |
| **Binary data**       | `xxd`, `od`, `hexdump`                 | Debugging binaries, file signatures, hex inspection             |                                         |
| **System data**       | `/proc`, `ps`, `df`, `top`, `free`     | Monitoring memory, CPU, disk usage                              |                                         |
| **Streaming data**    | Pipes (\`                              | `), `tail -f`, `grep\`                                          | Real-time log monitoring, ETL pipelines |
| **Relational data**   | `awk`, `mlr`, `sqlite3`                | SQL-like queries on CSVs, grouping, joins                       |                                         |
| **Large datasets**    | `mlr`, `parallel`, `xargs`             | Processing millions of rows in CSV, parallel batch operations   |                                         |



# Minimal Toolkit for Real-World Data Handling in Scripting

| **Category**                    | **Tool(s)**                                 | **Why Chosen (Reusable across use cases)**                                              |
| ------------------------------- | ------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Text / Logs**                 | `grep`, `awk`, `sed`                        | Core Unix trio: filtering, extracting, transforming. Covers most log and text parsing.  |
| **Tabular (CSV/TSV)**           | `awk` (built-in), `mlr (Miller)` (optional) | `awk` is everywhere; Miller adds SQL-like power for larger/messier datasets.            |
| **Structured Data (JSON/YAML)** | `jq`                                        | One tool that cleanly handles both JSON (native) and YAML (via `yq` wrapper around jq). |
| **Math / Numbers**              | `bc`                                        | Precise arithmetic and floating-point math (Bash is string-based).                      |
| **System Data**                 | Core utils (`ps`, `df`, `/proc`)            | Always available, minimal learning curve.                                               |


# Why this set works

* **Universal availability**: `awk`, `sed`, `grep`, `bc`, `ps`, `df` exist on nearly all Linux systems.
* **Structured data**: only **`jq`** needs to be added, but it solves JSON/YAML, which is unavoidable in modern DevOps/data.
* **Optional booster**: `mlr` (Miller) if you deal with **heavy CSV logs** (100k+ rows). Otherwise, stick with `awk`.


# Strategy

* **Start with core 4**: `grep + awk + sed + bc`.
* **Add `jq`** when JSON/YAML enters the picture.
* **Optionally add `mlr`** if CSV-heavy workloads become common.

