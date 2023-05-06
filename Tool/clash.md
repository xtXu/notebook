# Clash Setting & Problem
#tool #clash
## IEEE Xplore
**Problem:** 校园网ip能直接登录IEEE Xplore，若通过代理则无法认证。  
**Fix:** 可通过`Parsers`功能在机场提供的配置基础上增加分流规则：
```yml
parsers: # array
  - url: #{{订阅链接}}
    yaml:
      prepend-rules:
        - DOMAIN,ieeexplore.ieee.org,DIRECT # rules最前面增加一个规则
```

## Web of Science
**Problem:** 校园网ip登录webofscience，若通过代理则无法认证。  
**Fix:** 可通过`Parsers`功能在机场提供的配置基础上增加分流规则：
```yml
parsers: # array
  - url: #{{订阅链接}}
    yaml:
      prepend-rules:
        - DOMAIN-KEYWORD,webofscience,DIRECT
        - DOMAIN-KEYWORD,webofknowledge,DIRECT
        - DOMAIN-KEYWORD,clarivate,DIRECT
```
## TUN Mode
**Problem:** 部分浏览器，如Google Chrome，无法连接。  
**Fix:** 关闭**安全DNS**。

**Problem:** TUN模式下`git clone`等失败，因为需要通过`ssh`访问22端口，但大多机场会屏蔽22端口的连接。可以通过设置规则使22端口的流量走直连，但直连github速度很慢。  
**Fix:** 根据github官方做法，[使用https端口进行ssh连接](https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port)。具体地，在`/etc/ssh/ssh_config`中添加设置：
```
Host github.com
	Hostname ssh.github.com
	Port 443
	User git
```

**Problem:** windows会向所有网卡发送DNS查询请求，因此就算设置了TUN模式，也会将DNS请求发送给物理网卡，导致DNS泄露。  
**Fix:** 组策略-计算机配置-管理模板-网络-DNS客户端-禁用智能多宿主名称解析-启用。
![](../Resources/clash_img_1.png)

**Problem:** DNS设置：nameserver, fallback, fake-ip, fake-ip-filter。在分流规则匹配时，若域名要和ip规则进行匹配，则需要查询DNS时，Clash会先向nameserver中的所有服务器发送请求，并接受第一个返回的ip，若该ip是国外ip，则再次通过fallback查询，以保证获得未被污染的dns。  
**Fix:**
```yml
dns:
  enhanced-mode: fake-ip
  nameserver:
    - 114.114.114.114
    - tls://223.5.5.5:853
    - tls://223.6.6.6:853
    - tls://120.53.53.53
    - tls://1.12.12.12
  fallback:
    - https://1.0.0.1/dns-query
    - https://public.dns.iij.jp/dns-query
    - tls://8.8.4.4:853
  fake-ip-filter:
    - "*.lan"
    - "*.localdomain"
    - "*.example"
    - "*.invalid"
    - "*.localhost"
    - "*.test"
    - "*.local"
    - "*.home.arpa"
    - time.*.com
    - time.*.gov
    - time.*.edu.cn
    - time.*.apple.com
    - time1.*.com
    - time2.*.com
    - time3.*.com
    - time4.*.com
    - time5.*.com
    - time6.*.com
    - time7.*.com
    - ntp.*.com
    - ntp1.*.com
    - ntp2.*.com
    - ntp3.*.com
    - ntp4.*.com
    - ntp5.*.com
    - ntp6.*.com
    - ntp7.*.com
    - "*.time.edu.cn"
    - "*.ntp.org.cn"
    - +.pool.ntp.org
    - time1.cloud.tencent.com
    - music.163.com
    - "*.music.163.com"
    - "*.126.net"
    - musicapi.taihe.com
    - music.taihe.com
    - songsearch.kugou.com
    - trackercdn.kugou.com
    - "*.kuwo.cn"
    - api-jooxtt.sanook.com
    - api.joox.com
    - joox.com
    - y.qq.com
    - "*.y.qq.com"
    - streamoc.music.tc.qq.com
    - mobileoc.music.tc.qq.com
    - isure.stream.qqmusic.qq.com
    - dl.stream.qqmusic.qq.com
    - aqqmusic.tc.qq.com
    - amobile.music.tc.qq.com
    - "*.xiami.com"
    - "*.music.migu.cn"
    - music.migu.cn
    - +.msftconnecttest.com
    - +.msftncsi.com
    - msftconnecttest.com
    - msftncsi.com
    - localhost.ptlogin2.qq.com
    - localhost.sec.qq.com
    - +.srv.nintendo.net
    - +.stun.playstation.net
    - xbox.*.microsoft.com
    - xnotify.xboxlive.com
    - +.ipv6.microsoft.com
    - +.battlenet.com.cn
    - +.wotgame.cn
    - +.wggames.cn
    - +.wowsgame.cn
    - +.wargaming.net
    - proxy.golang.org
    - stun.*.*
    - stun.*.*.*
    - +.stun.*.*
    - +.stun.*.*.*
    - +.stun.*.*.*.*
    - heartbeat.belkin.com
    - "*.linksys.com"
    - "*.linksyssmartwifi.com"
    - "*.router.asus.com"
    - mesu.apple.com
    - swscan.apple.com
    - swquery.apple.com
    - swdownload.apple.com
    - swcdn.apple.com
    - swdist.apple.com
    - lens.l.google.com
    - stun.l.google.com
    - "*.square-enix.com"
    - "*.finalfantasyxiv.com"
    - "*.ffxiv.com"
    - "*.ff14.sdo.com"
    - ff.dorado.sdo.com
    - "*.mcdn.bilivideo.cn"
    - +.media.dssott.com
    - +.pvp.net
```

   
