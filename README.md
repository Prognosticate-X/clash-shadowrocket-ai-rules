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
iOS：下载conf到File.app后，直接Preview打开，底部会有shadowrocket的跳转按钮，点击后自动导入，然后点击配置文件，点Use Config即可使用，然后回到主界面选择机场与节点，（如果不确定是否生效规则，可以点击.conf右侧的感叹号进入编辑页面，然后找到Proxy Group，上面的节点组如美国节点 URL-TEST>...点进去就可以手动选择节点）然后返回首页，Global Routing选择Config打开VPN即可
Mac：下载yaml，打开Clash Verge，在Profiles里面找到Global Extend Config，右键点击Edit file，将yaml里面的内容完全复制粘贴进去保存即可（覆盖不是追加），也可以前往/Users/[username]/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/profiles/替换Merge.yaml。然后打开TUN即可。

具体的文档参考仓库内的README BY AI.md，写这个的时候我太困了，就连这个readme我都懒得用语法块了，等后面有时间了再更新吧...
