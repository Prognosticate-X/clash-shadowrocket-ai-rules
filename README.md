# clash-shadowrocket-ai-rules

实机使用环境：
MacBook Pro + MacOS 27.0 + Clash Verge V2.5.5
iPhone + iOS 27.0 + Shadowrocket

解决问题与实际使用效果：
Mac：日常使用Whatsapp或者Antigravity时需要开启TUN服务，但是开启TUN服务后部分国内服务如微信会遇到图片加载慢的问题
iPhone：使用Siri AI必须要开启Proxy模式，同样国内服务如微信会遇到加载慢的问题

Mac：使用这个配置文件后开启TUN服务并勾选RULE模式，能正常使用Whatsapp、Antigravity、Siri AI，且国内软件和iMessage同步、邮箱同步、日历同步等不受影响
iPhone：使用配置开启Config模式后，国内外软件正常使用无感，Siri AI常驻可用，视觉识别和ChatGPT扩展均正常使用

需要注意事项：
1.如果想要用Siri AI，首先你的帐号资质需要能够正常获取，这个配置只是为了正确连接海外网络
2.如果想要用Siri AI，使用的节点必须要支持UDP，Shadowrocket设置里也需要打开UDP支持（因为苹果的私有云计算需要UDP，不然会出现访问驳回
3.Clash Verge要打开IPV6支持

使用教程：
iOS：推荐下载轻量实测版 `SR Customized Config.conf`（如需多地区策略组可选 `Shadowrocket CC.conf`）。下载 conf 到 File.app 后，直接 Preview 打开，底部会有 Shadowrocket 的跳转按钮，点击后自动导入，然后点击配置文件，点 Use Config 即可使用，然后回到主界面选择机场与节点（使用 `Shadowrocket CC.conf` 时可点击右侧感叹号进入编辑页的 Proxy Group 手动选组），最后返回首页，Global Routing 选择 Config 打开 VPN 即可。
Mac：下载 `Merge.yaml`，打开 Clash Verge，在 Profiles 里面找到 Global Extend Config，右键点击 Edit file，将 yaml 里面的内容完全复制粘贴进去保存即可（覆盖不是追加），也可以前往 `/Users/[username]/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/profiles/` 替换 `Merge.yaml`。然后打开 TUN 即可。

最新更新（V3.0 / 2026-11-27）：
- 修复 Apple Maps 加载极慢及 Find My 地图无法加载的问题（`Merge.yaml` & `SR Customized Config.conf`）：移除宽泛的 `ls.apple.com`、`gspe35-ssl.ls.apple.com` 与 `apple-dns.net` 代理规则，仅精准保留 `gspe1-ssl.ls.apple.com` 和 `mask*.apple-dns.net` 走代理以保 Apple Intelligence，同时显式直连苹果地图与高德相关域名。
- `SR Customized Config.conf` 将 APNs（`push.apple.com` / `push-apple.com.akadns.net` / `DST-PORT,5223`）调整为走代理，以解决 Telegram / X 无法收到系统推送横幅的问题。

具体的文档参考仓库内的 `README BY AI.md`。
