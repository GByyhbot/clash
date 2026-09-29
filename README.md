# Mihomo / OpenClash 精简配置

本仓库只维护一套 Mihomo / OpenClash 规则与配置，避免多个近似模板重复维护。NAS 已独立运行 Sub-Store，Mihomo 只消费 Sub-Store 生成的聚合分享配置，不需要在 Mihomo 容器中重复安装 Sub-Store。

## 文件

- `clash.ini`：唯一配置入口。
- `mihomo.yaml`：Mihomo / FlClash 原生配置模板；部署时将 `__SUB_STORE_URL__` 替换为 NAS 上现有 Sub-Store 的聚合分享链接。
- `list/`：`clash.ini` 使用的自定义规则。
- `list/CustomDirect.list`：手工直连规则，优先级最高；用于修正仍被误判为国外的域名或 IP。
- `list/ChinaDomain.yaml`、`list/ChinaIP.yaml`：本地保存的中国域名和 IP 规则库。
- `list/mdc/`：MDC-NG 刮削数据源规则，每个站点独立分流。

## 使用

将下面的地址设为 Subconverter 或 OpenClash 的远程配置模板：

```text
https://raw.githubusercontent.com/GByyhbot/clash/main/clash.ini
```

自定义规则统一在 `list/` 中维护。所有运行时规则均已收录到本仓库，配置不再直接下载其他规则仓库的文件。规则从上到下匹配，修改时请保留末尾的 `FINAL` 规则。

安卓端建议使用 FlClash 导入 `mihomo.yaml`。模板不会保存私人订阅地址或令牌；手机无法访问 GitHub 时，可由 NAS 在局域网提供配置和安装包。

## NAS 当前部署

- NAS 地址：`192.168.31.95`。
- Sub-Store 已在 NAS 上运行，管理与分享服务端口为 `3001`。
- Mihomo 使用 Sub-Store 的“聚合”分享链接作为 `proxy-providers.聚合.url`，无需另外部署 Sub-Store。
- NAS 通过 `http://192.168.31.95:18088/` 向局域网设备提供配置、规则和客户端安装包。
- 安卓配置地址为 `http://192.168.31.95:18088/mihomo-cn.yaml`；其中规则提供器也使用 NAS 的 `18088` 本地地址，因此无代理环境下不需要访问 GitHub。
- Sub-Store 分享令牌属于私密信息，不写入公开仓库；只保存在 NAS 实际生成的配置中。

## 手工修正国内流量

无法识别的国内地址统一添加到 `list/CustomDirect.list`，每行一条且不要附带策略组。例如：

```text
DOMAIN-SUFFIX,example.cn
DOMAIN,www.example.com
DOMAIN-KEYWORD,example
IP-CIDR,203.0.113.10/32,no-resolve
IP-CIDR,203.0.113.0/24,no-resolve
IP-CIDR6,2001:db8::/32,no-resolve
```

优先使用 `DOMAIN-SUFFIX`；只有应用直接访问 IP、无法按域名匹配时才添加 IP。单个 IPv4 地址使用 `/32`，一段地址则填写实际 CIDR。修改后需要让客户端更新 `CustomDirect` 规则提供器或重新加载配置。

当前手工修正规则包括：`emby.oldchu.com` 精确直连，`yykx.top` 与 `kmexvuoz.cc` 整域直连。NAS 上的规则副本位于本地配置服务目录，仓库修改后需要同步该副本，手机端才能在不访问 GitHub 的情况下获取更新。

## MDC-NG 分流

MDC-NG 数据源按节点要求合并为五组：`刮削-日本`、`刮削-非日`、`刮削-港台`、`刮削-美国` 和 `刮削-通用`。DMM、MGStage、HBox、FC2、Caribbean 和日亚海报使用日本组；JavDB、MissAV 及其 `fourhoi.com` 相关资源使用不包含日本节点的非日组；AirAV、7MMTV、Madou 使用港台组；ThePornDB 与 A/V Entertainment 使用美国组；其余无强制地区要求的站点按延迟使用通用组。

GFriends 使用 GitHub 域名，因此沿用现有 `GitHub` 策略组；OpenAI 和 Google 翻译分别沿用现有 `ChatGPT` 和 `Google` 策略组，避免重复或互相覆盖。

对于 JavDB、MissAV、AVSox 等经常更换域名的数据源，规则使用站点名称关键字匹配；稳定站点使用域名后缀匹配。MissAV 的关键字规则位于 `MDC-NonJapan`，确保更换域名后仍不会误用日本节点。MDC-NG 中未启用的数据源不会产生流量，因此保留其规则不会影响日常连接。

## 规则来源

- 服务分流规则整理自 [`blackmatrix7/ios_rule_script`](https://github.com/blackmatrix7/ios_rule_script)，下载后已移除注释、去重并按本配置的策略组合并。
- `Game.list` 合并 Epic、EA、Blizzard、UBI、Sony 与 Nintendo。
- `Crypto.list` 合并 OKX、Bybit、Binance、Coinbase、Crypto.com、Kraken 与 TronLink。
- `Direct.list` 由个人直连补充规则与较精简的 China 规则合并；未采用体积过大的 ChinaMax。
- ChatGPT 规则另外参照 [OpenAI 官方网络建议](https://help.openai.com/en/articles/9247338)维护。

原始配置思路可参考：[OpenClash 配置视频](https://www.youtube.com/watch?v=S2l_0g4EOHk&t=2s)。
