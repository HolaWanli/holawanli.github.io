---
title: "如何搭建家用 homelab: 硬件和架构"
description: 家庭设备架构和考虑因素是根据实际需求不断地演变、试错和总结
date: 2023-01-31T19:28:50+08:00
slug: "how-to-homelab-part-1-hardware-and-architecture"
aliases:
  - how-to-homelab-part-1
type: posts
index: true
comments: true
isCJKLanguage: true
series:
  - Homelab
categories:
  - Technology
tags:
  - homelab
  - hardware
  - architecture
  - devops
  - linux
image: https://images.unsplash.com/photo-1549319114-d67887c51aed?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2874&q=80
imageSource:
  - name: Martijn Baudoin
    link: https://unsplash.com/photos/4h0HqC3K4-c
  - name: Unsplash
    link: https://unsplash.com
---

{{< updated at="2023-02-12" >}}
PDD 600 多采购 Apple TV 用于代替原先的 Android 电视盒子，终于有了一个价格合适即有网飞认证高清又能提供更好的生态体验。
{{< /updated >}}

前面[先导篇]({{< ref "posts/2022-02-12-how-to-homelab-part-0.md" >}})全面性介绍了搭建家用 homelab 软硬件的可能性，实际操作上每个人的室内环境，网络布线都不太一样，钞能力的不同也有会千万种解法。我无法给出一个 100% 的解决方案，但我给大家回顾自己设备架构演变的过程在每个阶段是什么需求，遇到什么问题以及如何应对的，在文章末尾也会给出一些搭建 homelab 中不容忽视的因素。

## 我的设备架构

