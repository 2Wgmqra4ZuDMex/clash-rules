# 我的精简分流规则

适用于 Mihomo / ShellCrash：国内直连，国外代理，AI 和国际流媒体独立选节点。

## 面板只需要看这几个组

- **默认代理**：普通国外网站与强制代理名单的出口。
- **AI**：国外 AI 服务的出口，可以固定节点。
- **流媒体**：YouTube、Netflix 和国际流媒体的出口。
- **自动选择**：按延迟自动选节点，供其他组选择。

AI、流媒体可选“默认代理”或直接选订阅节点。规则分类不代表节点已通过服务解锁测试。

## 平时怎么改

在 GitHub 打开文件，点击编辑，提交即可。网关每小时更新自定义名单；也可在面板的规则集合页面手动刷新。

| 文件 | 用途 |
| --- | --- |
| `rules/proxy.yaml` | 指定网站强制代理 |
| `rules/direct.yaml` | 指定网站强制直连 |
| `rules/ai.yaml` | 补充需要走 AI 组的网站 |
| `rules/media.yaml` | 补充需要走流媒体组的网站 |

例如把某个网站加入名单：

```yaml
payload:
  - '+.example.com'
  - 'api.example.net'
```

`+.example.com` 匹配主域名及全部子域名。不写 `https://`、端口或网页路径。空名单写 `payload: []`。

## 匹配顺序

1. 局域网与私有地址直连。
2. 自定义强制代理（同名冲突时优先）。
3. 自定义强制直连。
4. 网关本地保留的网站例外。
5. AI。
6. YouTube、Netflix、国际流媒体。
7. 已知国外域名代理。
8. 国内域名、国内 IP 直连。
9. 其他流量默认代理。

基础分类沿用 DustinWin/ruleset_geodata，定期更新；国内外归类依赖规则数据，误判时用自定义名单修正。

## 配置文件

`routing.yaml` 是分流片段，包含策略组、规则集合和匹配顺序，**不是能直接启动的完整配置**。订阅、节点凭据、DNS、监听端口与透明代理配置由网关本地管理。

初次部署时会在网关合并此片段并通过内核检查。修改 `routing.yaml` 的策略结构需要重新合并部署；四个 `rules/*.yaml` 名单则可以自动更新。

公开仓库不包含订阅链接、节点密码、SSH 凭据或 GitHub 令牌。

参考：[Mihomo 规则集合](https://wiki.metacubex.one/config/rule-providers/)、[策略组](https://wiki.metacubex.one/config/proxy-groups/)。
