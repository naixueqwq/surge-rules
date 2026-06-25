<div align="center">

# ⚡ Surge Rules

**个人 Surge 分流规则 · 模块 · 脚本 · 配置**

[![Stars](https://img.shields.io/github/stars/Valiumlove/surge-rules?style=flat-square&color=FFD700)](https://github.com/Valiumlove/surge-rules/stargazers)
[![Last Commit](https://img.shields.io/github/last-commit/Valiumlove/surge-rules?style=flat-square&color=7ED321)](https://github.com/Valiumlove/surge-rules/commits)

</div>

---

## 📂 目录说明

| 文件夹 | 内容 |
|--------|------|
| `Rules/` | 分流规则集 |
| `Module/` | Surge 模块 `.sgmodule` |
| `JavaSesponse/` | JS 脚本 |
| `conf/` | 个人配置文件 |

---

## 🚀 使用方式

### 引用规则

```ini
[Rule]
RULE-SET,https://raw.githubusercontent.com/Valiumlove/surge-rules/main/Rules/xxx.list,PROXY
```

### 安装模块

Surge → 模块 → 安装外部模块，填入：

```
https://raw.githubusercontent.com/Valiumlove/surge-rules/main/Module/xxx.sgmodule
```

---

> 如遇 `raw.githubusercontent.com` 访问受阻，可将域名替换为 `cdn.jsdelivr.net/gh/Valiumlove/surge-rules@main`
