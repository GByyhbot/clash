# OpenClash 精简配置

本仓库只维护一套 OpenClash / Subconverter 配置，避免多个近似模板重复维护。

## 文件

- `clash.ini`：唯一配置入口。
- `list/`：`clash.ini` 使用的自定义规则。

## 使用

将下面的地址设为 Subconverter 或 OpenClash 的远程配置模板：

```text
https://raw.githubusercontent.com/GByyhbot/clash/main/clash.ini
```

自定义规则统一在 `list/` 中维护。规则从上到下匹配，修改时请保留末尾的 `FINAL` 规则。

原始配置思路可参考：[OpenClash 配置视频](https://www.youtube.com/watch?v=S2l_0g4EOHk&t=2s)。
