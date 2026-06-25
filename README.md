<div align="center">

## ⚡ Surge

**个人专属的 Surge 分流规则 · 模块 · 脚本 · 配置集合**

[![GitHub Stars](https://img.shields.io/github/stars/Valiumlove/surge-rules?style=flat-square&logo=github&color=FFD700)](https://github.com/Valiumlove/surge-rules/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/Valiumlove/surge-rules?style=flat-square&logo=github&color=4A90E2)](https://github.com/Valiumlove/surge-rules/network)
[![Last Commit](https://img.shields.io/github/last-commit/Valiumlove/surge-rules?style=flat-square&logo=github&color=7ED321)](https://github.com/Valiumlove/surge-rules/commits)
[![License](https://img.shields.io/github/license/Valiumlove/surge-rules?style=flat-square&color=FF6B6B)](./LICENSE)

_适用于 [Surge for iOS](https://nssurge.com) & [Surge for Mac](https://nssurge.com)_

---

[📋 规则集](#-规则集) · [🧩 模块](#-模块) · [📜 脚本](#-脚本) · [⚙️ 配置](#️-配置) · [🚀 使用方法](#-使用方法) · [❓ FAQ](#-faq)

</div>

---

## ✨ 项目简介

> 本仓库收录了个人维护的 **Surge 分流规则、功能模块、实用脚本及完整配置**，旨在提供更精准的流量分流策略与更丰富的网络增强功能。

- 🎯 **精准分流** — 覆盖主流服务，国内直连 / 海外代理自动匹配
- 🧩 **功能模块** — 开箱即用，一键导入 Surge 增强体验
- 📜 **实用脚本** — 定时任务、重写脚本、MitM 解密
- ⚙️ **完整配置** — 提供可直接使用的 Surge 配置模板
- 🔄 **持续更新** — 规则随时跟进，保持最新有效

---

## 📋 规则集

### 🌐 分流规则 (Rule Set)

| 文件名 | 说明 | 策略建议 |
|--------|------|----------|
| `direct.txt` | 国内直连域名列表 | `DIRECT` |
| `proxy.txt` | 代理域名列表 | `PROXY` |
| `reject.txt` | 广告/隐私追踪屏蔽 | `REJECT` |
| `apple.txt` | Apple 国内可直连域名 | `DIRECT` |
| `icloud.txt` | iCloud 国内可直连域名 | `DIRECT` |
| `google.txt` | Google 国内可直连域名 ⚠️ | `DIRECT` |
| `private.txt` | 私有网络专用域名 | `DIRECT` |
| `cncidr.txt` | 中国大陆 IP 段 | `DIRECT` |
| `telegramcidr.txt` | Telegram IP 段 | `PROXY` |

> ⚠️ `google.txt` 慎用，仅包含在中国大陆极少数情况可直连的 Google 域名

### 📡 使用方式

在 Surge 配置的 `[Rule]` 段中引用（以 DOMAIN-SET 为例）：

```ini
[Rule]
# 🚫 广告拦截
DOMAIN-SET,https://raw.githubusercontent.com/Valiumlove/surge-rules/release/reject.txt,REJECT

# 🍎 Apple 直连
DOMAIN-SET,https://raw.githubusercontent.com/Valiumlove/surge-rules/release/apple.txt,DIRECT

# 🇨🇳 国内直连
DOMAIN-SET,https://raw.githubusercontent.com/Valiumlove/surge-rules/release/direct.txt,DIRECT

# 🌍 代理列表
DOMAIN-SET,https://raw.githubusercontent.com/Valiumlove/surge-rules/release/proxy.txt,PROXY

# 最终规则
GEOIP,CN,DIRECT
FINAL,PROXY,dns-failed
```

---

## 🧩 模块

模块可在 Surge → 模块 → 安装外部模块 中导入。

| 模块名 | 功能描述 | 安装链接 |
|--------|----------|----------|
| `AdBlock.sgmodule` | 去广告增强模块 | [点击安装](https://raw.githubusercontent.com/Valiumlove/surge-rules/main/modules/AdBlock.sgmodule) |
| `HeaderRewrite.sgmodule` | HTTP 头部重写 | [点击安装](https://raw.githubusercontent.com/Valiumlove/surge-rules/main/modules/HeaderRewrite.sgmodule) |
| `DNSOverHTTPS.sgmodule` | DoH 加密 DNS | [点击安装](https://raw.githubusercontent.com/Valiumlove/surge-rules/main/modules/DNSOverHTTPS.sgmodule) |
| `MitM.sgmodule` | HTTPS 解密模块 | [点击安装](https://raw.githubusercontent.com/Valiumlove/surge-rules/main/modules/MitM.sgmodule) |

> 💡 可在 Surge → 模块 界面中批量管理、启用/停用各模块

---

## 📜 脚本

### 脚本目录结构

```
scripts/
├── rewrite/          # 重写脚本 (URL Rewrite / Body Rewrite)
│   ├── adblock.js
│   └── mock.js
├── cron/             # 定时任务脚本
│   ├── notify.js
│   └── checkin.js
└── mitm/             # MitM 解密脚本
    └── decrypt.js
```

### 在配置中引用脚本

```ini
[Script]
# 定时签到示例
checkin = type=cron,cronexp="0 9 * * *",script-path=https://raw.githubusercontent.com/Valiumlove/surge-rules/main/scripts/cron/checkin.js

# 重写脚本示例
adblock = type=http-response,pattern=^https?://ads\.example\.com,script-path=https://raw.githubusercontent.com/Valiumlove/surge-rules/main/scripts/rewrite/adblock.js,requires-body=1
```

---

## ⚙️ 配置

### 模板说明

| 配置文件 | 适用场景 |
|----------|----------|
| `config_whitelist.conf` | 🌐 白名单模式（未命中规则走代理） |
| `config_blacklist.conf` | 🛡️ 黑名单模式（未命中规则走直连） |
| `config_minimal.conf` | ⚡ 极简配置（适合新手上手） |

### 白名单 vs 黑名单

```
🌐 白名单模式  ——  FINAL → PROXY
   适合：代理服务器稳定、流量充足、追求全局最优速度

🛡️ 黑名单模式  ——  FINAL → DIRECT  
   适合：代理流量有限、仅需突破特定封锁的用户
```

---

## 🚀 使用方法

### 方式一：直接导入配置

1. 打开 **Surge** → 配置 → 从 URL 下载配置
2. 输入配置文件 URL：
   ```
   https://raw.githubusercontent.com/Valiumlove/surge-rules/main/config_whitelist.conf
   ```

### 方式二：手动引用规则

将规则 URL 添加至已有配置的 `[Rule]` 段，参考上方「规则集 · 使用方式」。

### 方式三：导入模块

1. Surge → 首页 → 模块 → 安装外部模块
2. 粘贴模块 URL 即可

### 加速访问（CDN 备用地址）

若访问 `raw.githubusercontent.com` 受限，可替换为 jsDelivr CDN（更新延迟约 12 小时）：

```
# 原始地址
https://raw.githubusercontent.com/Valiumlove/surge-rules/release/reject.txt

# CDN 备用
https://cdn.jsdelivr.net/gh/Valiumlove/surge-rules@release/reject.txt
```

---

## ❓ FAQ

<details>
<summary><b>Q：规则不生效怎么办？</b></summary>

1. 确认 Surge 版本支持 `DOMAIN-SET` / `RULE-SET`
2. 检查配置中是否存在对应的策略组名称（如 `PROXY`）
3. 尝试在 Surge 中手动刷新规则集缓存

</details>

<details>
<summary><b>Q：脚本提示权限不足？</b></summary>

部分脚本需要开启 MitM（HTTPS 解密），请确保：
- 已安装并信任 Surge CA 证书
- 相应域名已加入 MitM 主机名列表

</details>

<details>
<summary><b>Q：如何只使用部分规则？</b></summary>

规则集之间相互独立，可按需引用单个文件，无需全量导入。

</details>

<details>
<summary><b>Q：规则更新频率如何？</b></summary>

规则集不定期更新，建议在 Surge 中设置合理的缓存时间（推荐 `update-interval=86400`）。

</details>

---

## 📁 目录结构

```
surge-rules/
├── 📂 rules/              # 分流规则集
│   ├── direct.txt
│   ├── proxy.txt
│   ├── reject.txt
│   └── ...
├── 📂 modules/            # Surge 模块
│   └── *.sgmodule
├── 📂 scripts/            # JavaScript 脚本
│   ├── rewrite/
│   ├── cron/
│   └── mitm/
├── 📂 configs/            # 完整配置模板
│   ├── config_whitelist.conf
│   └── config_blacklist.conf
└── 📄 README.md
```

---

## 🙏 致谢

感谢以下优秀项目提供的数据与灵感：

- [Loyalsoldier/surge-rules](https://github.com/Loyalsoldier/surge-rules) — 规则集数据来源
- [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community) — 域名社区列表
- [felixonmars/dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list) — 中国域名数据
- [NSSurge](https://nssurge.com) — Surge 官方

---

<div align="center">

**如果本仓库对你有帮助，欢迎点个 ⭐ Star 支持！**

_Made with ❤️ by [Valiumlove](https://github.com/Valiumlove)_

</div>
