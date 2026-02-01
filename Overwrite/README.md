🎭 Overwrite 覆写配置中心

> 💬 欢迎使用 [Issue](../../issues) 提出建议和问题！

🔄 自动同步：每日北京时间 08:30 更新

🧩 适配建议：OpenClash v0.46.001 及以上（THEOPENCLASH部分）

---

📖 目录

- [📌 使用建议](#-使用建议)
- [🧩 THEINI · 订阅转换配置](#-theini--订阅转换配置)
  - [什么是 INI](#什么是-ini)
  - [三大分类](#三大分类)
  - [使用方法](#使用方法)
- [⚙️ THEOPENCLASH · 覆写模块](#️-theopenclash--覆写模块)
  - [适用场景](#适用场景)
  - [配置说明](#配置说明)
  - [环境变量设置](#环境变量设置)
- [📂 项目来源](#-项目来源)

---

📌 使用建议

- THEINI 和 THEOPENCLASH 分别服务于不同的使用场景，请根据您的客户端类型选择：
  - 使用 订阅转换网站/工具 → 选择 [THEINI](#-theini--订阅转换配置)
  - 使用 OpenClash 插件 → 选择 [THEOPENCLASH](#️-theopenclash--覆写模块)
- 所有配置文件均使用 GitHub Raw 直链，请确保您的设备可正常访问 GitHub 或配置相应的代理。

---

🧩 THEINI · 订阅转换配置

适用于各类基于 [SubConverter](https://github.com/tindy2013/subconverter) 开发的订阅转换服务（如 ACL4SSR、Sub-Web 等在线转换工具）。

什么是 INI

INI（Initial Configuration）是 SubConverter 的远程配置格式，定义了分流规则、策略组、节点重命名等规则。引用本目录的 INI 文件，可将机场原始订阅自动转换为完整可用的 Clash/Mihomo 配置。

三大分类

分类目录	适用场景	特点描述	
[ACL4Category](./THEINI/ACL4Category/)	规则洁癖用户	基于 ACL4SSR 经典规则体系，精细化分流，适合对规则质量有极高要求的用户	
[Airport](./THEINI/Airport/)	特定机场用户	收录 jklolixxs 等机场专属定制方案，针对特定机场节点优化分组逻辑	
[Ordinary](./THEINI/Ordinary/)	通用场景	ShellCrash、DustinWin 等社区主流方案，适配大多数使用场景，开箱即用	

使用方法

1. 获取 Raw 链接：

   进入上方分类目录，选择所需的 `.ini` 文件，点击 Raw 按钮复制链接。

2. 填入订阅转换平台：

   在订阅转换网站的 远程配置 (Remote Config) 栏填入上述链接，订阅链接 栏填入您的机场订阅。

3. 生成配置：

   选择目标格式（Clash / Mihomo），点击生成即可。

> 💡 提示：建议使用 Raw 直链而非下载文件，以确保每日自动同步的最新规则生效。

---

⚙️ THEOPENCLASH · 覆写模块

专为 OpenClash 插件 设计的覆写配置文件（`.conf`），支持自动注入多订阅源与定时更新。

适用场景

- 主路由/软路由用户：直接通过 OpenClash 插件一键导入，无需手动下载 YAML
- 多机场/多订阅用户：支持 `EN_KEY1`, `EN_KEY2`, `EN_KEY3` 批量注入多个 `proxy-providers`
- 自动化维护：内置 Cron 定时任务，每日自动更新配置

配置说明

分类目录	对应内核	说明	
[General_Config](./THEOPENCLASH/General_Config/)	标准 Mihomo/Meta	通用进阶配置的覆写脚本，适合日常使用	
[Smart_Mode](./THEOPENCLASH/Smart_Mode/)	Smart 内核	SmartDNS/软路由专用，需启用 Smart 内核	
[Mobile_Modules](./THEOPENCLASH/Mobile_Modules/)	标准内核	Android 模块场景的覆写脚本	

使用方法

1️⃣ 新增覆写模块

进入 OpenClash → 配置订阅 → 添加：

- 配置名称：自定义（如 "Henry-Smart"）
- 订阅方式：`http`
- 订阅地址：选择对应分类下的 `.conf` 文件 Raw 链接

示例链接：

```bash
https://raw.githubusercontent.com/HenryChiao/mihomo_yamls/main/Overwrite/THEOPENCLASH/Smart_Mode/HenryChiao/MihomoSmartAIO.conf
```

2️⃣ 配置环境变量

在 OpenClash 覆写设置 或系统环境变量中添加：

```bash
EN_KEY1=https://你的机场订阅链接1;EN_KEY2=https://你的机场订阅链接2;EN_KEY3=https://你的机场订阅链接3
```

变量说明：

变量名	必填	说明	
`EN_KEY1`	✅ 是	第一个订阅源（主机场）	
`EN_KEY2`	❌ 否	第二个订阅源（备用/分流）	
`EN_KEY3`	❌ 否	第三个订阅源（按配置需求）	

多订阅分隔：使用分号 `;` 分隔不同 Key，单个 Key 内如有特殊字符无需编码。

配置完成后：保存 → 点击 应用配置 或重启 OpenClash。

3️⃣ 工作原理

```text
OpenClash 启动
    ↓
下载 .conf 覆写脚本
    ↓
读取 EN_KEY1/2/3 环境变量
    ↓
自动替换 proxy-providers 中的 URL 占位符
    ↓
注入真实订阅链接
    ↓
启动 Mihomo 内核（自动生成完整配置）
```

---

💡 温馨提示

1. 网络连通性：本仓库文件均使用 GitHub Raw 直链，请确保路由器可正常访问 GitHub，或在 OpenClash 中配置 GitHub 加速。
2. GeoIP 数据库：初次使用若提示内核错误，通常是由于缺少 GeoIP 数据库，请手动更新 GeoIP Dat 或多次重启后自动恢复。
3. Smart 内核：使用 `THEOPENCLASH/Smart_Mode/` 时，请确保 OpenClash 已切换至 Smart 内核，否则策略可能无法正常加载。

---

📂 项目来源

- THEYAMLS：原始配置文件同步来源（HenryChiao、666OS、JohnsonRan 等上游作者）
- THEINI upstream：ACL4SSR、ShellCrash、DustinWin、jklolixxs 等社区规则项目
- OpenClash：Vernesong 开发的 OpenWrt 代理插件

---

✨ 如果本项目对您有帮助，请点个 ⭐ Star 支持！

配置问题欢迎 [Issue](../../issues) 交流，优质建议欢迎 PR！

🔙 [返回项目主页](../README.md)
