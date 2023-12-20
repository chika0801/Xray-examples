### :memo:

是的，请不要使用代理访问境内 IP 网站，这是一个基本实践问题，**因为你使用任何代理访问境内网站，代理的 IP 都会被记录、上传、标记。**

这套机制已经很成熟了，根据内部人士的消息，一旦“你”使用代理访问了境内 IP，“你”就会被标记为在使用此代理（甚至还会标注情商）。

1. 因为你直连那个 VPS，并且时间吻合，所以本地 IP 被标记了。
2. 你挂着代理打开了境内应用比如微信，于是...

以上是早已被部署的检测方式，所以实践中应在服务端屏蔽所有境内 IP。 [#0](https://github.com/XTLS/Xray-core/discussions/593#discussioncomment-845165)

---

[使用 wireguard 配置示例](./wireguard_for_v1.8.6_or_higher.md)

### :memo:

相对于 XTLS Vision 的使用基数，目前几乎没有收到 **配置正确** 的 Vision 被封端口的报告，**配置正确** 指的是：

1. 服务端使用合理的端口，禁回国流量
2. 只配置 XTLS Vision，不兼容普通 TLS 代理
3. 回落到网页，不回落/分流到其它代理协议
4. 客户端启用 uTLS（fingerprint） [#1.1](https://github.com/XTLS/Xray-core/issues/1544#issuecomment-1399194727)

---

首先，如果你特别不想被封，**请先选择一个干净的 IP**，并按照 **配置正确** 去搭建、使用 XTLS Vision。

**但是，即使你这样做了，也无法保证 100% 不被封**。自去年底始，很多人的未知流量秒封 IP，TLS in TLS 流量隔天封端口。XTLS Vision 不是未知流量，且完整处理了 TLS in TLS 特征，目前看来效果显著。**但这并不意味着，用 XTLS Vision 可以 100% 不被封，认识到这一点是非常、非常重要的，不要自己偶然被封就大惊小怪**。

**因为除了协议本身，还有很多角度能封你**。以 IP 为例，你无法保证 IP 真的干净，无法避免被邻居波及，无法避免整个 IP 段被重点拉清单。也有可能某些地区的 GFW 有独特的标准，比如某个 IP 只有寥寥数人访问连却能跑那么多流量，封。**如果你的 XTLS Vision 被封了，但没有出现去年底 TLS 那样的大规模被封报告，我真心建议你换端口、换 IP、换服务商依次试一遍**。

XTLS Vision 完全没有特征吗？也不是，我就可以把它封得很精准。此外，两年前我就想出了很多种角度来不带 collateral damage 地精准封锁 FQ 流量，一个不剩。~当时我连文章草稿都写好了，只是没发，还是不给 GFW 提供弹药了，万一他们还没想到~。

最后，没看过黑镜第一季第一集的，建议去看一遍。 [#1.2](https://github.com/XTLS/Xray-core/issues/1544#issuecomment-1402118517)

---

如果你之前用了其它协议导致 TCP/443 端口被封，**Vision 并没有“解封已经被封的端口”的能力**，换个 IP 或端口

如果这是你新开到的 IP，说明这个 IP 的 TCP/443 端口已经被别人搞封了，换个 IP 或端口 [#1.3](https://github.com/XTLS/Xray-core/issues/1670#issuecomment-1436240888)

### :memo:

看来好多人还不知道代码里 Vision 只支持纯净入站或另一个 Vision 入站，~当然要改也是不难的~ [#2.1](https://github.com/XTLS/Xray-core/issues/1612#issuecomment-1418829266)

---
  
其实我早就看到了这个问题 [#1500](https://github.com/XTLS/Xray-core/issues/1500) ，~只是不想改~

因为根据历史，机场会用 SS 或 VMess 中转 XTLS 出墙，XTLS 把苦力活全干了，还给 GFW 喂了大量数据，却对社区没有任何帮助
我觉得这样并不好，所以我不会去改它，当然 PR is acceptable [#2.2](https://github.com/XTLS/Xray-core/issues/1612#issuecomment-1418880212)

---

~这个 bug 是这样的~，要中转的话不能用 Vision，~但其实可以 REALITY H2 / gRPC~

~以前我只知道 SS / VMess 中转机场，现在 Trojan 也开始了~

我说一下这个问题在哪，你们中转这些协议，支持的客户端是多，但是会给用机场的小白传达一种错误信息：~机场都在用（名言）~

现在还在用 SS / VMess 的机场很多，但很少直接过墙了，大都是中转 / IPLC，而后者很贵，机场要赚钱的，~所以可想而知是什么~

它们的安全性详见 [#1811 (comment)](https://github.com/XTLS/Xray-core/discussions/1811#discussioncomment-5355075) ，~我是觉得那一层加密是自欺欺人，因为迟早全解密了，在 GFW 面前其实无异于裸 Socks5~ [#2.3](https://github.com/XTLS/Xray-core/issues/1844#issuecomment-1479639520)

### :memo:

现在可以直接配置 REALITY H2 服务端，实测 N 个请求只开一条 H2，延迟超低，纵享丝滑。"flow" 为空，"network" 改为 "h2" 即可。

另一种方式是配置 REALITY VLESS 回落至 H2C，它可以与 Vision 共存，但暂不建议。H2 自带 MUX，理论上也可以减轻 TLS in TLS 特征，是否有效仍需实测。~但若目标域名在白名单内，可能测不出区别。~ [#3.1](https://t.me/projectXtls/57)

---

与 VLESS 回落功能无关，我看了下群，都什么理解啊，主动探测连 REALITY 那关都过不去，还轮得到 VLESS 回落？

用了 REALITY，VLESS 的回落就不是给你回落到网站用的，是给 Vision 与 H2 / gRPC 同端口共存用的。 [#3.2](https://github.com/XTLS/Xray-core/issues/1769#issuecomment-1464820362)

---

REALITY 是 TLSv1.3，VLESS 有回落很正常，默认回落到 H2C 或 gRPC 就能共存了，~但这俩协议不一定不封端口，风险自负~

~其实我有个猜想，就是对于白名单网站，可能现在 GFW 并不分析流量模型，所以测不出来封不封端口~ [#3.3](https://github.com/XTLS/Xray-core/issues/1769#issuecomment-1464821647)

---

gun（gRPC）最初就是 @DuckSoft 看到 CloudFlare 支持 gRPC 回源后写的，不是“gRPC后来也发展到过CDN”。

REALITY 不能过免费 CDN，故 gRPC 与 H2 区别不大，由于 gRPC 是 over H2，**直接用 H2 相对省一点点**。
REALITY 支持 gRPC 是顺手写的，just for fun，~毕竟相比于 H2 大家更喜欢 gRPC，多 padding 一点可能还是好事？~

你可以看到 Xray-core 内 REALITY 的第一个 commit 就有 REALITY H2 客户端支持，本来是没打算支持 gRPC 的。
~但是 REALITY WS 就算了吧，这个组合属实没有必要。~ [#3.4](https://github.com/XTLS/Xray-core/discussions/1719#discussioncomment-5138312)

### :memo:

关于机场，说实话，我对机场落地这类技术，持保留态度。

从去年底乃至这些年的经验来看，**很多时候，GFW 的封锁策略优先讲究一个最多人用、最大收益，而不是你协议特征明不明显。**
TLS 类一疯狂，指纹和 TLS in TLS 检测就被重点安排上了，反而是小众的 UDP 类没有被针对、还可以用。
要说特征，其实混淆后的 UDP 包一眼假，检测起来比 TLS 类更容易，只是机场已经遍地 TLS 类，而 UDP 类还是自建居多。

那谁会成为靶子就很明显了，这也好理解，**假如你是 GFW 的供应商，最后交差个 FQ 封锁率才百分之几的东西，不太合适吧。**
肯定先找用的人多的下手，也就是机场喜欢用的那些什么 SS / VMess，什么 Trojan，针对研究，一封一片，效果拔群。

~所以~ [#4.1](https://github.com/XTLS/Xray-core/issues/1767#issuecomment-1464882669)

---

开混淆可以暂时解决“没有真正的 h3 server 而露馅”的问题，但是带来了另一个问题，**即变成了全随机数，它本身就是更明显的特征**

以前对于 SS 这类“全随机数是不是最大的特征”还有过争议，现在已经没有悬念了，**GFW 直接封了目标 IP 也不会有什么附带伤害**

根据目前的反馈，暂时只有部分地区的 GFW 把该策略应用到了 UDP，且暂时只是封端口，~但是一旦机场大规模上，就~ [#4.2](https://github.com/XTLS/Xray-core/issues/1767#issuecomment-1465101806)

### :memo:

不稀罕，你不说我差点忘了，去年我有个套 CF 的 WSS 遇到了不断升级的“智能墙”：

- 最初，WSS 被精准阻断（网站能上），研究发现用 [Browser Dialer](https://github.com/XTLS/Xray-core/pull/421) 就能解决，所以是 Golang WSS 指纹被针对了。
- 不久后，又被精准阻断，**研究发现若一段时间内用浏览器打开过网页，WSS 才能用，加个自动请求解决了。**
- 最后，众所周知，TLS in TLS 检测被部署了，CF 节点倒没被直接封端口，但即时丢包干扰更恶心，相信不少人都深有体会。 [#5.1](https://github.com/XTLS/Xray-core/issues/1750#issuecomment-1459340564)

---

顺便，我说一下 WSS 代理为什么能被精准识别：

- **指纹：即使开了伪装，它发的 ALPN 始终为** `http/1.1~`，**一眼 WSS，实际上无法做到我们想要的“藏木于林”，只会裸送人头。**
- 握手：WSS 内层的 WS 要多握手一次，时序特征非常独特。其实开 [early data](https://github.com/XTLS/Xray-core/pull/375) 可以缓解，若不得不用 WSS，建议 `?ed=2048`
- TLS in TLS：这是 TLS 代理普遍存在的特征，需要针对性处理。多路复用可以缓解内层 TLS 握手特征，但却加重了“加密套娃”的特征，参考 [**XTLS Vision, TLS in TLS, to the star and beyond**](https://github.com/XTLS/Xray-core/discussions/1295) #1295 第二大段，所以目前 XTLS Vision 是较优解法。

**所以我现在的建议是：不要用 WSS，并且它应当被列为 deprecated**。套 CDN 有 gRPC，直连有 N 种姿势，已无任何必要用 WSS。 [#5.2](https://github.com/XTLS/Xray-core/issues/1750#issuecomment-1459469821)

### :memo:

> ~当然也有可能是被疯狂主动探测，记录握手超时时间，看像不像 Xray 的默认 60 秒~

对于这一点，我建议大家修改一下 policy 的 handshake 和 connIdle 等，不要用默认值，不然特征太明显

~中间人多收集些数据，分析出握手 60 秒超时 + 连接 300 秒超时，这不是 *ray 还能是啥~ [#6](https://github.com/XTLS/Xray-core/issues/1511#issuecomment-1376887076)

### :memo:

1. [**XTLS-REALITY** 自己偷自己时，**serverName**填的域名与实际**SSL**证书包含的域名不一致时，也能连接](https://github.com/XTLS/Xray-core/issues/1681#issuecomment-1436655742) #1681 (comment)
2. ~总有人问这个问题我是没想到的~，我系统性地回答一下：首先对于非 REALITY 客户端，REALITY 服务端只是端口转发。其次你直接访问 https://IP ，浏览器发的 TLS Client Hello 中不含 SNI，HTTP 头中的 Host 也不对，此时会得到何种响应完全取决于目标网站的策略，大概率会得到奇奇怪怪的响应，这是正常的，当然你的浏览器还会报证书不符。最后若你想用浏览器验证 REALITY 的端口转发，正确的做法是修改系统 hosts 文件，将目标域名指向你服务端的 IP，再用浏览器直接访问目标域名，可以访问即正常，并且你可以在浏览器 F12 的 Network 中看到实际上连接的是你服务端的 IP。 [#7](https://github.com/XTLS/Xray-core/discussions/1800#discussioncomment-5321705)

### :memo:

其实 @tdjnodj 的想法是有一定价值的。

我先纠正一个常见的错误观点，**“封了就是被识别，识别了就一定封”，其实是不对的。**

* 有时候封你真的只是“范围攻击”，比如特殊时期，很多仅建站的 IP 也会被封
* **很多时候识别了却不立刻封，是因为留着可以匹配一下流量包长、时间，推出你可能上了哪些网站、你是 tg 上哪位等，GFW 没少这么干**
  对于 SS / VMess 这类缺乏前向安全的协议，GFW 还能通过云服务拿到密码，直接解密你以前、以后的所有流量，你干了什么它一清二楚
  这个“云服务”包括不限于手机应用云备份、输入法上传数据等，就算你都关了，你总装有国产软件吧，~某浏览器插件直接上传你浏览记录~

**直接封了你，你反而会换用那些更难被识别、监控的协议，所以说大多数时候识别了也不必封，留着监控更有价值，这是 GFW 的基本操作。** 但是有些时期，上面要求的是封锁率、要看到封锁的效果，GFW 就会把识别出来的协议封掉，~比如现在~，但这种情况不会一直持续。

所以 @tdjnodj 想法的价值就在于，**不严的时候，我们可以在 REALITY 外面套一个已知被识别，但被留着监控的协议**，可以是 Socks5 / HTTP / Shadowsocks / VMess / Trojan / VLESS without flow 等等，让 GFW 错误地以为我们在访问 [www.bing.com](http://www.bing.com) ，~碟中谍之我预判了你的预判~。

这一想法扩展了 REALITY 的应用场景，毕竟直接使用 REALITY 的对外表现为端口转发，万一被无脑封，~说不定这一想法会上位成主流玩法~。 [#8](https://github.com/XTLS/Xray-core/discussions/1811#discussioncomment-5355075)

### :memo:

这个 issue 我没看，只想回复这一句：

> 不了解背后的代码实现，但是 shadowTLS 目前是可以国内外域名通吃，几乎不挑域名（v3需要挑域名），不知道为何reality对域名的要求这么严格，求大神解答或者等正式的release吧

[XTLS/REALITY#2 (comment)](https://github.com/XTLS/REALITY/pull/2#issuecomment-1479956295)

**简单来说，不是能不能的问题，而是应不应该的问题，这些协议握手时要连接目标服务端，你的国外机器填个国内域名合适吗**

这个原因是非常显而易见的啊，写模板时我以为一笔带过提醒一下，大家就明白了，~真的是我高估了~ [#9.1](https://github.com/XTLS/Xray-core/issues/1891#issuecomment-1499073501)

---

> 能不能在网站标准里提一下不能用被墙的网站和有国内镜像的网站，我试一次封几分钟IP，才想明白这件事

啊，这个还要说吗，对不起，是我高估了大家的。。。

https://twitter.com/kkitown/status/1636277251179438081 这位更是重量级

其实非要填国内网站，也不是不行，问题是，人家又没放国外机器上，其次，会产生各种回国流量，一眼 REALITY 加端口转发 [#9.2](https://github.com/XTLS/REALITY/pull/2#issuecomment-1479956295)

### :memo:

> > > ~当然也有可能是被疯狂主动探测，记录握手超时时间，看像不像 Xray 的默认 60 秒~
> > 
> > 
> > 对于这一点，我建议大家修改一下 policy 的 handshake 和 connIdle 等，不要用默认值，不然特征太明显
> > ~中间人多收集些数据，分析出握手 60 秒超时 + 连接 300 秒超时，这不是 *ray 还能是啥~
> 
> 
> 是不是可以理解：
> ```
> * 回落仍然是必要的
> 
> * 如果可以Nginx前置的情况（非xtls）前置更好一点
> ```

回落当然是必要的，尤其是现在我们大规模用 uTLS 模仿浏览器指纹，GFW 一个探测，没网页的话岂不是一眼假？

服务端指纹特征是一个值得解决的问题。 [#10.1](https://github.com/XTLS/Xray-core/issues/1511#issuecomment-1382042986)

---

我看到 sing-box 的 Trojan 有回落，不过有这样一段话：

> 没有证据表明 GFW 基于 HTTP 响应检测并阻止 Trojan 服务器，并且在服务器上打开标准 http/s 端口是一个更大的特征。

~其实去年就看到了，并且去年我还看到隔壁也这么说，没有证据表明 balabala，不知道“回落无用论”又是什么政治正确还是~

**还是想得不够多。**

GFW 有没有区别对待有/无回落的服务器，目前没有人对比测试过，但一个很浅显的道理是：

**当你发现没有回落好像也不会被封时，有没有一种可能，正是因为绝大多数人都配置了回落，GFW 才没把它纳入封锁依据。** **如果大家的代理服务器普遍没有回落，那么会是一个谁都看得出来的、送人头的特征，GFW 一定会将其纳入封锁依据。**

多想一步，就能推出“回落无用论”是错的。鼓励大家不配回落，更是自废武功，~GFW 喜闻乐见~。

当然现在我更推荐 TLS 级别的回落，也就是 REALITY，解决了传统回落的指纹问题，~VLESS 回落的文章还咕着就又成传统的了~。 [#10.2](https://github.com/XTLS/Xray-core/pull/1916#issuecomment-1500457011)

---

> > 当然现在我更推荐 TLS 级别的回落，也就是 REALITY，解决了传统回落的指纹问题，VLESS 回落的文章还咕着就又成传统的了。
> >
> 可以理解成是推俗称的自己偷自己吗？😂

是的，而且解决了 TLS 最令人诟病的 CA 问题，并且限制了客户端只能用浏览器指纹，都更安全，~早预告过 REALITY 是默秒全~。

关于回落是否有必要，之前预告 REALITY 时，我也以另一个角度评论过：[#1511 (comment)](https://github.com/XTLS/Xray-core/issues/1511#issuecomment-1382042986)

> 回落当然是必要的，尤其是现在我们大规模用 uTLS 模仿浏览器指纹，GFW 一个探测，没网页的话岂不是一眼假？

现在的情况是，Golang 的 TLS 指纹早已明显被针对了，于是我们不得不大规模用浏览器指纹。

然后 GFW 天天看你用浏览器访问某个网站，好奇探测一下，连网页都没，这，不太合适吧，~当 GFW 傻~。 [#10.3](https://github.com/XTLS/Xray-core/pull/1916#issuecomment-1500491248)

### :memo:

看到近期群里的一些发言，真是令人无语，有没有一点基本的 Linux 和编程常识啊。

**Xray 占几百兆内存，并不代表这是最低要求，而是正是因为你有空闲的内存，Xray 才会拿来当缓存、备用，因为不用白不用。**

仅此而已，内存完全够用的情况下，却非要追求这个数据的好看，想捂着不让 Xray 用，有什么意义呢？VPS 商家给你退钱？

---

**对于 Xray 这样的代理类软件，内存占用大头在于对被代理数据的缓存，能用的内存多就能多缓存一些数据，麻烦搞清楚状况。** [#11.1](https://github.com/XTLS/Xray-core/issues/1880#issuecomment-1505982997)

---

换句话说，内存占用大头取决于你要的缓存数据能力，**每个代理软件的默认策略不一样**，你调低缓存自然就可以实现数据的好看。 [#11.2](https://github.com/XTLS/Xray-core/issues/1880#issuecomment-1506049230)

### :memo:

目标网站/域名的选择会极大程度地影响 REALITY 代理的延迟、速度、稳定性等：

1. 至少目前，REALITY 每次都要去拿握手包，需要注意目标网站近不近、稳不稳定（请求多了就把你半拉黑也是一种不稳定）。
2. 运营商层面可能会给某些域名更高的流量优先级，拥堵时优先保证它们的流量通过。
3. GFW 层面至少有黑名单（google）和白名单（microsoft），可能还有其它名单，比如偶尔干扰/限速名单（github？）

你们对照排查一下。 [#12.1](https://github.com/XTLS/Xray-core/issues/2017#issuecomment-1532345891)

---

~也可能是你们天天逮着 microsoft、apple 之类的偷，GFW 开始测试了~，有人说伊朗那边就有运营商在“内测” yahoo 的 IP 白名单。

REALITY 以后会出个缓存模式，提前采集目标网站的特征，就不用每次都去拿了，这也是相对于 ShadowTLS 之类的优势之一。

还有就是 REALITY 隐藏玩法的任意 SNI、无 SNI，对 REALITY 来说，只要服务端 serverNames 写了，客户端 serverName 就能填。 我需要说明一下不是只有 1.1.1.1 和 8.8.8.8，而是绝大多数网站都有“默认证书”。不过不希望这个玩法泛滥，因为特征明显。 [#12.2](https://github.com/XTLS/Xray-core/issues/2017#issuecomment-1532359978)

### :memo:

顺便先简单说一下 v1.8.1 [增强版 XUDP](https://github.com/XTLS/Xray-core/issues/1963#issuecomment-1512532299) 的 [Global ID & UoT Migration](https://xtls.github.io/development/protocols/muxcool.html#%E6%96%B0%E5%BB%BA%E5%AD%90%E8%BF%9E%E6%8E%A5-new) 有什么效果：

v1.8.1 以前，你用任何 UoT，假设服务端用 A 端口与多目标通信，若 TCP 断了，比如切换网络，重连后服务端会改用 B 端口。 v1.8.1 开始，你用 VLESS（包括 Mux.Cool），即使 TCP 断了，重连后服务端还是会用 A 端口。

尤其是，对 P2P 有奇效。从某种程度上来说，这才是真正的 FullCone。双端 Xray-core v1.8.1+ 自动启用，无需额外配置。

可以用 [NatTypeTester](https://github.com/HMBSbige/NatTypeTester)，先连接家里 WiFi 测一下，再连接手机热点（流量）测一下，你会发现服务端出口端口没变，~挺神奇的。~

~更多内容，咕咕咕，请等文章。~ [#13](https://github.com/XTLS/Xray-core/issues/2017#issuecomment-1532488765)

### :memo:

都是 TLS，但怎么用 TLS，是有讲究的，有句话我早就想对鼓吹 Trojan 平替 VLESS 的人说：**真以为 Trojan 能用一辈子？** 早在三年前的 VLESS BETA 我就给你们说过，光套一层加密并不能掩盖里面的时序特征，所以 VLESS 有 flow 机制。 但是呢，以前的 GFW 没上手段，简单套个 TLS 在实践上的确还可以用，就像 WSS ALPN 一直很明显，但以前它能用。 它们还能用，我就没必要提前出牌，等 GFW 上了手段，我再继续出牌，并且不推荐大家再用旧的 WS、无 flow 等。

有一点需要再次强调，我支持的始终是 TLS 上的百花齐放，而不是 TCP 上的，原因以前说过很多，可以去 [v2ray](https://github.com/v2ray/v2ray-core/issues/2523#issuecomment-636548331) 翻翻。 前段时间不是有个论文嘛，~算了不想说了，有空时再评论。~ [#14](https://github.com/XTLS/Xray-core/issues/2017#issuecomment-1532568938)

### :memo:

还是简单说一下各协议 2023 现状（对于中国大多数地区）

1. SS 全随机数类秒封 IP；IPv6 不一定封，~因人品而异~；绕过“省钱规则”曾经不封，目前不知道，但若流行了肯定会封，参考 SSR
2. Trojan、WSS 隔天封端口；Cloudflare 不封但干扰会很严重，因地区而异
3. 黑名单是单连接 TLS in TLS 握手典型特征，因为用强 padding（Vision）或开 mux 就能绕过，注意不要让猪队友客户端连接
4. REALITY 类偷白名单域名的话即使有上述特征也不封；甲骨文等太黑的 IP 段偷大厂/偷别人不一定连得上
5. Hysteria、TUIC 不一定封，因配置、地区而异；可能会遇到 QoS 限速，因运营商而异；总之就是使用体验严重因人而异

所以你可以看到以前的流行协议在今年是什么样的存活状况，**事实上今年自建的大都是新协议，非 IPLC 中转用的协议原理也没差** **你的主观印象中“今年能连接国外网络的人数并没有减少”，严格来说就是因为自建，一些人把它透明化了，卖中转给机场和个人** [#15](https://github.com/XTLS/Xray-core/issues/2317#issuecomment-1637142176)
