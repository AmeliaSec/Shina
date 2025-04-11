# 🧠 SHINA — Smart Heuristic Interpreter for Nefarious Activities

**Shina** is a lightweight, fast, and extensible rule-based detection engine designed for log analysis, anomaly hunting, and embedded detection in SIEM pipelines.  
It turns simple, human-readable expressions into structured logic trees and evaluates them against raw and parsed logs.

> ✨ Inspired by retro cyberpunk vibes. Built for modern threats.

---

## 💡 What is SHINA?

SHINA stands for:

> **S**mart **H**euristic **I**nterpreter for **N**efarious **A**ctivities

It is a DSL-powered engine that lets you write detection logic like this:

```txt
path == "/vulns" && user-agent CONTAINS "fox"
```

…and automatically transforms it into structured conditions and evaluates them efficiently.

---

## 🚀 Key Features

- 🧠 **Custom DSL** with logical operators (`&&`, `||`)
- 🔍 **Operators**: `==`, `CONTAINS`, `RCONTAINS`
- 🔀 **Logical trees** with precedence (AND > OR)
- 🧾 **YAML-based rule loading**
- ⚙️ Designed for integration with your own SIEM (like [Amelia.lu](https://amelia.lu))

---

## 📦 Example Rule YAML

Here’s how a SHINA rule looks like in YAML:

```yaml
- name: Fox detector
  description: Detect fox browser accessing vuln endpoint
  metadata:
    severity: medium
    author: Pascalito
  rules: path == "/vulns" && user-agent CONTAINS "fox"
```

---

## 🦾 How It Works

1. **Define a rule in YAML**
2. **SHINA loads it** and parses it into a `Condition` tree
3. **Evaluate the rule** on a raw log line and its parsed key-values

---

## 🛠 DSL Syntax

| Operator      | Meaning                                     |
|---------------|---------------------------------------------|
| `==`          | Field equals value                          |
| `CONTAINS`    | Field contains substring                    |
| `RCONTAINS`   | Raw line contains substring (only for `raw`)|
| `&&`          | Logical AND                                 |
| `||`          | Logical OR                                  |
| `()`          | Grouping expressions                        |

---

## 🔍 DSL Example

```txt
method == "POST" && path == "/admin" || user-agent CONTAINS "curl"
```

This will match:
- Logs where `method` is POST and `path` is `/admin`
- OR logs where `user-agent` contains `curl`

---

## 🧪 Usage in Rust

```rust
let expr = r#"path == "/vulns" && user-agent CONTAINS "fox""#;
let condition = parse_shina_expression(expr).unwrap();

let raw = r#"GET /vulns HTTP/1.1 - user-agent: firefox"#;
let mut parsed = HashMap::new();
parsed.insert("path".to_string(), "/vulns".to_string());
parsed.insert("user-agent".to_string(), "firefox".to_string());

if eval(&condition, &raw, &parsed) {
    println!("🛑 Threat detected!");
}
```

---

## 🧬 Integration Example (YAML → Shina)

```rust
let yaml = r#"
- name: Fox detector
  description: Detect fox in the logs
  metadata:
    author: Pascalito
  rules: path == "/vulns" && user-agent CONTAINS "fox"
"#;

let shinas = parse_shina_from_yaml(yaml).unwrap();
for rule in shinas {
    let condition = parse_shina_expression(&rule.raw_rule).unwrap();
    // eval condition here
}
```

---

## 📡 Roadmap

- [ ] Add `NOT`, `REGEX`, `IN` operators
- [ ] Rule packs with versioning
- [ ] Rule testing framework
- [ ] Web UI with retro terminal vibes
- [ ] Full SIEM integration (→ [Amelia.lu](https://amelia.lu))

---

## ⚡ About

**Shina** is part of the [Amelia](https://amelia.lu) project — a retro-cyber detection interface built for analysts and by analysts.  
Handcrafted with 💜 by [@Sn0wAlice](https://github.com/Sn0wAlice)
