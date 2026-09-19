# 沪日 IX 测评：上云互联方案选型、实测延迟与 Mkcloud 价格表

搜索"沪日 IX"的人，通常不是想买一台普通 VPS，而是已经在用或刚接触到日本方向的专线，并意识到传统的 IPLC/IEPL 在某些场景下还不够合适。如果只在零售 VPS 列表里翻，根本分不清 IX、IPLC、IEPL 三种沪日线路的差别。下文先把概念理清，再把 Mkcloud 沪日 IXP（日本方向）当前在售的流量计费和独享带宽套餐摆在一张表里做横评，把延迟、带宽、价格、放向和前置条件讲清楚。

## 一、先弄清：沪日 IX、IPLC、IEPL 分别是什么

沪日方向的专线有三种形态：

| 类型 | 入口（国内侧） | 出口（境外侧） | 是否需要云厂前置 | 是否需要运营商省份白名单 |
| --- | --- | --- | --- | --- |
| 沪日 IEPL/IPLC | 国内主流运营商或 BGP 入口 | 日本 BGP/指定机房 | 否 | 通常限定为单省份连入 |
| 上云互联优化专线（IX） | 在 IXP 内宣告，仅云厂可路由 | 日本 BGP（Nearoute, AS51847） | 是（阿里云/腾讯云/百度云/华为云/火山云/UCloud 等） | 否 |
| 普通国际线路 | 公网 | 公网 | 否 | 否 |

要点：

- IPLC/IEPL 多用运营商私线或企业专线，端内延迟典型值 25~28ms。
- IX（Internet Exchange Point）走的是云厂交换中心内网，入口 IP 不在公网广播，因此三大运营商、家宽、移动网络均不可直接连入，必须用一台已加入 IX 的云厂 ECS 作为前置。
- Mkcloud 给出的沪日 IX 端内参考延迟同样是 25~28ms，最终用户到目标服务的总延迟取决于前置云厂到 IX、再到 Mkcloud、再到目标服务的叠加开销。

如果用户在用沪日专线过程中碰到以下问题，那沪日 IX 这一形态需要被重新评估：

- 当前入口家宽被通报或扫描触发异常。
- 想换到一个对端带宽更高、流量档位更灵活的线路。
- 已经有阿里云/腾讯云/华为云等节点，需要一个"云上跳板直通日本"的入口。

## 二、Mkcloud 沪日 IX 套餐与配置全表

Mkcloud 是早期布局沪日方向 IXP 的服务商之一，主页把沪日 IXP 上云产品挂在 `/index.php/store/cloud-jp-sh` 下。当前端仍在售的沪日 IX 流量计费和独享带宽两套套餐，整理如下。

流量计费套餐（沪日上云互联优化-日本 BGP，入口走云厂优化通道，出口日本 BGP 25~28ms，每台 1 对 2 个独立 IP）：

