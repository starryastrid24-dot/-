# LxBox 分流规则与两套 DNS

从天悦云原版规则提取，用于 LxBox 2.25.18 / sing-box-lx 1.14.4-lx.25。这里只发布规则匹配条件，不包含代理节点、订阅或私密配置。

## 两套 DNS

| 方案 | 国内直连 | Claude / Google / 其他 AI / Telegram、Instagram | 私网 |
| --- | --- | --- | --- |
| 分流版 | 阿里、腾讯 DoH 直连 | Google DoH，经对应代理方向 | 保留 Tailscale MagicDNS |
| 原版版 | 阿里、腾讯 DoH 直连 | 阿里、腾讯 DoH 直连 | 保留 Tailscale MagicDNS |

两版共用规则和稳定 DNS 标签。切换只替换本项目管理的 DNS 记录，不重新导入节点。候选记录和实际切换、验收结果分别保存；文件存在不代表平板已启用。

## 为什么文件不只有六个

源文件有 26,482 条规则，连续动作分成 20 段。相同域名有时前面直连、后面代理；直接合并为几个大集合会改变首条命中的结果。因此保留动作段顺序，在段内细分 Anthropic、Google、其他 AI、敏感流媒体、下载、国内域名和直连 IP。

`route-manifest.json` 给出所有文件的动作、分类、来源行号和哈希。JSON 可读，SRS 供 LxBox 下载。同动作内部分类方向是本次有意细化；这不是整份 Mihomo 配置的逐字等价转换。

## 特殊规则

- UDP 443 拒绝、LAN 和最终 MATCH 在应用层配置；SRS 不存储动作。
- 原 GEOIP,CN 对应 [MetaCubeX 的中国 IP 规则](https://github.com/MetaCubeX/meta-rules-dat/tree/sing/geo/geoip)，下载地址为 `https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geoip/cn.srs`。它是独立上游，不是天悦云原 IP 集的同版本证明。
- `no-resolve` 在 manifest 中保留，SRS 本身不能表达；已有入口解析会影响 IP 匹配时机。
- Android 包名与桌面进程名分别保留。桌面进程规则通常不会匹配 Android 应用。
- Anthropic 只按原件提取和静态核对，没有主动访问测试。
- Google FCM 是推送连接。LxBox 默认 FCM 预设会直连、且没有 DNS 规则，本次不直接启用这个绕过；原件已有的 mtalk 直连例外按原顺序保留。

## 来源和验证边界

原件 SHA256：`0a33ca6056de9fb6383c62b05cbefaa78172f9def814171ce545f47467aac6c5`。

86 个 SRS 已通过上述官方核心离线编译，逐条来源覆盖与产物哈希已核对。平板接入、缓存、运行配置和真实 DNS/出口验证仍需独立检查。详细本地研究与证据记录在 Personal Archive 的 `pa-8pi`，不公开设备快照。

官方实现参考：[LxBox Debug API](https://github.com/Leadaxe/LxBox/blob/v2.25.18/docs/api/debug-api-reference.md)、[SRS 下载实现](https://github.com/Leadaxe/LxBox/blob/v2.25.18/app/lib/services/rule_set_downloader.dart)、[规则生成](https://github.com/Leadaxe/LxBox/blob/v2.25.18/app/lib/services/builder/post_steps/custom_rules.dart)。
