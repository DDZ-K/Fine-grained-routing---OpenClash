# Fine-grained-routing---OpenClash

**OpenClash / Clash 细粒度分流配置** — 面向 OpenWrt OpenClash 的规则与策略组方案，按应用/地区精细选择节点。

> 原描述：OpenClash YAML 配置

## 仓库文件

| 文件 | 说明 |
|------|------|
| `yaml.ini` | 完整 Clash 配置示例（端口、代理集合、策略组、规则等） |
| `youtube.ini` | OpenClash **自定义规则生成**配置（`[custom]`，方案 B：应用层可选手动/自动） |

## 设计思路

1. **节点订阅**：通过 `proxy-providers` 拉取机场订阅（HTTP），定时更新，并 **过滤** 流量说明/套餐/防失联等非节点条目
2. **策略组细分**（示例）：
   - 🚀 手动选择 / ♻️ 自动选择（url-test）
   - 地区节点：港 / 美 / 日 / 新 / 台 / 韩 等
   - 应用向：即时通讯、社交媒体、GitHub、ChatGPT、AI 服务、TikTok、YouTube、Notion、Telegram、Adobe 等
   - 🎯 全球直连
3. **规则优先级**（`youtube.ini` 方案 B）：
   - 私网、国内直连、下载/BT 直连
   - 核心应用单独 ruleset
   - 代理补丁仅作兜底，避免越权抢走专用策略
4. 所有应用层策略组可在 **手动选择 / 自动选择** 间切换，方便临时切换线路

## 适用环境

- OpenWrt + **OpenClash**
- 或兼容 Clash Premium / Meta 的客户端（需按实际语法微调）

## 使用方法（OpenClash）

### A. 使用 `youtube.ini`（推荐作为「覆写/自定义规则」）

1. 打开 OpenClash → **配置文件订阅 / 配置生成**
2. 启用自定义规则（对应项：`enable_rule_generator` / `overwrite_original_rules`）
3. 将 `youtube.ini` 内容合并进 OpenClash 的自定义规则模板
4. 更新配置并重启 OpenClash
5. 在面板中为各策略组选择节点

文件头注释说明其为 **方案 B**：在方案 A 基础上，为应用层策略增加手动/自动选择。

### B. 使用 `yaml.ini` 作为完整配置参考

1. 复制 `yaml.ini` 到路由器配置目录（或导入为配置文件）
2. **务必修改**：
   - `proxy-providers` 的订阅 URL / token（换成你自己的）
   - 端口、`external-controller`、密钥等
3. 语法检查通过后启用

默认端口示例：

```text
mixed/http  : 7890
socks       : 7891
controller  : 127.0.0.1:9090
mode        : rule
```

## 安全提醒（重要）

- 仓库中的订阅链接、token **可能含有敏感信息**。
- **请尽快在机场侧重置订阅 token**，并改用私有配置，避免公网仓库泄露导致订阅被盗用。
- 本 README **故意不粘贴** 任何真实订阅地址或 token。
- 分享配置时请脱敏：`url: "https://YOUR_PROVIDER/?sid=***&token=***"`

## 自定义建议

1. 按自己常用 App 增删 `proxy-groups` 与 `ruleset`
2. `exclude` / `exclude-filter` 保持清洗「流量|套餐|到期…」等展示节点
3. `url-test` 的 `interval` / `tolerance` 可按网络质量调整
4. 国内直连依赖 GEOSITE/GEOIP 数据，请保证 OpenClash 的 Geo 数据库为较新版本

## 许可证

个人学习与自用配置；转发时请脱敏。
