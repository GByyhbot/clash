# OpenClash 精简配置

本仓库只维护一套 OpenClash / Subconverter 配置，避免多个近似模板重复维护。

## 文件

- `clash.ini`：唯一配置入口。
- `list/`：`clash.ini` 使用的自定义规则。
- `list/mdc/`：MDC-NG 刮削数据源规则，每个站点独立分流。

## 使用

将下面的地址设为 Subconverter 或 OpenClash 的远程配置模板：

```text
https://raw.githubusercontent.com/GByyhbot/clash/main/clash.ini
```

自定义规则统一在 `list/` 中维护。所有运行时规则均已收录到本仓库，配置不再直接下载其他规则仓库的文件。规则从上到下匹配，修改时请保留末尾的 `FINAL` 规则。

## MDC-NG 分流

MDC-NG 数据源按节点要求合并为五组：`刮削-日本`、`刮削-非日`、`刮削-港台`、`刮削-美国` 和 `刮削-通用`。DMM、MGStage、HBox、FC2、Caribbean 和日亚海报使用日本组；JavDB 单独使用不包含日本节点的非日组；AirAV、7MMTV、Madou 使用港台组；ThePornDB 使用美国组；其余无强制地区要求的站点按延迟使用通用组。

GFriends 使用 GitHub 域名，因此沿用现有 `GitHub` 策略组；OpenAI 和 Google 翻译分别沿用现有 `ChatGPT` 和 `Google` 策略组，避免重复或互相覆盖。

对于 JavDB、MissAV、AVSox 等经常更换域名的数据源，规则使用站点名称关键字匹配；稳定站点使用域名后缀匹配。MDC-NG 中未启用的数据源不会产生流量，因此保留其规则不会影响日常连接。

## 规则来源

- 服务分流规则整理自 [`blackmatrix7/ios_rule_script`](https://github.com/blackmatrix7/ios_rule_script)，下载后已移除注释、去重并按本配置的策略组合并。
- `Game.list` 合并 Epic、EA、Blizzard、UBI、Sony 与 Nintendo。
- `Crypto.list` 合并 OKX、Bybit、Binance、Coinbase、Crypto.com、Kraken 与 TronLink。
- `Direct.list` 由个人直连补充规则与较精简的 China 规则合并；未采用体积过大的 ChinaMax。
- ChatGPT 规则另外参照 [OpenAI 官方网络建议](https://help.openai.com/en/articles/9247338)维护。

原始配置思路可参考：[OpenClash 配置视频](https://www.youtube.com/watch?v=S2l_0g4EOHk&t=2s)。