| 流量档 | CPU/内存 | 带宽峰值 | 月流量/月 | 价格（人民币） | 购买 |
| --- | --- | --- | --- | --- | --- |
| 1TB/月 | 2 核 / 4GB | 200Mbps | 1TB | ¥166/月 | [ 查看 1TB 沪日 IX 套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 2TB/月 | 2 核 / 4GB | 300Mbps | 2TB | ¥268/月 | [ 查看 2TB 沪日 IX 套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 3TB/月 | 2 核 / 4GB | 500Mbps | 3TB（超量限速 10Mbps） | ¥358/月 | [ 查看 3TB 沪日 IX 套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 6TB/月 | 4 核 / 8GB | 1Gbps | 6TB（超量限速 20Mbps） | ¥688/月 | [ 查看 6TB 沪日 IX 套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 10TB/月 | 4 核 / 8GB | 1Gbps | 10TB | ¥1125/月 | [ 查看 10TB 沪日 IX 套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 20TB/月 | 4 核 / 8GB | 1Gbps | 20TB | ¥2150/月 | [ 查看 20TB 沪日 IX 套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 30TB/月 | 4 核 / 8GB | 2Gbps | 30TB | ¥3165/月 | [ 查看 30TB 沪日 IX 套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 50TB/月 | 8 核 / 8GB | 2Gbps | 50TB | ¥5222/月 | [ 查看 50TB 沪日 IX 套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |

> 注：以上 8 个档位是当前商店页在售的完整流量计费套餐，未做任何删减。3TB 与 6TB 档位在历史预售公告里写明"超量限速 10Mbps / 20Mbps"作为预留兜底规则；当前实际上是按月双向统计，超量后机器会暂停，可自助购买流量重置或工单补差价升级。

独享带宽套餐（沪日上云互联优化-日本 BGP 独享带宽，按带宽计费，无月流量上限）：

| 带宽档 | CPU/内存 | 带宽 | 价格（人民币） | 购买 |
| --- | --- | --- | --- | --- |
| 100Mbps | 2 核 / 4GB | 100Mbps | ¥1600/月 | [ 查看 100M 沪日 IX 独享套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 200Mbps | 2 核 / 4GB | 200Mbps | ¥3000/月 | [ 查看 200M 沪日 IX 独享套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 500Mbps | 8 核 / 8GB | 500Mbps | ¥6000/月 | [ 查看 500M 沪日 IX 独享套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 1Gbps | 28 核 / 64GB | 1Gbps | ¥9000/月 | [ 查看 1G 沪日 IX 独享套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 2Gbps | 28 核 / 64GB | 2Gbps | ¥16000/月 | [ 查看 2G 沪日 IX 独享套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |
| 5Gbps | 28 核 / 64GB | 5Gbps | ¥35000/月 | [ 查看 5G 沪日 IX 独享套餐](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh) |

> 独享价目表太大是历史特价频道的硬性范围，不同套餐还会与活动重叠；非活动期实际单价以商店购物车为准。👉 想要稳定速率、需要大带宽持续跑业务的，可直接 [👉 进入 Mkcloud 沪日 IX 选购](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh)。

补充说明：

- 沪日 IX 当前使用云厂 BGP 网络优化通道，仅允许阿里云、腾讯云、百度云国内全网，以及火山云、华为云、UCloud 华东等已对接的云厂网络连入；具体以下单页说明为准。
- 出口走日本 BGP 多线出口，AS51847（Nearoute），报道点 PCCWG、NTT、Cogent、Lumen、Telstra、HE、Voxility；既有 Equinix IX、JPIX、BBIX、JPNAP 多交换中心，也有 Google、Cloudflare、Valve、SGGS 私有 PNI。
- IX、IPLC、IEPL 都仅在国内内部、对端外部使用 1 对 2 个独立 IPv4（1 个独立入口 IP + 1 个独立出口 IP），机器是单端口单主机，出口不接收公网连入。

## 三、官方给出的端内参考延迟与实测样本

Mkcloud 在知识库里对外公开的沪日 IX 端内参考延迟为 25~28ms，这是云厂接入点到日本 BGP 出口之间的专线延迟，不含本地到云厂、出口到目标服务两端的本地耗时。

社区近期可检索到的沪日 IX 实测样本（来源 blog.ahu.moe / catcat.blog 等公开测评）：

- yabs（YABS-GitHub）上显示，机器使用的 CPU 为 AMD EPYC-Rome，Geekbench 6 单核约 1277、多核约 2313；fio 4k 随机读写约 250MB/s，1m 顺序读写约 430MB/s。
- iperf3 出向实测：东京 GSL 190Mbps 上传 / 184Mbps 下载，延时 25.7ms；洛杉矶 Clouvider 199Mbps 上传；新加坡 Leaseweb 205Mbps 上传；伦敦 Clouvider 179Mbps 上传。延迟 252~279ms 区间为公网部分。
- 日本 IIJ、KDDI、NTT、SoftBank、CN2 路由回程平均 26~28ms；上海 → 机器本地测试 211Mbps / 190Mbps / 32ms。
- 双向溯源 流媒体解锁：Disney+、Netflix、YouTube、Amazon Prime、ChatGPT 均识别为日本原生；Abema / Niconico / Hulu Japan 未解锁，与该机设置一致。

> 不同云厂前置、不同目标服务、不同时间段的实测结果都会有差别；上表数值仅供参考，不能作为 SLO。

## 四、IX 与传统专线优缺点对比

IX 架构在业内社区被讨论较多的两个点是：

- 入口 IP 在 IXP 内宣告，公网扫描不了，连带被云厂商通报/封禁的可能性变小。
- DDoS 压力被推到云厂前置侧，叠加运营商实名机制，整体追踪成本上升。

但这不是说 IX 适合所有用户：

1. DDoS 防护责任在用户侧。IX 不自带公网清洗能力，云厂遇到大规模异常会触发黑洞，且一般不在 SLA 范围内。
2. 通报责任前置。云厂走强实名，一旦出问题，责任和调查路径都会落在使用者侧。
3. 延迟上略多一跳。走 IX 内网多一段云厂前置链路，相比直连优化的传统 IPLC/IEPL，链路堆加更多一层。
4. 前置选型门槛不低。阿里云/腾讯云轻量应用服务器限速情况极端，火山云按量计费约 0.8 元/GB 长期用偏高，需要结合云厂、地域、带宽选择合适的云机。
5. 选用 IXP 的一个前提：已有合适的接入云厂。如果还没买云厂，直接遭 IPLC/IEPL 更具调调性。

因此，沪日 IX 适合这类用户：在云上已经走了 1 台或者多台云厂 ECS，主要做日本侧业务；要避免公网到家宽被扫描；能够接受走一条"云厂 → IX 交换中心（HKIX/Equinix/JPIX）→ Mkcloud 日本 → 目标"这条加长链路。

## 五、使用前必须看的前置限制

购买沪日 IX 产品之前，Mkcloud 自己公开的限制如下，依次对照可避免踩坑：

- 仅允许云厂 BGP 网络连入。家用宽带、移动网络、三网专属接入均不可。
- 实名要求。需要中国大陆手机号 + 身份证号 + 姓名一致；专线产品均需认证。
- 流量双向计费。上行、下行都计入月流量，超量后机器会暂停。
- 重置机制。可到商店自助购流量重置，或工单中处理。
- 不提供原生 IP / 业务账号保证。IP 仅为服务机 IP，不保证原生气、不保证流媒体长期解锁、不保证平台账号全部完好。
- 不能用作入站。不接受公网连入，不能用于公开站点、支付回调、对外游戏服务。

## 六、当前可用的优惠代码与活动状态

需要注意：Mkcloud 多次通过"五一""十一""双旦"发布循环优惠码，但在商店实际售卖页面，多数优惠码都标注"活动期内有效"。

- IXCLOUD——IXP 上云产品 6.9 折循环折扣（五一活动期间限定），适用于 2TB/3TB/6TB 档。
- JP-7.7——沪日 IXP 上云流量计费 7.7 折循环（五一活动期间限定）。
- MK-8.8——全场流量计费产品 8.8 折循环（双旦活动源，现已结束）。
- MK-7.8——独享带宽产品首月 7.8 折（双旦活动源，现已结束）。
- MK-NEW——新用户专享活动机 236 元/月（双旦活动源，现已结束）。
- MK-IPLC-WELCOME / MK-IEPL-WELCOME——IEPL/IPLC 全线 9 折循环据点（部分场景有效）。

到 2026 年初可见的常态化循环优惠仅限【IXCLOUD 6.9 折（上云互联 2TB/3TB/6TB）、JP-7.7 7.7 折（沪日 IXP）】；下单时直接填入购物车【优惠劵码】框位即可，依次试一下 6.9 折套餐的实价。

👉 需要 2TB 以上沪日 IX、想要拼 IXCLOUD 6.9 折的，[👉 进 Mkcloud 沪日 IX 选购](https://www.mkcloud.net/aff.php?aff=390&url=/index.php/store/cloud-jp-sh)。

## 七、怎么样判断适合不适合你

下列三种情形，沪日 IX 一般会是个靠谱选项：

- 平台业务需要日本原生 IP，例如 Amazon JP、乐天市场、雅虎日拍、电商后台、企划后台。IX 的日本 BGP 多面出口加多交换中心，路由質量较釈定。
- 手上已有云厂节点，不希望再去单独购买一台办公出口机器。
- 对扫描、封通知关切，且能接受自己顶置一套云备方案。

不需要 IX 的情形：

- 只做轻量访问。要访问几个公众号、Latin 资料、少量 VPN 流量，考虑 188 元的沪日 IPLC-500GB 共享专线型更合适。
- 要求对外支付回调、公开站点、点对点游戏端口。IX 及其他专线产品都不接收外部入站，需要另行选购带入站服务。
- 需劳动者人数在 30 人以上的。要求设初领带宽 SLA，可询问 Mkcloud 是否支持定制。

## 八、衡量沪日 IX 的几个参考指标

如果决定入手前要在多台 IX 沪日、以及同类沪日 IPLC/IEPL 之间哪个选，可以从这几个参考点来排到测验师：

| 维度 | 检查项 | 记录点 |
| --- | --- | --- |
| 延迟 | 端到端（云厂入口 → IX 终端 → 日本出口 → 业务后台） 12 个时段。不选瞬时值，看 30 分钟平均。 | 2 万个常用 业务后台 ，取中位。 |
| 带宽 | 实测 100MB+ 文件 UDP 下载。不记录示例峰值，记实际可持续平均值。 | YABS/Power 仓库有现成脚本。 |
| 协议 | 要看从你的云厂到 IX 的路由，不管业务后台在哪个 AS。 | bgp.he.net / bgp.tools。 |
| 解锁 | 需要哪些服务是否原生、必要组 TikTok/Amazon/Google Play 是否区分。 | 使用脚本一键检验。 |
| 售后 | 能不能处理云厂商黑洞 / 实名变更 / PUSH。 | 工单记录、直播记录、企业验证。 |

## 九、常见问题

Q1：弄不拥有云厂 ECS 能不能买沪日 IX？

不行。入口 IP 只在 IXP 内宣告，运营商、家宽、移动网络都选不到路径。补上一台阿里云/腾讯云/百度云/华为云/火山云/UCloud 中任意一家的云服务器ECS 就可以。

Q2：实名要求严格吗？

中国手机、身份证号、姓名一致性验证。需遵守中国法律，严禁价类型。

Q3：沪日 IX 和 IPLC 哪个好？

IPLC 不是云厂前置、是运营商私线，对起点到家宽、杢公胼家宽的用户更方便。IX 是云厂前置，输出公网曝召、动不动被扫描的概率更低，代价是多加一段云厂链路。

Q4：可以在同一台机器上启动多个转发服务吗？

技术上可以。同机中断会同时计入流量，计费按上行、下行双向迭加。

Q5：会出一个 IP 的时候跳出游戏住宅原生气吗？

官方资料表示不保证原生、住宅、流媒体解锁，也不能保证平台账号不被限制。与其拿"原型"为发夬依据，不如动能实测。

Q6：认购后能反悔吗？

24 小时内产品无质量问题不支持退款。要选之前决定。
