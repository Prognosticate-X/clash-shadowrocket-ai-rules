# Clash Verge & Shadowrocket AI 分流规则

> Clash Verge Rev 全局扩展覆写（Merge）+ Shadowrocket 配置：为 Siri / Apple Intelligence、Gemini、ChatGPT 等 AI 服务分流，内置地区节点自动匹配组与 9900+ 条国内外分流规则。
>
> **本仓库只包含规则与策略组，不含任何节点、订阅链接或凭证。**

`Clash Verge Rev` · `mihomo` · `Shadowrocket` · `CC BY-SA 4.0`

---

## 目录

- [文件说明](#文件说明)
- [特性](#特性)
- [快速开始](#快速开始)
- [分流设计](#分流设计)
- [规则统计](#规则统计)
- [苹果生态保活与 Apple Intelligence](#苹果生态保活与-apple-intelligence)
- [自定义指南](#自定义指南)
- [兼容性](#兼容性)
- [常见问题](#常见问题)
- [隐私说明](#隐私说明)
- [更新日志](#更新日志)
- [许可证 · 致谢 · 免责声明](#许可证--致谢--免责声明)

---

## 文件说明

| 文件 | 适用客户端 | 说明 |
| --- | --- | --- |
| `Merge.yaml` | Clash Verge Rev（mihomo 内核） | 「全局扩展覆写配置」，22 个策略组 + 9890 条规则 |
| `Shadowrocket CC.conf` | Shadowrocket（iOS） | 完整配置文件（`[General]` / `[Rule]` / `[Proxy Group]`），21 个策略组 + 9923 条规则 |

两份文件的分流意图一致，差异只在客户端语法与少量平台专有规则：

| 能力 | Clash / mihomo | Shadowrocket |
| --- | --- | --- |
| 地区组按节点名自动匹配 | `include-all-proxies: true` + `filter` | `policy-regex-filter=` |
| 地区组空组兜底 | `empty-fallback: DIRECT` | 在组成员里保留 `DIRECT` |
| 进程名分流（`PROCESS-NAME`） | 支持（6 条） | iOS 无法获取进程信息，不适用 |
| Gemini / Antigravity 独立策略组 | 有（18 条规则） | 无，走「🔰 选择节点」兜底 |

> 文件名可按自己习惯修改，不影响使用；若在仓库里重命名了配置文件，记得同步修改本 README 中出现的文件名。

---

## 特性

- **地区节点自动匹配**：香港 / 日本 / 美国 / 新加坡 / 台湾 / 英国 / 韩国 / 德国 / 低价区，按节点名正则自动归组，**换机场不用改配置**
- **AI 服务定向分流**：Siri & Apple Intelligence、Gemini / Antigravity、ChatGPT / Claude 等各自独立策略组
- **防「直连跑赢节点」**：地区组统一用 `http://www.gstatic.com/generate_204` 测速，国内直连必然失败，不会被自动选中
- **苹果生态保活**：APNs 推送、iMessage、日历 / 通讯录 / 提醒、iCloud 邮件全部走直连，防断流
- **国内外智能分流**：国内域名 / IP 直连，`GEOIP,CN` 兜底，国外流量走代理
- **广告拦截**：1615 条广告 / 追踪域名 `REJECT`
- **QUIC 阻断**：YouTube / Instagram 等 UDP 443 走 REJECT，避免代理 UoT 不稳
- **零凭证**：仓库内不含任何节点、订阅或账号信息

---

## 快速开始

### Clash Verge Rev

1. 打开 Clash Verge Rev → **订阅** 页 → 底部 **全局扩展覆写配置** → 编辑
2. 把 `Merge.yaml` 全文粘贴进去并保存（或直接导入该文件）
3. 回到订阅卡片，点 **重新激活订阅**（或先「刷新」拉取订阅）
4. 首页开启 **TUN 模式** 或 **系统代理** —— 二者都不开时，流量不会经过 Clash
5. 进 **代理** 页，把 `🔰 选择节点` 选成你想要的国家组（例如 `🇺🇲 美国节点`），地区组会自动测速选最快节点

> ⚠️ 覆写文件里的 `proxy-groups` 与 `rules` 会**整体替换**订阅自带的对应内容（实测行为），订阅原有的策略组不会保留。节点列表本身仍来自你自己的订阅，所以换任何一个订阅都能直接用。

### Shadowrocket（iOS）

1. 把 `Shadowrocket CC.conf` 传到手机（AirDrop / 文件 App / 剪贴板粘贴）
2. Shadowrocket → **配置** → 添加配置 → 选择该 `.conf` 文件
3. 选中该配置使其生效（文件内没有 `[Proxy]` 段，节点使用你当前订阅里的节点）
4. 首页开启代理，策略组里选 `🔰 选择节点` 与对应国家组

---

## 分流设计

### 1. 地区自动匹配组

节点归组完全靠**节点名正则**，只要订阅里的节点名包含对应关键词就会自动进组。

| 策略组 | 类型 | 匹配关键词（正则） |
| --- | --- | --- |
| 🇭🇰 香港节点 | url-test | `港` / `HK` / `Hong Kong` |
| 🇯🇵 日本节点 | url-test | `日本` / `JP` / `Japan` / `东京` / `Tokyo` / `大阪` / `Osaka` |
| 🇺🇲 美国节点 | url-test | `美国` / `US` / `USA` / `United States` / `洛杉矶` / `圣何塞` / `硅谷` / `西雅图` / `达拉斯` / `芝加哥` |
| 🇸🇬 新加坡节点 | url-test | `新加坡` / `狮城` / `SG` / `Singapore` |
| 🇨🇳 台湾节点 | url-test | `台湾` / `台灣` / `臺灣` / `TW` / `Taiwan` |
| 🇬🇧 英国节点 | url-test | `英国` / `英國` / `UK` / `United Kingdom` / `伦敦` |
| 🇰🇷 韩国节点 | url-test | `韩`（一次覆盖 `韩` / `韩国` / `韓國` / `韩國`）/ `KR` / `Korea` / `首尔` |
| 🇩🇪 德国节点 | url-test | `德国` / `德國` / `DE` / `Germany` / `法兰克福` |
| 🇦🇷 低价区节点 | select | `阿根廷` / `土耳其` / `印度` / `俄罗斯` / `乌克兰` 及对应英文与二字码 |

> 表中关键词是两份文件的合集，个别繁体写法两边略有差异（例如 Clash 端台湾组为 `台湾|台灣|TW|Taiwan`，Shadowrocket 端额外收录了 `臺灣`；英国 / 德国组同理）。

三个关键细节：

1. **Clash 端必须写 `include-all-proxies: true`**。mihomo 的 `filter` 默认只作用于 proxy-provider（`use`）提供的节点；只写 `filter` 而不写 `include-all-proxies`，组里会只剩 `DIRECT` 一个成员 —— 表现为"所有地区组都显示 DIRECT、外网全打不开"。这是本配置踩过并修复的坑。
2. **测速地址是 gstatic 的 204**。用 `cp.cloudflare.com` 之类的地址时，国内直连往往也能通过测速，url-test 会把 `DIRECT` 选成最快，等于绕过代理。
3. **空组兜底**。Clash 用 `empty-fallback: DIRECT`（仅在正则一条都没匹配到时生效，不会参与测速）；Shadowrocket 没有该机制，所以在组成员里保留 `DIRECT` 兜底 —— 因为测速地址在国内被墙，正常不会选中它。

### 2. 上层策略组

| 策略组 | 默认出口 | 用途 | 规则条数（Clash 端） |
| --- | --- | --- | ---: |
| 🔰 选择节点 | 🇭🇰 香港节点 | 总出口，被绝大多数规则引用 | 7311 |
| 🤖 Siri & Apple AI | 🔰 选择节点 | Siri、Apple Intelligence、Private Cloud Compute、Apple 服务 | 46 |
| ✨ Gemini & Antigravity | 🔰 选择节点 | Gemini、AI Studio、Antigravity（仅 Clash 端） | 18 |
| 🌏 爱奇艺&哔哩哔哩 | DIRECT | 港澳台流媒体 | 24 |
| 📺 动画疯 | 🔰 选择节点 | 动画疯 | 5 |
| 🎮 Steam 登录/下载 | DIRECT | Steam 下载走直连 | 2 |
| 🎮 Steam 商店/社区 | 🔰 选择节点 | Steam 商店走代理 | 19 |
| 🌩️ Cloudflare | 🔰 选择节点 | Cloudflare 站点 | 36 |
| ☁️ OneDrive | 🔰 选择节点 | OneDrive 同步 | 14 |
| 🎓学术网站 | DIRECT | 学术站点（默认直连，需要时可切代理） | 17 |
| 🇨🇳 国内网站 | DIRECT | 国内域名与 `GEOIP,CN` | 706 |
| 🛑 拦截广告 | REJECT | 广告 / 追踪域名 | 1615 |
| 🐟 漏网之鱼 | 🔰 选择节点 | 兜底（`MATCH` / `FINAL`） | 2 |

### 3. 规则优先级

规则自上而下匹配，越靠前越优先，大致顺序为：

```
Appstorrent 指定新加坡  →  苹果推送 / APNs 直连  →  Apple 日历 · 通讯录 · iCloud 邮件直连
  →  Gmail / Outlook 走代理  →  进程名分流（Siri / Antigravity）
  →  Gemini · Antigravity  →  Apple Intelligence 专用域名
  →  B 站 / 动画疯 / Steam / OneDrive 等专项
  →  域名与 IP 分流表（广告拦截、学术、Facebook / Telegram / Twitter 等 IP 段）
  →  GEOIP,CN 国内直连  →  MATCH 兜底
```

需要把某个域名绑到指定组时，把它加在**最前面**即可覆盖后面的规则。

---

## 规则统计

| 项目 | Merge.yaml（Clash） | Shadowrocket CC.conf |
| --- | ---: | ---: |
| 策略组 | 22 | 21 |
| 规则总数 | 9890 | 9923 |
| 文件大小 | 479 KB / 10107 行 | 459 KB / 9972 行 |

**Clash 端规则类型分布**：`DOMAIN-SUFFIX` 9065 · `IP-CIDR` 489 · `DOMAIN` 168 · `DOMAIN-KEYWORD` 138 · `IP-CIDR6` 17 · `PROCESS-NAME` 6 · `DST-PORT` 4 · `AND` 1 · `GEOIP` 1 · `MATCH` 1

---

## 苹果生态保活与 Apple Intelligence

### 保活直连（防断流 / 防推送延迟）

- APNs：`push.apple.com`、`push-apple.com.akadns.net`、`ess.apple.com`、`DST-PORT,5223`
- Apple 网段：`17.0.0.0/8`（同时写入 `tun.route-exclude-address`）
- 日历 / 通讯录 / 提醒：`caldav.icloud.com`、`calendars.icloud.com`、`contacts.icloud.com`、`reminders.icloud.com`
- iCloud 邮件：`mail.me.com`、`me.com`、`imap.mail.me.com`、`smtp.mail.me.com`
- Gmail / Outlook：走代理（`imap.gmail.com`、`smtp.gmail.com`、`outlook.office365.com` 等）

### Apple Intelligence / Siri（`🤖 Siri & Apple AI` 组）

共 30 个域名 / 规则指向该组，覆盖 Siri 与 Private Cloud Compute 的关键端点，例如：

`gateway.icloud.com`、`p162-acsegateway.icloud.com`、`attester.gateway.icloud.com`、`tis.gateway.icloud.com`、`mask.icloud.com`、`mask-h2.icloud.com`、`mask-api.icloud.com`、`mask*.apple-dns.net`、`apple-relay.apple.com`、`apple-relay.cloudflare.com`、`apple-relay.fastly-edge.com`、`apple-relay.akamaized.net`、`guzzoni.apple.com`、`siri.apple.com`、`pcc.apple.com`、`api.smoot.apple.com`、`api.apple-cloudkit.com`、`cdn.apple-cloudkit.com`、`icloud-content.com`、`gspe1-ssl.ls.apple.com`、`gspe35-ssl.ls.apple.com`、`ls.apple.com`、`fides.apple.com`、`playgrounds-cdn.apple.com`、`apple-sub-services.apple.com`、`cp4.cloudflare.com`

实测定向结果（出口节点 = 美国）：

| 域名 | 命中规则 | 出口 |
| --- | --- | --- |
| `gateway.icloud.com` | `DOMAIN-SUFFIX,gateway.icloud.com` | 🤖 Siri & Apple AI → 美国节点 |
| `mask.icloud.com` / `mask-api.icloud.com` | `DOMAIN-SUFFIX,mask*.icloud.com` | 🤖 Siri & Apple AI → 美国节点 |
| `apple-relay.*` | `DOMAIN-SUFFIX,apple-relay.*` | 🤖 Siri & Apple AI → 美国节点 |
| `api.smoot.apple.com`、`guzzoni.apple.com`、`pcc.apple.com` | 精确 `DOMAIN` | 🤖 Siri & Apple AI → 美国节点 |
| `acsegateway.icloud.com`、`ropes.apple.com`、`experiments.apple.com`、`configuration.apple.com` | 落到 `apple.com` / `icloud.com` 后缀 | 🇨🇳 国内网站 → DIRECT（实测国内可达） |

说明：另有少量域名（`intelligence.apple.com`、`app-attest.apple.com`、`privatecloudcompute.apple.com`、`issuer.apple.com` 等）在公网 DNS 中没有 A 记录，属于防御性条目，留着不影响性能。

> 提示：要让这套规则真正生效，必须开启 **TUN 模式** 或 **系统代理**；两者都关闭时，系统流量不经过 Clash，规则再全也不会被命中。

---

## 自定义指南

**换一个地区的关键词** —— 改对应组的 `filter`（Clash）/ `policy-regex-filter=`（Shadowrocket）即可，例如台湾组加上 `臺灣`。

**新增一个地区组**（Clash 端模板）：

```yaml
- name: 🇫🇷 法国节点
  type: url-test
  url: http://www.gstatic.com/generate_204
  interval: 600
  tolerance: 50
  include-all-proxies: true
  empty-fallback: DIRECT
  filter: "(?i)(法国|法國|FR|France|巴黎|Paris)"
```

记得同时把它加进 `🔰 选择节点` 的 `proxies` 列表，否则组里选不到。

**把某个域名固定走某条线路**：在规则最顶部加一条，例如让 `chatgpt.com` 固定走美国组：

```yaml
- DOMAIN-SUFFIX,chatgpt.com,🇺🇲 美国节点
```

**改默认出口**：每个 select 组的第一项就是默认值，例如把 `🔰 选择节点` 的第一项从 `🇭🇰 香港节点` 改成 `🇺🇲 美国节点`。也能在客户端 UI 里直接选择，`profile.store-selected: true` 会记住选择。

---

## 兼容性

| 客户端 / 内核 | 支持情况 | 说明 |
| --- | --- | --- |
| Clash Verge Rev + verge-mihomo ≥ 1.19 | ✅ 推荐 | 本配置在 verge-mihomo `v1.19.31` 实测通过 |
| 其他 mihomo 内核（FlClash、ClashMeta for Android 等） | ✅ | 只要内核支持 `include-all-proxies`、`empty-fallback` |
| Shadowrocket（iOS） | ✅ | 使用 `.conf`，依赖 `policy-regex-filter` |
| Clash Premium / Clash for Windows / ClashX 等旧内核 | ❌ | 不支持 `include-all-proxies` / `empty-fallback`，地区组会退化成只剩 `DIRECT` |

---

## 常见问题

**Q1：地区组显示 `DIRECT`（或出现奇怪的 `COMPATIBLE`）？**
A：两种原因。① 内核不支持 `include-all-proxies`（旧内核）；② 节点名没有命中正则，此时 Clash 会走 `empty-fallback: DIRECT`，而没有该字段时 mihomo 会塞一个隐藏的 `COMPATIBLE`（实测它等价于直连）。解决办法：确认内核版本，或检查订阅里的节点名是否包含对应关键词。

**Q2：Google / YouTube / Gemini 打不开？**
A：按顺序检查 ① 是否开启了 **TUN 模式** 或 **系统代理**；② `🔰 选择节点` 是否被选成了 `DIRECT`；③ 地区组里是否有可用节点（打开代理页看延迟）；④ 节点本身能否访问目标站点。

**Q3：某个地区组里没有节点？**
A：该地区没有改名匹配的节点，把节点名或正则对齐即可。这是设计使然（换机场零修改的前提是节点名含地区关键词）。

**Q4：韩国节点筛选失败？**
A：中文节点名常见简繁混写（`韩國`）。本配置的正则已用 `韩` 覆盖 `韩国 / 韓國 / 韩國`，若你自定义过正则，请注意这一点。

**Q5：`chatgpt.com` 返回 403？**
A：这是 Cloudflare 对该出口 IP 的风控，不是配置问题（`api.openai.com` 通常返回 401，说明链路正常）。换一个地区的出口（如日本 / 美国）通常可解决。

**Q6：规则会不会影响国内 App（微信、支付宝、国内视频）？**
A：不会。国内域名有直连规则，未命中的域名由 `GEOIP,CN` 兜底走直连；`skip-proxy` / `bypass-tun` 也放行了局域网与内网网段。

---

## 隐私说明

- 仓库内所有文件**不含**节点地址、端口、密码、UUID、订阅链接、订阅域名或账号信息
- 节点始终由使用者自己的订阅提供，本仓库只负责"怎么分流"
- 建议不要把自己的订阅信息、节点配置提交到任何公开仓库

---

## 更新日志

### 2026-09-24

- 修复地区自动匹配组：补上 `include-all-proxies: true`（此前 `filter` 不生效，所有地区组只剩 `DIRECT`）
- 地区组测速地址由 `cp.cloudflare.com` 改为 `www.gstatic.com/generate_204`，避免直连被自动选中
- Clash 端新增 `empty-fallback: DIRECT`；Shadowrocket 端保留 `DIRECT` 兜底
- 韩国组正则兼容简繁混写（`韩 / 韩国 / 韓國 / 韩國`），台湾 / 英国 / 德国组补充繁体写法
- 核查 Apple Intelligence / Siri 定向分流（30 个域名 → `🤖 Siri & Apple AI`）

---

## 许可证 · 致谢 · 免责声明

### 许可证

本仓库内容采用 **CC BY-SA 4.0**（Creative Commons Attribution-ShareAlike 4.0 International）许可协议。
完整协议文本见仓库根目录的 `LICENSE` 文件，或访问 <https://creativecommons.org/licenses/by-sa/4.0/>。

署名要求：转载或基于本仓库二次修改时，请保留作者署名与本仓库链接，并以相同方式共享。

### 致谢

规则与分组整理自中文社区公开规则集，版权归原作者所有，并遵循其原始许可：

- [ACL4SSR](https://github.com/ACL4SSR/ACL4SSR) — CC BY-SA 4.0
- [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) — GPL-2.0
- [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev) — GPL-3.0（覆写配置模板）
- [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo) — MIT（内核）

### 免责声明

本仓库仅包含分流规则与配置模板，**不提供任何节点、订阅、账号或服务**。
所有内容仅供学习交流与技术研究使用，使用前请确认符合你所在地区的法律法规以及相关服务的使用条款。
因使用本配置产生的任何后果由使用者自行承担。