> 首先声明个人设备架构并不代表这是最佳的方案，只是当前符合我要求的结果，随着需求和技术变化而迭代更新。我的梦想就是有间下图的[地下室](https://blog.cavelab.dev/2021/02/new-home-office/)能够给我随意折腾就好了。

{{< figure src="/tutorials/how-to-homelab/part-1/memo-devices-changes.png"
    title="homelab 梦想预期"
    caption="创意来源"
    attr="@HCocoa@twitter"
    attrlink="https://twitter.com/featherye/status/1619863195145089025"
>}}

当时房子装修是一切从简{{< spoiler >}}（没有钱）{{< /spoiler >}}造成网络线路设计不太合理：

- 电视墙，卧室和工作室虽预埋 6 类网线但都是单根
- 弱电箱[太小](https://twitter.com/icyleaf/status/1591070791646744576)无法扩展，前期很多设备都堆积到电视后面
- 房子不大无法设置散热友好的独立机柜
- 房间有 WIFI 死角

新增家庭成员后原工作室改成了儿童房，部分设备又做了迁移，只能基于现有的结构进行优化和改进，以下请目睹我的血泪史。

### 设备架构演变

#### 2014 ~ 2016

{{< figure src="/tutorials/how-to-homelab/part-1/homelab-diagram-v0.png"
    link="/tutorials/how-to-homelab/part-1/homelab-diagram-v0.png"
    title="v0 设备架构拓扑图"
>}}

这个期间换了新工作上班通勤每天都较长回家也不太折腾网络，路由器是从租房时代沿用过来的网件 WGR614。电视图便宜买了当时乐视出的第一代智能电视满足打开听个响的需求结果不到半年时间系统卡成翔，以至于之后再也没考虑过 Android 系统电视，头两年为了省钱明知在北京应该用联通还是莽了北方电信，那网速真是垃圾啊。还捡了一些不太靠谱的垃圾件就不提了。

#### 2016 ~ 2018

{{< figure src="/tutorials/how-to-homelab/part-1/homelab-diagram-v0.1.png"
    link="/tutorials/how-to-homelab/part-1/homelab-diagram-v0.1.png"
    title="v0.1 设备架构拓扑图"
>}}

托工作福利政策出国旅游频次逐渐变多，拍摄的照片越来越多需要存储，2016 年从[什么值得买](https://post.smzdm.com/p/399864/)了解后德淘了一台 HP Microserver Gen8。这是一台拥有 4 盘位，双千兆网口还带 iLO 管理功能的服务器，我却给它只安装了黑群晖算是成为了最早的 NAS 服务器设备架构，5.x 版本的群晖还不支持 Docker 算是一台单纯的 NAS 服务用来存储照片、视频、替代 Dropbox 的 Drive 服务，从上图也能看到我当时完全不了解什么是链路聚合，要不然我肯定给黑群晖接入双网口了。

路由器升级到了网件 6300v2 并刷入了 KoolCenter 定制化的[梅林固件](https://twitter.com/icyleaf/status/887209662012989440)方便畅游网络，内网穿透这个时候被迫在使用，黑群晖升级后没法完全洗白之前的[群晖 QuickConnect](https://quickconnect.to/) 服务被废，从联通客服申请公网 IP 后在通过 DDNS 定时上报（脚本还是[糖醋鼻子](https://twitter.com/zhmocean)提供的）。

电视更换成了索尼 4K 60 寸互联网电视，外接了一个晶晨 S912 外贸盒子安装了 Kodi 和 Youtube 成为家庭影音系统。

PS 4 应该是在某一年黑五美亚捆绑赠送了 GTA5 和最后生还者的版本。

#### 2018 ~ 2019

{{< figure src="/tutorials/how-to-homelab/part-1/homelab-diagram-v1.0.png"
    link="/tutorials/how-to-homelab/part-1/homelab-diagram-v1.0.png"
    title="v1 设备架构拓扑图"
>}}

2018 年我在推上吐槽梅林固件刷机太难被 [anbutu](https://twitter.com/icyleaf/status/1100322141914853376) 安利 openwrt 系统并赠送了一台 [N270 x86 32 位双网口工控机](https://cn.v2ex.com/t/659602)让我了解了新领域：软路由。

黑群晖升级到了支持 Docker 的 6.x 版本，开始尝试跑一些基础服务，比如 [aria2](https://aria2.github.io/)，[home assistant](https://www.home-assistant.io/)，[Adguard home](https://github.com/AdguardTeam/AdGuardHome) 等。不知道什么时候小区会莫名其妙突然停电几分钟后再恢复，有一次硬盘被群晖的检测爆出来好多坏道，吓得我立刻买了[施耐德 APC BK650](https://twitter.com/icyleaf/status/1096386681907798017) 带通讯协议，接入群晖保证断电后安全关机。

2019 年初开始学习拍摄视频，采购了[新的 Intel 主机]({{< ref "2019-01-30-itx-coffee-lake-hackintosh-build-for-4k-video-editing.md" >}})在时隔 [9 年](https://twitter.com/icyleaf/status/11322513061580800)后后重新学习安装[黑苹果]({{< ref "2019-03-28-asrock-z390-gaming-itx-install-hackintosh-tutorial.md" >}})来代替年迈的 Macbook Pro 2015 款，期间不升级后续版本主要是因为 Intel 太拉，Apple 把硬件全焊死升级顶配不值得。

#### 2020 ~ 2021

{{< figure src="/tutorials/how-to-homelab/part-0/homelab-diagram-v2.0.png"
    link="/tutorials/how-to-homelab/part-0/homelab-diagram-v2.0.png"
    title="v2 设备架构拓扑图"
    caption="首发"
    attr="@icyleaf@twitter"
    attrlink="https://twitter.com/icyleaf/status/1472036769742745603"
>}}

2020 年末实际上才是我正式踏入 homelab 元年，之前只是满足存储需求的 NAS 服务。

软路由升级了至 [E3845 四口工控机](https://twitter.com/icyleaf/status/1242070362839330817)（代号 [Larva](https://starcraft.fandom.com/wiki/Larva_(StarCraft_II))）它只承担最基础服务比如拨号，DNS 服务、屏蔽广告，DDNS 等。

交换机迷之自信选择网件 GS105 4 口非网管版，当时很多人问我为什么不选 8 口，我没有意识到后面的变化只是考虑弱电箱只能塞下 4 口。

应用服务由当时新爆出来的[蜜獾超存](https://twitter.com/icyleaf/status/1339568737083527169)（代号 [Corruptor](https://starcraft.fandom.com/wiki/Corruptor)）矿机[^honeybadger]承担：6 盘位 / 双千兆网口 / J1900 CPU /8G 内存 / 64G MSATA，我主要是看中了它的机箱体积和设计，比蜗牛星际之类的好看太多。在它身上分别调研尝试下列系统：

- Debian: 直接跑 Docker 服务有点心不甘
- [OMV](https://maxiee.github.io/post/mihuan1md/): 最简单的 NAS 系统提供 Docker/Proxmox 内核，但任何操作都要 Apply 好久，实在没法接受
- [Proxmox](https://www.proxmox.com): 跑十几个不太吃性能系统及服务勉强可接受
- [harvester](https://www.rancher.com/products/harvester): Rancher 开源的基于 k8s 提供完全集成的存储和虚拟化功能的[超融合基础架构软件](https://mp.weixin.qq.com/s?__biz=MzkyNzM4Nzk1NQ==&mid=2247500683&idx=1&sn=2a30a79313c6b51b41a39c6618ccef1b)，万万没想到 [J1900 带不动](https://twitter.com/icyleaf/status/1429100937499447296)...

或许你会问为什么不利用好 Gen 8 服务器？原因是 G1610T 性能太弱带不起 ESXI 虚拟化，可升级 CPUs 比如 E3 1265L v2 之类的价格都虚高。我从一开始就把它当做纯粹的 NAS 服务器。四盘位上设置两两硬盘组成 RAID 1。一组 RAID 3T 作为照片存储，第二组 3T 是提供给蜜獾超存安装的服务提供数据存储服务，光驱位扩展一个 SSD 充当缓存盘就完事了。

经过一番折腾倔强的 J1900 扛不住压力，从咸鱼分别入了 17x17 豆希 ITX 主板，定制了 flex 电源，[改了静音风扇](https://www.bilibili.com/read/cv9438621#reply149844420272)，配上 8700es CPU，PDD 入的单条 32G 内存和[酷兽 256G M2 SSD](https://twitter.com/icyleaf/status/1416395829858869249)，复用蜜獾超存的机箱但它机身宽度较窄符合要求的只有利民 AXP90 x36，机箱后出风用都是酷冷的漩涡 80 静音风扇。

系统继续使用 Proxmox 设置备份还原工具[^pve_backup_restore]后用 VMs 跑 [jellyfin](https://github.com/jellyfin/jellyfin), [portainer](https://github.com/portainer/portainer), [vaultwarden](https://github.com/dani-garcia/vaultwarden), [uptime kuma](https://github.com/louislam/uptime-kuma), [traefik](https://github.com/traefik/traefik) 等监控、数据库、应用服务，存储方面酷兽 SSD 作为系统盘，[闲置的 4 块硬盘](https://twitter.com/icyleaf/status/1228626720489537537)组了 btrfs RAID 10，第五块做 3T 做电影、电视剧下载盘，最后一块是备用。

通过两次硬件升级淘汰的主机也都没有失去价值

- J1900 板 U 卖了也不值几个钱，干脆新套了[MAXT 小机箱](https://twitter.com/icyleaf/status/1561341317778575363/)用于测试新的试验田捣腾其他稀奇古怪的系统、服务，并赋予新代号 [Deadpool](https://en.wikipedia.org/wiki/Deadpool)
- N270 软路由在爆发疫情初期寄给了需要的武汉朋友

#### 2022 ~ 2023

> 实际上是 2022 整年的版本，后续有变化会再更新。

{{< figure src="/tutorials/how-to-homelab/part-1/homelab-diagram-v2.1.png"
    link="/tutorials/how-to-homelab/part-1/homelab-diagram-v2.1.png"
    title="v2.1 设备架构拓扑图"
    caption="首发"
    attr="@icyleaf@twitter"
    attrlink="https://twitter.com/icyleaf/status/1619228928685801474"
>}}

原计划是可以安心跑个 1 - 2 年不太可能会有什么大变化，只需要在基于 Proxmox 系统继续试验并搞定多 VMs 跑 k8s/k3s 集群后就能安心养老，两次事故打乱了我的规划。一次是升级至 64G 内存后[挂了块硬盘](https://twitter.com/icyleaf/status/1534419122427408385)，幸好从 RAID 移除后还能[正常工作](https://twitter.com/icyleaf/status/1534543188333297665)；第二次直接 All in one boom 原因是 [CPU 散热风扇无法工作](https://twitter.com/icyleaf/status/1583461623179509761)。在采购风扇和优化机箱风道期间给了我反思的过程，我明明是排斥 All in one 设计的结果自己主力开发机也是这样的设计，服务越多则越要保证服务可用性，那就要新添至少 1 - 2 新主机但家里真的没有更多的地方，翻阅国内外的资料目标锁定了几个目标：

- [NEC M700](https://homelab.khuedoan.com/): 6 代，魔改支持 7/8/9 代
- [树莓派 4B](https://rpi4cluster.com/): 有丰富的成功案例但价格简直一个离谱[^rpi]
- [荣品 king3399](https://v2ex.com/t/878188): RK3399 性能强但我没赶上车价格被炒上去且 2G 内存有点小

某天在闲鱼刷推荐线看到一个比荣品 king3399 更好的 [EAIDK 610](https://twitter.com/icyleaf/status/1586636664457551872) RK3399 开发板 4G 内存价格才 200 出头，尝试买了两个又在 [anbutu](https://twitter.com/icyleaf/status/1598302816938127360) 和社区 [eaidk-610 armbian-build 项目](https://gitee.com/e190/armbian/)的帮助下成功编译并烧录了 armbian 系统，跑通 k3s 服务后又入了两个和 Proxmox 的 VMs 组成了 amd86 和 arm86 的混合集群。

{{< figure src="/tutorials/how-to-homelab/part-1/eaidk-610.jpg"
    link="/tutorials/how-to-homelab/part-1/eaidk-610.jpg"
    title="EAIDK 610 开发板"
    caption="首发"
    attr="@icyleaf@twitter"
    attrlink="https://twitter.com/icyleaf/status/1585792876235005953"
>}}

就这上面集群测试跑通的同时 J1900 主机也完成了对 [nomad](https://twitter.com/icyleaf/status/1579499063577554945) 服务的试水并逐步稳定下来，后面也可以和 Proxmox VMs 组成混合集群。

内网穿透方面除了公网加端口之外多了 [traefik hub](https://twitter.com/icyleaf/status/1539849981598740480) 方案。作为最早一批内测用户还额外拥有更多的免费额度，现在免费版限制最大 5 个公网服务。

Gen 8 在更换新的氦气盘转移数据有大量的视频转码工作要跑 CPU 有点吃不消，闲鱼 1230v2 价格 150 [收了它](https://twitter.com/icyleaf/status/1564962797372395520)继续卖命，群晖[需要修改](https://twitter.com/icyleaf/status/1565176566535323649)才能正确识别新 U。

## 我的设备选择

> 用了很长的篇幅介绍了我的设备演变史，从文字和拓扑图大家已经了解了七七八八，为了方便理解这里会再汇总一份供大家赏阅。在软硬件的折腾方面我总能想起[明城](https://www.gracecode.com/posts/3191.html)大哥。

软硬件我采用的是 [thoughtworks 技术雷达](https://www.thoughtworks.com/zh-cn/radar) 的策略把任何一个方案划分成`评估`、`试验`、`采纳`和`暂缓` 4 个阶段，因此会包含很多解决方案，标记`采纳`的可安心服用。

### 硬件

{{< div >}}
<table>
<thead><tr><th>主机代号</th>
<th>系统</th>
<th>阶段</th>
<th>数量</th>
<th>用途</th>
</tr></thead>
<tbody>
<tr>
<td>
  <a href="https://starcraft.fandom.com/wiki/Immortal">Immortal</a> <sup>amd64</sup>
  <br />9700k/32G/6TB/6600xt
</td>
<td>macOS<br />Windows</td>
<td><span class="badge bg-success">采纳</span></td>
<td>1</td>
<td>个人生产力工具</td>
</tr>
<tr>
<td>
  <a href="https://starcraft.fandom.com/wiki/Queen_(StarCraft_II)">Queen</a> <sup>amd64</sup><br />
  HP Gen8 (1230v2/8G/40TB)
</td>
<td>群晖</td>
<td><span class="badge bg-success">采纳</span></td>
<td>1</td>
<td>NAS</td>
</tr>
<tr>
<td>
  <a href="https://starcraft.fandom.com/wiki/Corruptor">Corruptor</a> <sup>amd64</sup><br />
  蜜獾超存 (8700es/64G/10TB)
</td>
<td>Promox</td>
<td><span class="badge bg-success">采纳</span></td>
<td>1</td>
<td>虚拟开发机</td>
</tr>
<tr>
<td>
  <a href="https://starcraft.fandom.com/wiki/Bunker">Bunker</a> <sup>amd64</sup><br />
  J1900/8G/1TB
</td>
<td>Debian</td>
<td><span class="badge bg-success">采纳</span></td>
<td>1</td>
<td>Nomad 集群成员</td>
</tr>
<tr>
<td>
  <a href="https://starcraft.fandom.com/wiki/Larva_(StarCraft_II)">Larva</a> <sup>amd64</sup><br />
  E3845/2G/16GB/i211<small>x4</small>
</td>
<td>Openwrt</td>
<td><span class="badge bg-success">采纳</span></td>
<td>1</td>
<td>软路由</td>
</tr>
<tr>
<td>
  <a href="https://starcraft.fandom.com/wiki/Splitter">Splitter</a> <sup>arm64</sup><br />
  EAIDK610 (RK3399/4G/6+128GB)
</td>
<td>Armbian</td>
<td><span class="badge bg-warning">试验</span></td>
<td>4</td>
<td>k3s 集群</td>
</tr>
<tr>
<td>
  <a href="https://starcraft.fandom.com/wiki/Colossus">Colossus</a> <sup>arm64</sup><br />
  Apple TV 2021(4K/32G)
</td>
<td>Apple TV</td>
<td><span class="badge bg-success">采纳</span></td>
<td>1</td>
<td>新电视盒子</td>
</tr>
<tr>
<td>
  <a href="https://starcraft.fandom.com/wiki/Lair">Lair</a> <sup>armv8</sup><br />
  H96 Pro+ (S912/4G/32GB)
</td>
<td>Android TV</td>
<td><span class="badge bg-success">采纳</span></td>
<td>1</td>
<td>备用电视盒子</td>
</tr>
<tr>
<td>
  <a href="https://starcraft.fandom.com/wiki/Drone">Drone</a> <sup>arm64</sup><br />
  Orange Pi 3 LTS
</td>
<td>Armbian</td>
<td><span class="badge bg-notice">评估</span></td>
<td>1</td>
<td>未完成的 IP KVM</td>
</tr>
</tbody></table>
{{< /div >}}

当主机出现故障需要维护时需要单独的显示器和键鼠，更好的解决方案是 [IP KVM](https://github.com/stars/icyleaf/lists/ip-kvm)。现有方案要么仅支持树莓派 4B，要么需要两个开发板。我曾使用树莓派 3B 和 [香橙派 3LTS](https://twitter.com/icyleaf/status/1539975823670710274) 刷入 [tinypilot](https://github.com/tiny-pilot/tinypilot) 只能开启 HDMI 画面采集，无法模拟键鼠操作。

偏向底层及硬件一直是我的弱项，我想世界末日了我们这些写软件服务的都会在第一时间挂了吧，哈哈哈。

#### 硬盘

在个人能力范围内优先级考虑：`M2 SSD` > `SATA SSD` > `氦气盘` > `非叠瓦盘`，这方面我也看着大家的建议买，建议是硬盘太多做好整理，尤其是购买时间，购买渠道，购买数量，硬盘编号，保质年限，过保时间以及定期的 SMART 检测数据记录。

{{< figure src="/tutorials/how-to-homelab/part-1/harddisk-2022-full.png"
    link="/tutorials/how-to-homelab/part-1/harddisk-2022-full.png"
    pswp-width="949"
    pswp-height="1253"
    title="硬盘大军"
>}}

{{< figure src="/tutorials/how-to-homelab/part-1/harddisk-category.png"
    link="/tutorials/how-to-homelab/part-1/harddisk-category.png"
    title="硬盘分布情况"
>}}

#### UPS 电源

一次电力的闪断更有可能让服务器硬件（尤其是的硬盘）就会出现故障而坏掉，保证硬件和数据的安全性 UPS 电源是必备的，优先考虑支持通讯的，因为停电虽然有 UPS 可以用电池模式继续保持运行，电池电量是有限的服务器如果知道当前的状态进行安全关机。

{{< div >}}
<table>
<thead><tr>
<th>UPS 设备</th>
<th>辐射范围</th>
<th>描述</th>
</tr></thead>
<tbody>
<tr>
<td>APC BK650</td>
<td>Proxmox + 黑群晖 + WIFI AP</a></td>
<td>连接 Proxmox 并开启 NUT 服务和<br /> apcupsd 数据接入 Prometheus</td>
</tr>
<tr>
<td>APC BK650</td>
<td>黑苹果 + Armbian 集群 + Nomad</a></td>
<td>连接 Nomad 并开启 NUT 服务<br /> apcupsd 数据接入 Prometheus</td>
</tr>
<tr>
<td><a href="https://twitter.com/icyleaf/status/1590351704020910080">弱电箱 UPS</a></td>
<td>光猫 + 软路由 + 交换机</a></td>
<td>软路由接受其他两个 NUT 通知<br />四口 12V 还富裕一个</td>
</tr>
</tbody></table>
{{< /div >}}

我只使用过施耐德 APC 带有通讯协议的 UPS 基本上可通过 [apcupsd](http://www.apcupsd.org/) 或 NUT（绝大多数 NAS 系统比如群晖、威联通等都支持）通过该服务可以让没有直接插通讯线的设备也能过接收到通知并执行安全关机操作[^ups-post-action]。

### 软件

#### 操作系统

{{< div >}}
<table>
<thead><tr><th>操作系统</th>
<th>阶段</th>
<th>描述</th>
</tr></thead>
<tbody>
<tr>
<td><a href="https://www.proxmox.com/">proxmox</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>高可玩性且兼具自动化管理的虚拟机系统</td>
</tr>
<tr>
<td><a href="https://www.debian.org/">debian</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>作为服务器个人最熟悉的基础 amd64 OS</td>
</tr>
<tr>
<td><a href="https://www.armbian.com/">armbian</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>ARM 版本的 Debian，理由同上</td>
</tr>
<tr>
<td><a href="https://openwrt.org/">openwrt</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>可玩性很高的开源软路由系统</td>
</tr>
<tr>
<td><a href="https://www.talos.dev/">talos</a></td>
<td><span class="badge bg-warning">试验</span></td>
<td>100% API 管理且支持多部署环境的基于 k8s 实现的发行版</td>
</tr>
<tr>
<td><a href="https://pi-hole.net/">pi-hole</a></td>
<td><span class="badge bg-warning">试验</span></td>
<td>国外非常流行的 DNS 管理系统，界面友好</td>
</tr>
<tr>
<td><a href="https://rockstor.com/">rockstor</a></td>
<td><span class="badge bg-notice">评估</span></td>
<td>基于 openSUSE + btrfs 的 NAS 系统，支持 SMART 和 NUT<br />注意<a href="https://twitter.com/icyleaf/status/1610885440961404929">不兼容 Asia/Beijing 时区</a></td>
</tr>
<tr>
<td><a href="https://kairos.io/">kairos</a></td>
<td><span class="badge bg-notice">评估</span></td>
<td>新发布的容器化系统，感兴趣但还没成功跑通</td>
</tr>
<tr>
<td><a href="https://www.truenas.com/">truenas</a></td>
<td><span class="badge bg-notice">评估</span></td>
<td>基于 FreeBSD 开发的 NAS 系统（ZFS 首选）</td>
</tr>
<tr>
<td><a href="https://www.openmediavault.org/">omv</a></td>
<td><span class="badge bg-danger">暂缓</span></td>
<td>完成度高的 NAS 系统但个人无爱</td>
</tr>
<tr>
<td><a href="https://vmware.github.io/photon/">photonOS</a></td>
<td><span class="badge bg-danger">暂缓</span></td>
<td>Vmware 虚拟化优化但嫌弃 redhat 系统</td>
</tr>
<tr>
<td><a href="https://www.tritondatacenter.com/smartos">smarterOS</a></td>
<td><span class="badge bg-danger">暂缓</span></td>
<td>支持虚拟化和 ZFS 但依赖高内存成度的 NAS 系统但个人无爱</td>
</tr>
</tbody></table>
{{< /div >}}

性能强大的机器以 Proxmox 作为宿主机内部套 Debian 或 Armbian 来跑所需的服务或基于容器化技术的编排管理服务或容器化系统（Linux Container OS[^linux-container-os]）

#### 文件系统

{{< div >}}
<table>
<thead><tr><th>类型</th>
<th>阶段</th>
<th>描述</th>
</tr></thead>
<tbody>
<tr>
<td>btrfs</td>
<td><span class="badge bg-success">采纳</span></td>
<td>磁盘管理方便，支持快照和 COW</td>
</tr>
<tr>
<td>ext4</td>
<td><span class="badge bg-success">采纳</span></td>
<td>最保险的文件系统</td>
</tr>
<tr>
<td>zfs</td>
<td><span class="badge bg-notice">评估</span></td>
<td>强健可靠、可伸缩、易于管理就是吃内存</td>
</tr>
<tr>
<td>xfs</td>
<td><span class="badge bg-notice">评估</span></td>
<td>据说速度很快，个人没有太多研究<br />talos <a href="https://www.talos.dev/v1.3/learn-more/architecture/">默认文件系统</a></td>
</tr>
</tbody></table>
{{< /div >}}

个人优先级选择 `btrfs` > `ext4` > `zfs` > `xfs`。注意的是 btrfs 现阶段不建议使用 [RAID 5/6](https://btrfs.wiki.kernel.org/index.php/Status#RAID56)，不太考虑 zfs 是组 RAID 后新增硬盘麻烦且有成本。xfs 真不了解，感兴趣的可以看下以上文件系统[在 PostgreSQL 的基准测试](https://www.dimoulis.net/posts/benchmark-of-postgresql-with-ext4-xfs-btrfs-zfs/)。

对于 btrfs 我个人的看法是只有自己尝试过才知道结果，尽管 Promox [7.0](https://www.proxmox.com/en/news/press-releases/proxmox-virtual-environment-7-0) 第一把 btrfs 作为[技术预览](https://pve.proxmox.com/wiki/BTRFS)的情况下发布，我也做了小白鼠使用它在将近 2 年的时间内基本正常，只有一次小故障还是因淘宝买的垃圾硬盘质量太差出现太多的坏道。btrfs 在 RAID10 最低 4 块磁盘的前提下移除掉坏盘也能过正常工作（删减都做一次 [balance](https://btrfs.readthedocs.io/en/latest/Balance.html) 即可），除此之外我并没有遇到任何问题。虽然 COW 特性会拖累磁盘 IO 现状也都能接受。

对 btrfs 感兴趣的小伙伴推荐看 [@Houge](https://twitter.com/Houge_Langley) 的[教学视频](https://www.bilibili.com/video/BV1Fh411e7Wk/)或者 openSUSE 官方推出的[入门教学视频](https://www.bilibili.com/video/BV115411u7bU/)。用过或已入门 btrfs 的可深入阅读 [btrfs 与 zfs 快照实现差异](https://farseerfc.me/zhs/btrfs-vs-zfs-difference-in-implementing-snapshots.html)，[btrfs 与 xfs 对比](https://linuxhint.com/btrfs-vs-xfs-brief-comparison/)，[Five Years of Btrfs](https://markmcb.com/2020/01/07/five-years-of-btrfs/) 和 [BTRFS Best Practices](https://it-notes.dragas.net/2018/10/13/btrfs-best-pratices/) 做到心中有数。

#### 存储服务

{{< div >}}
<table>
<thead><tr><th>服务</th>
<th>阶段</th>
<th>描述</th>
</tr></thead>
<tbody>
<tr>
<td>samba</td>
<td><span class="badge bg-success">采纳</span></td>
<td>兼容性和实用性最高，仅建议手动文件挂载使用</td>
</tr>
<tr>
<td>nfs</td>
<td><span class="badge bg-success">采纳</span></td>
<td>可作为最低保障数据挂载</td>
</tr>
<tr>
<td><a href="https://min.io/">minos</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>兼容 S3 应用的开源存储服务</td>
</tr>
<tr>
<td><a href="https://juicefs.com/">juicefs</a></td>
<td><span class="badge bg-warning">试验</span></td>
<td>S3 兼容且 POSIX 兼容性高的开源存储服务</td>
</tr>
<tr>
<td><a href="https://longhorn.io/">longhorn</a></td>
<td><span class="badge bg-warning">试验</span></td>
<td>简单好用的块存储开源服务，磁盘迁移<a href="https://twitter.com/icyleaf/status/1607372383245172737">很容易</a></td>
</tr>
<tr>
<td><a href="https://rook.github.io/">rook ceph</a></td>
<td><span class="badge bg-notice">评估</span></td>
<td>当下具有很大潜力的云原生存储服务<br />小集群或弱鸡 CPU 不建议使用</td>
</tr>
<tr>
<td><a href="https://github.com/openebs/Mayastor">mayastor</a></td>
<td><span class="badge bg-notice">评估</span></td>
<td>针对 NVME 优化的块存储服务</td>
</tr>
</tbody></table>
{{< /div >}}

存储方面之前都是 Samba、NFS 甚至只用 APF，直到 2022 年才正式开始在生产环境试验，尤其是针对 k8s 的存储我还是一个小白。

#### 容器化管理及编排服务

{{< div >}}
<table>
<thead><tr><th>服务</th>
<th>阶段</th>
<th>描述</th>
</tr></thead>
<tbody>
<tr>
<td><a href="https://www.portainer.io/">portainer</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>支持 Docker/k3s/nomad 多种编排服务的管理服务{{< /div >}}[^portainer-nomad]{{< div >}}</td>
</tr>
<tr>
<td><a href="https://kubesphere.io/">kubesphere</a></td>
<td><span class="badge bg-danger">暂缓</span></td>
<td>新手和企业友好的 k8s 前端容器管理服务，整体来说有点重</td>
</tr>
<tr>
<td><a href="https://www.nomadproject.io/">nomad</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>入门门槛较低但缺乏教学资料的编排服务</td>
</tr>
<tr>
<td><a href="https://k3s.io/">k3s</a></td>
<td><span class="badge bg-warning">试验</span></td>
<td>针对边缘计算、物联网等场景进行了高度优化轻量级的 k8s 发行版</td>
</tr>
<tr>
<td><a href="https://kubernetes.io/">kubernetes</a></td>
<td><span class="badge bg-notice">评估</span></td>
<td>100% 正统血缘 k8s，不敢靠近 :D</td>
</tr>
<tr>
<td><a href="https://docs.docker.com/engine/swarm/">docker swarm</a></td>
<td><span class="badge bg-danger">暂缓</span></td>
<td>官方自己都快弃权的编排服务，不推荐</td>
</tr>
</tbody></table>
{{< /div >}}

Portainer 是一款新人友好的容器管理工具，至今我现在也还在用。k3s 也是最容易跨进 k8s 世界且 edge 友好的编排服务。

#### 网关

{{< div >}}
<table>
<thead><tr><th>服务</th>
<th>阶段</th>
<th>描述</th>
</tr></thead>
<tbody>
<tr>
<td><a href="https://doc.traefik.io/traefik/">traefik</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>个人认为最好用的网关服务</td>
</tr>
<tr>
<td><a href="https://caddyserver.com/">caddy</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>简单好用的支持 Let's Encrypt 的网关服务</td>
</tr>
<tr>
<td><a href="https://www.nginx.com/">nginx</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>管理多域名可考虑 <a href="https://nginxproxymanager.com">nginx proxy manager</a></td>
</tr>
</tbody></table>
{{< /div >}}

虽然都是标记的`采纳`，我主要用前两个，网关首推 traefik，简单使用 caddy，前两个简单好用功能还强大我想不出来理由用第三个。

#### 自动化部署

{{< div >}}
<table>
<thead><tr><th>服务</th>
<th>阶段</th>
<th>描述</th>
</tr></thead>
<tbody>
<tr>
<td><a href="https://www.ansible.com/">ansible</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>无代理（利用 SSH）就能自动化部署的配置工具</td>
</tr>
<tr>
<td><a href="https://www.terraform.io/">terraform</a></td>
<td><span class="badge bg-success">采纳</span></td>
<td>只要有接口管理的服务都能自动化部署的工具<br />还是 ansible 最佳排挡</td>
</tr>
<tr>
<td><a href="https://fluxcd.io/flux/">fluxcd</a></td>
<td><span class="badge bg-warning">试验</span></td>
<td>最好用的 gitops 里对 k8s 自动配置部署工具</td>
</tr>
<tr>
<td><a href="https://argo-cd.readthedocs.io/">argocd</a></td>
<td><span class="badge bg-warning">试验</span></td>
<td>gitops 里对 k8s 自动配置部署还有可视化拓扑图</td>
</tr>
<tr>
<td><a href="https://www.pulumi.com/">pulumi</a></td>
<td><span class="badge bg-warning">试验</span></td>
<td>支持多种原生语言配置版本的 terraform<br />实现架构优秀，使用者友好，插件开发者痛苦</td>
</tr>
<tr>
<td><a href="https://saltproject.io/">salt</a></td>
<td><span class="badge bg-danger">暂缓</span></td>
<td>有代理，当初一经面世就要碾压 ansible 的方案<br />看看市场的选择，它也不咋地嘛</td>
</tr>
</tbody></table>
{{< /div >}}

只要是跟操作系统打交道的 ansible + terraform 打遍天下无敌手！fluxcd 在对 k8s 服务配置和部署上确实找不出毛病，门槛有就看你能不能入门，建议在熟悉 k8s 基础概念和有一定实际部署经验后再使用。

## 不容忽视的因素

大量的篇幅介绍了我个人 homelab 设备架构演变和软硬件的选择，还有什么容易忽略的因素呢？

如果把设备比做核心建筑，不容忽视的因素那就是基础建设。两手都要抓这样才能以确保 homelab 能够发挥最大的效能，谁也不想性能无法 100% 压榨或意外故障的发生。

### 网线规格

**`务必保证所有 homelab 的设备都接入千兆以上有线网络`**，WIFI 会收到周围信道干扰、传输衰减等不稳定性问题。

{{< figure src="/tutorials/how-to-homelab/part-1/cable-ethernet-data-rates.png"
    link="/tutorials/how-to-homelab/part-1/cable-ethernet-data-rates.png"
    title="不同规格网线速率图"
    caption="资料来源"
    attr="IEEE 802 LMSC"
    attrlink="https://www.ieee802.org/3/cfi/1114_1/CFI_01_1114.pdf"
>}}

{{< div >}}
<table>
<thead>
<tr>
<th>规格</th>
<th>类型</th>
<th>速率</th>
<th>接口</th>
<th>备注</th>
</tr>
</thead>
<tbody>
<tr>
<td>五类线<br /><sup>CAT 5</sup></td>
<td>100Base-T<br />10Base-T</td>
<td>100Mbps</td>
<td>RJ45</td>
<td>不推荐</td>
</tr>
<tr>
<td>超五类线<br /><sup>CAT 5E</sup></td>
<td>100Base-T</td>
<td>1000Mbps<br />2.5Gbps{{< /div >}}[^bandwidth]{{< div >}}</td>
<td>RJ45</td>
<td>2.5G 网络<a href="https://www.bilibili.com/video/BV1p14y137v3">仅限 100 米以内</a></td>
</tr>
<tr>
<td>六类线<br /><sup>CAT 6</sup></td>
<td>100Base-T</td>
<td>1Gbps<br />10Gbps</td>
<td>RJ45</td>
<td>万兆网络仅限 50 米内</td>
</tr>
<tr>
<td>超六类线<br /><sup>CAT 6A</sup></td>
<td>100Base-T</td>
<td>10Gbps</td>
<td>RJ45</td>
<td>200 米内可达万兆网络<br />没有 6E 标准</td>
</tr>
<tr>
<td>七类线<br /><sup>CAT 7</sup></td>
<td>100Base-T</td>
<td>10Gbps</td>
<td>GG45/TERA</td>
<td>带着遮蔽</td>
</tr>
<tr>
<td>光纤</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>不懂，详见<a href="https://zh.wikipedia.org/zh-cn/%E5%85%89%E7%BA%96%E9%80%9A%E8%A8%8A">维基百科</a></td>
</tr>
</tbody>
</table>
{{< /div >}}

再次重申 **`千兆以上网络是不可或缺的`**，最低限度使用 CAT 5E，强烈推荐使用 CAT 6/6A 规格，土豪们 CAT 7 或光纤随意。假如你不知道家里网络的状态提供两种方法检查：

1. 查看网线上面的印字会有网线规格标识
1. 过 [iperf3](https://github.com/esnet/iperf) 在两台任意可连接有线的设备充当服务器端和客户端进行检测。

```bash
# 一台开启服务端，假设服务器 IP 是 192.168.1.100
iperf3 -s

# 一台开客户端，连接服务端进行测试
iperf3 -c 192.168.1.100
```

### 噪音和散热

- 硬件
  - 机械硬盘读写盘的噪音（有钱就全 SSD 或等待 [EDSFF E1/E3 卡](https://twitter.com/icyleaf/status/1619937731777556486)民用）
  - 风扇的轴承、转速和大小也会产生噪音（CPU 散热、显卡、机箱、电源等）
  - 主板 DEBUG 蜂鸣器（有的可关闭或[拆除](https://twitter.com/icyleaf/status/1432343002765139968)）
- 软件
  - 群晖为保障系统稳定性默认会全盘写入系统作为 backup 若要解决噪音问题建议[策略性移除](https://twitter.com/icyleaf/status/1620093653791436800)
  - Linux 系统可考虑使用 [lm-sensors](https://wiki.archlinux.org/title/fan_speed_control) 侦测配置
- 空间
  - 摆放位置决定噪音耐受度和散热效率

### 省电和功率

CPU 的待机 TPW 只是参考，还需要考虑硬盘，内存和显卡整体，还需要考虑峰值功率，这块没有太多可展开的。要考虑能效但也不过分的在意，尤其是为了降低 5 - 10w TPW 而购买溢价过高的产品，这个在前篇的小结也有提到。

## 小结

在 homelab 这条路上刚开始选什么硬件不重要， All in one 基本上是每个入门都会经历的，随着时间推移，就像房子需要维修、车子需要定期保养一样，服务的稳定性、数据安全性都需要关注和维护。

{{<twitter user="0x8023" id="1569942942651121664">}}

你可能会说我 all in one 几年了一切稳定，我只能说：没有经历过痛苦的人永远不会知道什么叫痛。

<!-- 以下是注脚 -->

[^honeybadger]: 我记得是在[矿渣社区](https://bbs.nas66.com/)看到的发车，后来看的[阿文菌](https://post.smzdm.com/p/andr83k3/)的文章。
[^pve_backup_restore]: 官方提供[备份和恢复](https://pve.proxmox.com/wiki/Backup_and_Restore)方法，Github Gist 也有[备份脚本](https://gist.github.com/mrpeardotnet/6bdc4b504f43ce57fa7eaee96d376edf)
[^rpi]: 以疫情为由成本上升，国内市场 4B 最高能卖到 1200，我吃灰的 3B 都卖了 600 块
[^linux-container-os]: 关于容器化 OS 可选性可以看看 [Reimu 的博文](https://blog.k8s.li/Photon-OS.html)
[^portainer-nomad]: Nomad 支持需要[商业授权](https://docs.portainer.io/admin/licenses)，当前可以申请[ 5 节点免费商业](https://www.portainer.io/take-5)
[^bandwidth]: 带宽是 1000 进制，1Gbps = 1000Mbps = 千兆网络, 10Gbps = 万兆网络
[^ups-post-action]: [配置教程](https://blog.irain.in/archives/NUT_apcupsd_Synology_DSM_UPS.html)和关机方案 [1](https://blog.k8s.li/apcupsd-on-openwrt-with-esxi.html)、[2](https://github.com/mingcheng/apcupsd_guarder)
