

## 语音合成技术调研之如何低成本实现完美TTS

### 序

本篇重点：一个是调研，一个是寻找的目标完美的TTS

本篇介绍了笔者在自己能力范围内寻找完美机器声音的过程，有些原理、概念、笔者也是初步了解，本篇大家要抱着不求甚解的心态，语音篇笔者有可能做个系列，后面可能针对其中某一方面技术，或者理论深入探讨学习，本篇不会深入解析。见谅！

### 基础概念

声音是一种波动，当演奏乐器、拍打一扇门或者敲击桌面时，声音的振动会引起介质——空气分子有节奏的振动，使周围的空气产生疏密变化，形成疏密相间的纵波，这就产生了声波，这种现象会一直延续到振动消失为止。

声音总可以被分解为不同频率不同强度正弦波的叠加。这种变换（或分解）的过程，称为傅里叶变换。因此，一般的声音总是包含一定的频率范围。人耳可以听到的声音的频率范围在20到2万赫兹（Hz）之间。

- 频率：音调越高，频率越大；音调越低，频率越小。（介质相同时，fλ成反比）人耳可以感知到的声音，其频率范围为20 Hz至20,000 Hz

- 波长：音调越高，波长越短；音调越低，波长越长。（介质相同时，fλ成反比）在标准状况下的空气中，音波对应的波长从17 m至17 mm之间。有时，。

- 波数:波数和其方向则会用波矢表示

- 振幅：音量（响度）越大，振幅越大；音量越小，振幅越小。

- 声压

- 音强

- 音速：音速及其方向会用速度矢量来表示

- 音色：即波形 （音色主要决定于声音频谱对人的刺激，但也决定于波形、声压、频谱的频率位置和频谱对人的时间性刺激。）音色换句话说，一个物体发生的同时，会发出很多不同频率的波（谐波）。这许多不同频率的波由于相位差很小（也就是相隔时间很短），人是无法单独分辨的，所以这些波会混合起来一起给人一个整体的感受，而这个感受就叫做音色

- 音素

- 梅尔谱图、F0、频谱包络、LSP 或 LPC 系数

- Griffin-Lim 

  (当发音体越短、越细、越紧、越薄时，音调越高、频率越大、波长越短；发音体越长、越粗、越松、越厚时，音调越低、频率越小、波长越长)

### TTS市场背景

- 从文本生成自然语音（语音合成，TTS）研究了几十年仍然是一项有挑战的任务。这一领域的主导技术随着时代的发展不断更迭。我们为什么要写一篇关于语音合成的文章，因为技术壁垒，以为TTS市场巨大，被头部公司垄断，并且收费，普通玩家无法使用完美的TTS。总之，因为语音合成潜在价值，我们想去探寻市面上，主流大厂是如何做到的。

- 现在TTS 市场如何？以百度语音为例： 看看各个厂商都是如何收费的：百度语音离线 

| 购买授权数       | 每个授权单价 |
| ---------------- | ------------ |
| 第100～1000个    | 5元/个       |
| 第1001～5000个   | 3元/个       |
| 第5001～20000个  | 2.5元/个     |
| 第20001～50000个 | 2元/个       |
| 大于50000个      | 线下商务沟通 |

https://www.cnblogs.com/cr330326/p/10796420.html

中国知网 关于语音合成相关论文

|      |                                                              |                                                              |                                                              |                  |      |                                                              |                             |                                                              |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------- | ---- | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| 1    | [HMM与神经网络相融合的低资源语音合成方法](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=1&recid=&FileName=JSJZ202112042&DbName=CJFDLAST2022&DbCode=CJFD&yx=&pr=&URLID=) | [帕丽旦·木合塔尔](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=帕丽旦·木合塔尔&scode=000019713950&acode=000019713950); [吾守尔·斯拉木](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=吾守尔·斯拉木&scode=000036835541&acode=000036835541); [买买提阿依甫](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=买买提阿依甫&scode=000022601133&acode=000022601133) | [计算机仿真](https://kns.cnki.net/KNS8/Navi?DBCode=CJFD&BaseID=JSJZ) | 2021-12-15       | 期刊 |                                                              | [111](javascript:void(0);)  | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWpZWdCdGSM9mVjB3azRHcQB1YFRDWRFjeBFERTl0dThjQ1UVSqBzbXBjU5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CJFDLAST2022) |
| 2    | [基于BERT的端到端语音合成方法](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=2&recid=&FileName=JSJA20211206003&DbName=CAPJLAST&DbCode=CAPJ&yx=Y&pr=&URLID=50.1075.TP.20211207.0015.004) 网络首发 | [安鑫](https://kns.cnki.net/KNS8/Detail?sdb=CAPJ&sfield=作者&skey=安鑫&scode=000007380043&acode=000007380043); [代子彪](https://kns.cnki.net/KNS8/Detail?sdb=CAPJ&sfield=作者&skey=代子彪&scode=000010367352&acode=000010367352); [李阳](https://kns.cnki.net/KNS8/Detail?sdb=CAPJ&sfield=作者&skey=李阳&scode=000031401425&acode=000031401425); [孙晓](https://kns.cnki.net/KNS8/Detail?sdb=CAPJ&sfield=作者&skey=孙晓&scode=000028747410&acode=000028747410); [任福继](https://kns.cnki.net/KNS8/Detail?sdb=CAPJ&sfield=作者&skey=任福继&scode=000038579056&acode=000038579056) | [计算机科学](https://kns.cnki.net/KNS8/Navi?DBCode=CAPJ&BaseID=JSJA) | 2021-12-07 13:50 | 期刊 |                                                              | [355](javascript:void(0);)  | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWLJnV14kVQdla0RWdyo0ZpNXZFJXWBRlcyo0YxsWMoNFUKVDU3gVc25WW5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CAPJLAST) |
| 3    | [基于BERT预训练语言模型的印尼语语音合成](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=3&recid=&FileName=YNDZ202106005&DbName=CJFDLAST2021&DbCode=CJFD&yx=&pr=&URLID=) | [赵立铉](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=赵立铉&scode=000043739769&acode=000043739769); [杨鉴](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=杨鉴&scode=000038834014&acode=000038834014) | [云南大学学报(自然科学版)](https://kns.cnki.net/KNS8/Navi?DBCode=CJFD&BaseID=YNDZ) | 2021-11-10       | 期刊 |                                                              | [67](javascript:void(0);)   | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GW2RTYyQUVGJldIhncGlXbMVWauBDTkJFTwN2RFpGMR92MxhGZrR2Q4lFT5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CJFDLAST2021) |
| 4    | [基于迁移学习的自适应语音合成](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=4&recid=&FileName=SJTX202105012&DbName=CJFDLAST2021&DbCode=CJFD&yx=&pr=&URLID=) | [孙志宏](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=孙志宏&scode=000043514348&acode=000043514348); [叶焱](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=叶焱&scode=000025756969&acode=000025756969); [刘太君](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=刘太君&scode=000025729271&acode=000025729271); [许高明](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=许高明&scode=000004472327&acode=000004472327) | [数据通信](https://kns.cnki.net/KNS8/Navi?DBCode=CJFD&BaseID=SJTX) | 2021-10-28       | 期刊 |                                                              | [179](javascript:void(0);)  | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWrhkUY1mTYF0RsJER3QGbR50MXZENSlGTFlXN2tWUhFVQ1UVSqBzbXBjU5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CJFDLAST2021) |
| 5    | [不同粒度嵌入单元的端到端语音合成技术研究](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=5&recid=&FileName=XDJS202124003&DbName=CJFDLAST2021&DbCode=CJFD&yx=&pr=&URLID=) | [姑丽斯坦·奥布力喀斯木](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=姑丽斯坦·奥布力喀斯木&scode=000043117190&acode=000043117190); [帕力旦·吐尔逊](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=帕力旦·吐尔逊&scode=000023500351&acode=000023500351); [艾斯卡尔·艾木都拉](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=艾斯卡尔·艾木都拉&scode=000036958651&acode=000036958651) | [现代计算机](https://kns.cnki.net/KNS8/Navi?DBCode=CJFD&BaseID=XDJS) | 2021-08-25       | 期刊 |                                                              | [64](javascript:void(0);)   | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWHJ1L3EXN6ZVeG9yKJd3QnhVamVGNYVGNzJnQpBnUhFVQ1UVSqBzbXBjU5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CJFDLAST2021) |
| 6    | [一种基于改进注意力机制的实时鲁棒语音合成方法](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=6&recid=&FileName=XXCN20210811002&DbName=CAPJLAST&DbCode=CAPJ&yx=Y&pr=&URLID=11.2406.TN.20210811.1632.010) 网络首发 | 唐君;张连海;李嘉欣                                           | [信号处理](https://kns.cnki.net/KNS8/Navi?DBCode=CAPJ&BaseID=XXCN) | 2021-08-12 09:42 | 期刊 |                                                              | [246](javascript:void(0);)  | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWvYketpnUFZWc1I1ZrI1TqtyT5kzYzNXUJZDZxsWMoNFUKVDU3gVc25WW5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CAPJLAST) |
| 7    | [人机交互系统中语音合成技术的研究](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=7&FileName=1021125698.nh&DbName=CMFDTEMP&DbCode=CMFD) | [朱禹](https://kns.cnki.net/KNS8/Detail?sdb=CMFD&sfield=作者&skey=朱禹&scode=000044186110&acode=000044186110) | [辽宁工业大学](https://kns.cnki.net/KNS8/Navi?DBCode=CMFD&BaseID=GLNGC) | 2021-06-01       | 硕士 |                                                              | [86](javascript:void(0);)   | [**下载**](https://kns.cnki.net/KNS8/download?filename=FNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GW=0zauFmTCpVcXZVNCNVUwdmY5Fkd5JlewUnWvImMlBTW2FGMy5WW5VnViN0Q5gXOIZVR5hXUNdGN5M&tablename=CMFDTEMP) |
| 8    | [基于强制单调注意力机制的改进Tacotron2语音合成方法](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=8&FileName=1021900176.nh&DbName=CMFDTEMP&DbCode=CMFD) | [王庆尧](https://kns.cnki.net/KNS8/Detail?sdb=CMFD&sfield=作者&skey=王庆尧&scode=000047101507&acode=000047101507) | [哈尔滨工业大学](https://kns.cnki.net/KNS8/Navi?DBCode=CMFD&BaseID=GHGDU) | 2021-06-01       | 硕士 |                                                              | [47](javascript:void(0);)   | [**下载**](https://kns.cnki.net/KNS8/download?filename=FNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GW=0zcjZEVrBndXhlMpVkZXJmW5Fkd5JlewUnWvImMlBTW2FGMy5WW5VnViN0Q5gXOIZVR5hXUNdGN5M&tablename=CMFDTEMP) |
| 9    | [基于深度学习的中文语音合成方法研究](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=9&FileName=1021695470.nh&DbName=CMFD202201&DbCode=CMFD) | [王鹤](https://kns.cnki.net/KNS8/Detail?sdb=CMFD&sfield=作者&skey=王鹤&scode=000030529840&acode=000030529840) | [大连理工大学](https://kns.cnki.net/KNS8/Navi?DBCode=CMFD&BaseID=GDLLU) | 2021-06-01       | 硕士 |                                                              | [224](javascript:void(0);)  | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWoFzLr1GSxVHWRN3byJ1bRZ1UIlzQuVTdYp3QyRDSTBVWtJFe5ADVU9iT5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CMFD202201) |
| 10   | [激光侦听及智能语音处理技术研究](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=10&FileName=1021900026.nh&DbName=CMFDTEMP&DbCode=CMFD) | [段长昊](https://kns.cnki.net/KNS8/Detail?sdb=CMFD&sfield=作者&skey=段长昊&scode=000011636966&acode=000011636966) | [哈尔滨工业大学](https://kns.cnki.net/KNS8/Navi?DBCode=CMFD&BaseID=GHGDU) | 2021-06-01       | 硕士 |                                                              | [36](javascript:void(0);)   | [**下载**](https://kns.cnki.net/KNS8/download?filename=FNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GW=0zZNlnVVFTe4JVepVkZXJmW5Fkd5JlewUnWvImMlBTW2FGMy5WW5VnViN0Q5gXOIZVR5hXUNdGN5M&tablename=CMFDTEMP) |
| 11   | [多项式函数拟合实现汉语声调的语音合成](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=11&recid=&FileName=XKXB202103017&DbName=CJFDLAST2021&DbCode=CJFD&yx=&pr=&URLID=) | [李建文](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=李建文&scode=000029438467&acode=000029438467); [王咿卜](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=王咿卜&scode=000012972134&acode=000012972134) | [西安科技大学学报](https://kns.cnki.net/KNS8/Navi?DBCode=CJFD&BaseID=XKXB) | 2021-05-31       | 期刊 |                                                              | [148](javascript:void(0);)  | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWBdFdvEzKJh0RyoGdiF0Y1sUMzEDa1pneEhEcxQTQaZzL6hWZrR2Q4lFT5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CJFDLAST2021) |
| 12   | [基于端到端的富有韵律的语音合成系统的研究与实现](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=12&FileName=1021130134.nh&DbName=CMFD202201&DbCode=CMFD) | [牛伟华](https://kns.cnki.net/KNS8/Detail?sdb=CMFD&sfield=作者&skey=牛伟华&scode=000043677632&acode=000043677632) | [北京邮电大学](https://kns.cnki.net/KNS8/Navi?DBCode=CMFD&BaseID=GBYDU) | 2021-05-27       | 硕士 |                                                              | [83](javascript:void(0);)   | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWyIVcwQzbHp0TlZGN3JUUPVWePd0RvUmMNVTTrcDSTBVWtJFe5ADVU9iT5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CMFD202201) |
| 13   | [基于单元向量表征的拼接语音合成方法研究](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=13&FileName=1021076401.nh&DbName=CDFDLAST2021&DbCode=CDFD) | [周骁](https://kns.cnki.net/KNS8/Detail?sdb=CDFD&sfield=作者&skey=周骁&scode=000006847376&acode=000006847376) | [中国科学技术大学](https://kns.cnki.net/KNS8/Navi?DBCode=CDFD&BaseID=GZKJU) | 2021-05-24       | 博士 |                                                              | [118](javascript:void(0);)  | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWHdkd0RDdaZERTt2TBd2Vh1ESycHV2lHeip0bZdXN2NHbEFkN6JmQ1sSV5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CDFDLAST2021) |
| 14   | [智能泵站平台人机交互中语音识别和语音合成的研究与设计](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=14&FileName=1021075845.nh&DbName=CMFD202102&DbCode=CMFD) | [姚锦玮](https://kns.cnki.net/KNS8/Detail?sdb=CMFD&sfield=作者&skey=姚锦玮&scode=000019818872&acode=000019818872) | [中国科学技术大学](https://kns.cnki.net/KNS8/Navi?DBCode=CMFD&BaseID=GZKJU) | 2021-05-24       | 硕士 |                                                              | [176](javascript:void(0);)  | [**下载**](https://kns.cnki.net/KNS8/download?filename=IZVR5hXUNdGN5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GW==wL0dGTmJkR1MHMMZUbrF2ZzU3bW5kbyskbkBTd24GTNpWbQFnNBV3SMdmW1I0V6R1QV9US3A3ci9iT5VnViN0Q5gXO&tablename=CMFD202102) |
| 15   | [基于深度学习的汉语语音合成技术研究](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=15&FileName=1021799661.nh&DbName=CMFDTEMP&DbCode=CMFD) | [牛芳](https://kns.cnki.net/KNS8/Detail?sdb=CMFD&sfield=作者&skey=牛芳&scode=000047180548&acode=000047180548) | [新疆大学](https://kns.cnki.net/KNS8/Navi?DBCode=CMFD&BaseID=GXJDU) | 2021-05-22       | 硕士 |                                                              | [38](javascript:void(0);)   | [**下载**](https://kns.cnki.net/KNS8/download?filename=FNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GW=0TUXFTOMdnRPNVVXlkQGhXV5Fkd5JlewUnWvImMlBTW2FGMy5WW5VnViN0Q5gXOIZVR5hXUNdGN5M&tablename=CMFDTEMP) |
| 16   | [基于表征学习的语音合成声学建模方法研究](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=16&FileName=1021072869.nh&DbName=CDFDLAST2021&DbCode=CDFD) | [张雅洁](https://kns.cnki.net/KNS8/Detail?sdb=CDFD&sfield=作者&skey=张雅洁&scode=000041253212&acode=000041253212) | [中国科学技术大学](https://kns.cnki.net/KNS8/Navi?DBCode=CDFD&BaseID=GZKJU) | 2021-05-11       | 博士 |                                                              | [173](javascript:void(0);)  | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GW3g3NGp3bslTYiVTTW1Wbi1ESycHV2lHeip0bZdXN2NHbEFkN6JmQ1sSV5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CDFDLAST2021) |
| 17   | [藏语卫藏方言的语音合成技术研究](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=17&FileName=1021699555.nh&DbName=CMFD202102&DbCode=CMFD) | [谢香腾](https://kns.cnki.net/KNS8/Detail?sdb=CMFD&sfield=作者&skey=谢香腾&scode=000009089824&acode=000009089824) | [西藏大学](https://kns.cnki.net/KNS8/Navi?DBCode=CMFD&BaseID=GXZDX) | 2021-05-08       | 硕士 |                                                              | [72](javascript:void(0);)   | [**下载**](https://kns.cnki.net/KNS8/download?filename=IZVR5hXUNdGN5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GW==gQrNUVrF0aIhXYihTUWZHa2M3Y3RkQHJFT2gEd6N0bJFEOxV2QVpGUlNnemBVRLpFcOVlb5oGOl9iT5VnViN0Q5gXO&tablename=CMFD202102) |
| 18   | [基于CVAE的语气语音合成及在便携式同声翻译器的应用](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=18&FileName=1021645481.nh&DbName=CMFD202102&DbCode=CMFD) | 王研                                                         | [东华大学](https://kns.cnki.net/KNS8/Navi?DBCode=CMFD&BaseID=GDHUU) | 2021-05-01       | 硕士 |                                                              | [65](javascript:void(0);)   | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWI9kWlVTU2MHeGNDR4YlVutWZp9CcPhle012Y0Y0QjdDUtJFe5ADVU9iT5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CMFD202102) |
| 19   | [基于深度学习的语音合成与转换技术综述](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=19&recid=&FileName=JSJA202108029&DbName=CJFDLAST2021&DbCode=CJFD&yx=A&pr=&URLID=50.1075.TP.20210421.1321.024) | [潘孝勤](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=潘孝勤&scode=000016604717&acode=000016604717); [芦天亮](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=芦天亮&scode=000036814487&acode=000036814487); [杜彦辉](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=杜彦辉&scode=000038919751&acode=000038919751); [仝鑫](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=作者&skey=仝鑫&scode=000003403249&acode=000003403249) | [计算机科学](https://kns.cnki.net/KNS8/Navi?DBCode=CJFD&BaseID=JSJA) | 2021-04-22 09:03 | 期刊 | [3](https://kns.cnki.net/KNS8/Detail?sdb=CJFD&sfield=cite&skey=基于深度学习的语音合成与转换技术综述&scode=JSJA202108029) | [1065](javascript:void(0);) | [**下载**](https://kns.cnki.net/KNS8/download?filename=5MFNxhmWyg1VtlUMlNTeiJ3R5IWMjFnZlZDV0tWYmtSY1Q1Rjdne5VncycEZ3FTbFZnNn9kTSNGbS5GWnBTSuF1KkBnSa5mRPFHcvImeMpHS4gXb2dFc1dHOmFVQ1UVSqBzbXBjU5VnViN0Q5gXOIZVR5hXUNdGN&tablename=CJFDLAST2021) |
| 20   | [克隆声音，天使还是魔鬼？](https://kns.cnki.net/KNS8/Detail?sfield=fn&QueryID=20&CurRec=20&recid=&FileName=CBXB202104190072&DbName=CCNDLAST2021&DbCode=CCND&yx=&pr=&URLID=) | 苏洁                                                         | [中国银行保险报](https://kns.cnki.net/KNS8/Navi?DBCode=CCND&BaseID=CBXB) | 2021-04-19       |      |                                                              |                             |                                                              |

[基于Tacotron的中文语音合成研究及系统实现](http://202.120.82.200:8800/docinfo.action?id1=1399b9624df1b2bbab8fe82d39766a3f&id2=D%2BroP1y565c%3D)程杰硕士2021-05-072[基于深度学习的中文语音合成](http://202.120.82.200:8800/docinfo.action?id1=2edc26d4ca403b93b13b37b25012b732&id2=0LozebveC4g%3D)陈梦楠硕士2020-05-233[基于ARM便携式盲人浏览器的设计与实现](http://202.120.82.200:8800/docinfo.action?id1=c8c16ae7fb4b6df211520d0757afc296&id2=2ARkn5CaAHA%3D)苏兴龙硕士2012-03-01

### 调研高德TTS&讯飞TTS

说到TTS，除了科大讯飞和百度语音，我首先想到的是高德导航。

<video src="/Users/caining/Downloads/1648452170775272.mp4"></video>

那么我们看看[高德官方Demo](https://lbs.amap.com/api/android-navi-sdk/guide/voice/speech-synthesizer)关于语音的介绍

- 导航SDK从v5.6.0开始在AMapNavi提供了内置语音功能
- 关键API

```java
/**
    * 设置使用内部语音播报
    *
    * @param isUseInnerVoice 是否使用内部语音播报， 默认为false
    * @param isCallBackText  isUseInnerVoice设置为true以后，{@link AMapNaviListener#onGetNavigationText}接口是否继续返回文字，默认为false
    */
    @Override
    public void setUseInnerVoice(boolean isUseInnerVoice, boolean isCallBackText)
      /**  
    * 开始内置语音播报，只有在使用内置语音的情况下才有效
    * @since 6.4.0
    */
    @Override
    public void startSpeak()
    
   /**
    * 停止内置语音播报，只有在使用内置语音的情况下有效
    * 注意：7.1.0版本以后，调用该接口，会停止播放导航语音，但是仍然可以播放自定义语音
    * @since 6.4.0
    */
    @Override
    public void stopSpeak()
      /**
      SDK 会通过AMapNaviListener类回调将播报的文字内容透传出来，如果您不想使用SDK的内置语音进行播报，也可以选择第三方的语音合成SDK（如：阿里云语音合成）将内容转化成声音信息，完成导航播报。以下是涉及到相关接口和回调：
    * 导航播报信息回调函数。
    * @param text 播报文字。
    * @since 5.3.0
    */
    void onGetNavigationText(String text)
      /**
      如果您使用自己合成的方式来进行播报，需要在每句播报前和播报完成后设置一下AMapNavi中如下接口，保证不出现语音打断和延迟问题。
    * 获取当前SDK播报状态
    * 注意：正在播报的时候请不要播报语音，会导致与导航的播报冲突。
    * @since 6.4.0
    * @return 如果正在播报语音，返回值为true，如果没有在播报语音，返回值为false
    */
    public static boolean isTtsPlaying()
       /**
     * 播放自定义文字，注意如果当前正在播放导航语音，可能导致播放失败，只在内置语音下生效
     * 如果您使用了导航sdk的内置语音，我们还提供了自定义语音的播报功能，可以根据您的需要，传入自定义的播报文案进行语音合成与播报。
     * @param tts       要播放的文字
     * @param forcePlay 是否强制进行播报
     *                  true  如果当前有其他导航语音，会等导航语音播放完毕后播放该文字，但是可能导致导航丢失掉关键引导信息
     *                  false 如果当前有其他导航语音，则不播报
     * @return 播放是否成功 true 成功，false 失败
     * @since 6.7.0
     */
    
    @Override
    public boolean playTTS(String tts, boolean forcePlay)
```

#### 高德引入了科大讯飞

<img src="/Users/caining/Library/Application Support/typora-user-images/image-20220328153517917.png" alt="image-20220328153517917" style="zoom:50%;" />

#### [申请科大讯飞](https://console.xfyun.cn/services/xtts)

<img src="/Users/caining/Library/Application Support/typora-user-images/image-20220328153653919.png" alt="image-20220328153653919" style="zoom:50%;" />

免费语音包包括：小燕 与 小锋

- 具体效果大家自己体验 - 僵硬，机器音严重

- 非免费语音收费标准（劝退）

  <img src="/Users/caining/Library/Application Support/typora-user-images/image-20220328153911817.png" alt="image-20220328153911817" style="zoom:50%;" />

### Android内置TextToSpeech

- android.speech.tts.TextToSpeech

- 最简单的 Android 移动端 TTS方案-Google 官方内置 TextToSpeech
- 类似IOS-Siri

#### Android内置TTS使用

TextToSpeech实例只有在完成初始化后才能用于合成文本。 实现TextToSpeech.OnInitListener，以通知初始化的完成。
使用完TextToSpeech实例后，调用shutdown()方法以释放TextToSpeech引擎使用的本机资源。 使用文本到语音的针对Android 11的应用程序应在其清单的查询元素中声明TextToSpeech.Engine.INTENT_ACTION_TTS_SERVICE：

```xml
 <queries>
   ...
  <intent>
      <action android:name="android.intent.action.TTS_SERVICE" />
  </intent>
 </queries>
 
```

| Nested classes |                                                              |
| :------------- | ------------------------------------------------------------ |
| `class`        | `TextToSpeech.Engine`  TTS 引擎.                             |
| `class`        | `TextToSpeech.EngineInfo`     TTS 引擎 信息 Information about an installed text-to-speech engine. |
| `interface`    | `TextToSpeech.OnInitListener`引擎初始化回调.                 |
| `interface`    | `TextToSpeech.OnUtteranceCompletedListener`回调合成进度状态等，在API级别18中不建议使用此接口。 改用“UtteranceProgressListener”。 |

| Constants |                                                              |
| :-------- | ------------------------------------------------------------ |
| `String`  | ACTION_TTS_QUEUE_PROCESSING_COMPLETED 队列中语音合成状态已完成. |
| `int`     | `ERROR`失败.                                                 |
| `int`     | `ERROR_INVALID_REQUEST`表示由无效请求导致的失败.             |
| `int`     | `ERROR_NETWORK`表示网络连接问题导致的故障.                   |
| `int`     | `ERROR_NETWORK_TIMEOUT`表示网络超时导致的故障.               |
| `int`     | `ERROR_NOT_INSTALLED_YET`表示因未完成语音数据下载而导致的故障. |
| `int`     | `ERROR_OUTPUT`表示与输出相关的故障（音频设备或文件）.        |
| `int`     | `ERROR_SERVICE`表示TTS服务的失败.                            |
| `int`     | `ERROR_SYNTHESIS`表示TTS引擎未能合成给定的输入.              |
| `int`     | `LANG_AVAILABLE`表示区域设置为该语言提供的语言，但不表示国家/地区和变体. |
| `int`     | `LANG_COUNTRY_AVAILABLE`表示该语言适用于区域设置指定的语言和国家/地区，但不适用于变体. |
| `int`     | `LANG_COUNTRY_VAR_AVAILABLE`表示语言完全按照区域设置指定的可用. |
| `int`     | `LANG_MISSING_DATA`表示语言数据缺失.                         |
| `int`     | `LANG_NOT_SUPPORTED`表示不支持该语言.                        |
| `int`     | `QUEUE_ADD`队列模式，在播放队列末尾添加新条目.               |
| `int`     | `QUEUE_FLUSH`队列模式，播放队列中的所有条目（要播放的媒体和要合成的文本）都被删除并替换为新条目. |
| `int`     | `STOPPED`表示客户端请求的停止.                               |
| `int`     | `SUCCESS`表示操作成功.                                       |

| Public constructors                                          |
| :----------------------------------------------------------- |
| `TextToSpeech(Context context, TextToSpeech.OnInitListener listener)`TextToSpeech类的构造函数，使用默认的TTS引擎。 |
| `TextToSpeech(Context context, TextToSpeech.OnInitListener listener, String engine)`TextToSpeech类的构造函数，使用给定的TTS引擎。 |

| Public methods                  |                                                              |
| :------------------------------ | ------------------------------------------------------------ |
| `int`                           | `addEarcon(String earcon, String packagename, int resourceId)`在软件包中添加文本字符串和声音资源之间的映射。 |
| `int`                           | `addEarcon(String earcon, String filename)`*API级别21不建议使用此方法。 从API级别21起，替换为“addEarcon（java.lang.String，java.io.File）`.* |
| `int`                           | `addEarcon(String earcon, File file)`在文本字符串和声音文件之间添加映射。 |
| `int`                           | `addEarcon(String earcon, Uri uri)`在文本字符串和声音文件之间添加映射。 |
| `int`                           | `addSpeech(CharSequence text, File file)`添加CharSequence（可能与TtsSpans跨度）和声音文件之间的映射. |
| `int`                           | `addSpeech(String text, String packagename, int resourceId)`在软件包中添加文本字符串和声音资源之间的映射. |
| `int`                           | `addSpeech(CharSequence text, String packagename, int resourceId)`添加文本的CharSequence（可能用TtsSpans跨度）和软件包中的声音资源之间的映射。 |
| `int`                           | `addSpeech(String text, String filename)`在文本字符串和声音文件之间添加映射。 |
| `int`                           | `addSpeech(CharSequence text, Uri uri)`添加CharSequence（可能与TtsSpans跨度）和声音文件之间的映射。 |
| `boolean`                       | `areDefaultsEnforced()`检查用户的设置是否应覆盖调用应用程序请求的设置。 |
| `Set<Locale>`                   | `getAvailableLanguages()`查询引擎中关于可用语言集的信息。    |
| `String`                        | `getDefaultEngine()`获取默认语音合成引擎的包名。             |
| `Locale`                        | `getDefaultLanguage()`*API级别21不建议使用此方法。 从API级别21，使用`getDefaultVoice().getLocale()`(`getDefaultVoice()`)* |
| `Voice`                         | `getDefaultVoice()`返回一个语音实例，这是默认文本到语音语言的默认语音。 |
| `List<TextToSpeech.EngineInfo>` | `getEngines()`获取所有已安装TTS引擎的列表.                   |
| `Set<String>`                   | `getFeatures(Locale locale)`*API级别21不建议使用此方法。 从API级别21起，请使用语音。 要查询语音功能，请调用“getVoices()”以检索可用语音列表，并调用“Voice#getFeatures()”以检索功能集.* |
| `Locale`                        | `getLanguage()`*API级别21不建议使用此方法。 从API级别21，请使用“getVoice().getLocale()`(`getVoice()`)”。* |
| `static int`                    | `getMaxSpeechInputLength()`通过说话和合成ToFile的输入字符串的长度限制。 |
| `Voice`                         | `getVoice()`返回一个语音实例，描述当前用于发送到TextToSpeech引擎的合成请求的声音。 |
| `Set<Voice>`                    | `getVoices()`查询引擎关于可用语音集的信息。                  |
| `int`                           | `isLanguageAvailable(Locale loc)`检查区域设置表示的指定语言是否可用和支持。 |
| `boolean`                       | `isSpeaking()`检查TTS引擎是否忙于说话。                      |
| `int`                           | `playEarcon(String earcon, int queueMode, HashMap<String, String> params)`*API级别21不建议使用此方法。 从API级别21起，替换为“playEarcon(java.lang.String, int, android.os.Bundle, java.lang.String)”。* |
| `int`                           | `playEarcon(String earcon, int queueMode, Bundle params, String utteranceId)`使用指定的排队模式和参数播放耳机。 |
| `int`                           | `playSilence(long durationInMs, int queueMode, HashMap<String, String> params)`*API级别21不建议使用此方法。 从API级别21起，替换为“playSilentUtterance（long，int，java.lang.String）”。* |
| `int`                           | `playSilentUtterance(long durationInMs, int queueMode, String utteranceId)`使用指定的队列模式在指定时间内播放静音。 |
| `int`                           | `setAudioAttributes(AudioAttributes audioAttributes)`设置在朗读文本或播放文件时使用的音频属性。 |
| `int`                           | `setEngineByPackageName(String enginePackageName)`*在API级别15中不建议使用此方法。 当TTS引擎初始化时，这不会通知来电者。 `TextToSpeech(android.content.Context, android.speech.tts.TextToSpeech.OnInitListener, java.lang.String)'可以与适当的引擎名称一起使用。 此外，不能保证指定的发动机会加载。 如果没有安装或禁用，将适用用户/系统范围的默认值。* |
| `int`                           | `setLanguage(Locale loc)`设置文本到语音语言。                |
| `int`                           | `setOnUtteranceCompletedListener(TextToSpeech.OnUtteranceCompletedListener listener)`*在API级别15中不建议使用此方法。 改用“setOnUtteranceProgressListener(android.speech.tts.UtteranceProgressListener)”。 * |
| `int`                           | `setOnUtteranceProgressListener(UtteranceProgressListener listener)`设置将收到与合成给定话语相关的各种事件的通知的监听器。 |
| `int`                           | `setPitch(float pitch)`设置TextToSpeech引擎的语音音高。      |
| `int`                           | `setSpeechRate(float speechRate)`设置语音速率。              |
| `int`                           | `setVoice(Voice voice)`设置文本到语音的声音。                |
| `void`                          | `shutdown()`发布TextToSpeech引擎使用的资源。                 |
| `int`                           | `speak(CharSequence text, int queueMode, Bundle params, String utteranceId)`使用指定的排队策略和语音参数朗读文本，文本可以与TtsSpans跨。 |
| `int`                           | `speak(String text, int queueMode, HashMap<String, String> params)`*API级别21不建议使用此方法。 从API级别21开始，替换为“speak(java.lang.CharSequence, int, android.os.Bundle, java.lang.String)”。* |
| `int`                           | `stop()`中断当前话语（无论是播放还是渲染为文件），并丢弃队列中的其他话语。 |
| `int`                           | `synthesizeToFile(CharSequence text, Bundle params, ParcelFileDescriptor fileDescriptor, String utteranceId)`使用指定的参数将给定的文本合成到ParcelFileDescriptionor。 |
| `int`                           | `synthesizeToFile(CharSequence text, Bundle params, File file, String utteranceId)`使用指定的参数将给定文本合成到文件中。 |
| `int`                           | `synthesizeToFile(String text, HashMap<String, String> params, String filename)`*API级别21不建议使用此方法。 从API级别21开始，替换为“synthesizeToFile（java.lang.CharSequence，android.os.Bundle，java.io.File，java.lang.String）`.* |

关于Android内置 语音引擎

```java
public String getDefaultEngine() {
        String engine = getString(mContext.getContentResolver(),
                Settings.Secure.TTS_DEFAULT_SYNTH);
        return isEngineInstalled(engine) ? engine : getHighestRankedEngineName();
    }

 /**
         * Default text-to-speech engine.
         */
        public static final String TTS_DEFAULT_SYNTH = "tts_default_synth";
```

```java
 /**
     * Gets a list of all installed TTS engines.
    获取所有已安装TTS引擎的列表。
     *返回引擎信息对象列表。 列表可以是空的，但永远不会{@code null}。
     * @return A list of engine info objects. The list can be empty, but never {@code null}.
     */
public List<EngineInfo> getEngines() {
        PackageManager pm = mContext.getPackageManager();
        Intent intent = new Intent(Engine.INTENT_ACTION_TTS_SERVICE);
        List<ResolveInfo> resolveInfos =
                pm.queryIntentServices(intent, PackageManager.MATCH_DEFAULT_ONLY);
        if (resolveInfos == null) return Collections.emptyList();

        List<EngineInfo> engines = new ArrayList<EngineInfo>(resolveInfos.size());

        for (ResolveInfo resolveInfo : resolveInfos) {
            EngineInfo engine = getEngineInfo(resolveInfo, pm);
            if (engine != null) {
                engines.add(engine);
            }
        }
        Collections.sort(engines, EngineInfoComparator.INSTANCE);

        return engines;
    }
```

<img src="/Users/caining/Library/Application Support/typora-user-images/image-20220325170351452.png" alt="image-20220325170351452" style="zoom:50%;" />

#### 内置TTS-Engine从哪来的？

- queryIntentServices

```java
public abstract List<ResolveInfo> queryIntentServices (Intent intent, 
                int flags)
                
                
         /**
         * Intent for starting a TTS service. Services that handle this intent must
         * extend {@link TextToSpeechService}. Normal applications should not use this intent
         * directly, instead they should talk to the TTS service using the the methods in this
         * class.
         */
        @SdkConstant(SdkConstantType.SERVICE_ACTION)
        public static final String INTENT_ACTION_TTS_SERVICE =
                "android.intent.action.TTS_SERVICE";
```

#### 主流开放TTS-Engine引擎

- 引擎下载地址：链接: https://pan.baidu.com/s/1lCa_qzJ_mgm4ETJwTuecQA 提取码: k5s6 
- `com.svox.pico` 系统自带不支持中文语音
- `com.svox.classic` 搜svox搜的，和上面类似不支持中文
- `com.google.android.tts` **谷歌文字转语音引擎**，不支持5.0以下系统，大小17.98M
- `com.iflytek.speechcloud` **科大讯飞语音引擎3.0**，支持4.0以上系统，大小27.27M
  - `com.iflytek.speechsuite` **新版科大讯飞语音引擎**，2018年开始新版手机一般会内置，如oppo、vivo、华为
- `com.baidu.duersdk.opensdk` **度秘语音引擎3.0** 不支持5.0以下系统，大小11.95M
- `com.iflytek.tts` **科大讯飞语音合成**，较老，不支持7.0以上系统，大小9M
- 其他引擎 参考http://hyperionics.com/TtsSetup/eng/TtsInfo.html

<img src="/Users/caining/Library/Application Support/typora-user-images/image-20220325172711508.png" alt="image-20220325172711508" style="zoom:50%;" />

```bash
$adb install /Users/caining/Downloads/语音引擎/度秘语音引擎3.0-BaiduSpeechService.apk
Performing Streamed Install
Success
```

![image-20220325173639964](/Users/caining/Library/Application Support/typora-user-images/image-20220325173639964.png)

- 经过测试，各厂语音各有千秋，无法选出哪个音色更（不）好！！！
- 疑问，是否可自定义引擎？？

#### 自定义TTS 引擎

- ```java
  1. public class MyTTSEngine extends TextToSpeechService
    
  2.<service
              android:name=".MyTTSEngine"
              android:exported="true"
              android:label="@string/app_name"
              tools:ignore="ExportedService">
              <intent-filter>
                  <action android:name="android.intent.action.TTS_SERVICE" />
                  <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
              <meta-data
                  android:name="android.speech.tts"
                  android:resource="@xml/tts_engine" />
          </service>
                    
                    
   3.重写方法
                    
    TTS 引擎实现的抽象基类
  android.speech.tts.TextToSpeechService
  需实现
  onIsLanguageAvailable(String, String, String)//	检查引擎是否支持给定的语言
  onLoadLanguage(String, String, String)//
  onGetLanguage()//返回 TTS 引擎当前使用的语言、国家
  onSynthesizeText(SynthesisRequest, SynthesisCallback)
  onStop()
  前三个主要处理语言管理，并用于查询引擎是否支持给定语言，并向它指示给定语言的请求即将到来。 onSynthesizeText(SynthesisRequest, SynthesisCallback)是引擎实现的核心
  
  public 方法还有：
  onIsValidVoiceName(String voiceName)
  检查引擎是否支持具有给定名称的语音。
  onLoadVoice(String voiceName)
  通知引擎它应该加载语音合成语音。
  
  5.  @Override
      protected void onSynthesizeText(SynthesisRequest request, SynthesisCallback callback) {
          if (request == null || callback == null) return;
          String input = request.getCharSequenceText().toString();
        
          callback.start(AUDIO_SAMPLE_RATE, AudioFormat.ENCODING_PCM_16BIT, 1);//1.开始
          
          callback.audioAvailable(bytes, 0, bytesToWrite);//返回流 - pcm流
         
          callback.done();//结束
      }
  6.load16BitPCMRawDataFileAsDoubleArray
  //http://www.java2s.com/Code/Android/Date-Type/load16BitPCMRawDataFileAsDoubleArray.htm
  
  ```

- 什么是 PCM 格式  

  声音从模拟信号转化为数字信号的技术，经过采样、量化、编码三个过程将模拟信号数字化。

  - 采样

    顾名思义，对模拟信号采集样本，该过程是从时间上对信号进行数字化，例如每秒采集 44100 次，即采样频率 44.1 khz

    <img src="/Users/caining/Library/Application Support/typora-user-images/image-20220331175735897.png" alt="image-20220331175735897" style="zoom:50%;" />

  - 量化

    既然是将音频数字化，那就需要使用二进制来表示声音的每一个样本。例如每个样本使用 16 位长度来表示，即音频的位深度为 16 位

  - 编码

    编码就是按照一定的格式记录采样和量化后的数据，比如顺序存储或压缩存储等

  编码后经由不同的算法，音频被保存为不同的格式，例如 MP3、AAC 等，而 PCM 就是最为原始的一种格式，PCM 数据是音频的裸数据格式，不经过任何压缩。

  

  每个整数占据2个字节(16-bit)，9个采样也就是18字节的数据。每个采样的整数大小最小为 -32768，最大为 32768 。根据采样数据的位置和值画一个图的话，就会得到像播放器上那样的波浪形图。

  

- 其他实现方式---插庄反射-    利用其他引擎，这里不搞了（只是想法）







#### Android内置TTS优劣势

- 优点
  - 免费！！！
  - 使用简便，各厂商出厂内置科大讯飞引擎
  - 可动态扩展引擎，主流引擎有，Google、百度、科大讯飞等
- 缺点
  - 内置引擎依赖第三方服务
  - 内置引擎Voices 暂时不可扩充
  - 内置引擎效果不好

所以，这个解决方案不是我们想要的，我们想要的是完美的音色解决方案，但是内置TTS并不是完全没有价值，我们是否可以通过技术手段，对TTS合成声音进行处理，从而时机器声音得到变化，成为希望听到的声音呢？

### [Fmod](http://www.fmod.org)

#### Fmod简介

FMOD 是一个易于使用的跨平台音频引擎，可在 Windows、Windows CE、Linux、Macintosh、GameCube、Playstation 2 和 XBox 平台上使用。它可以与 C/C++、Visual Basic、Delphi 和 MASM 一起使用。因此，如果您在其中一个平台上使用其中一种语言，并且想在应用程序中使用声音，那么 FMOD 就是为您准备的。下面的示例将使用 C 语言，尽管所有平台的基本原则应该相同。

#### Fmod费用和资源

FMOD 的开发者有一个很好的理念；如果您不打算通过您的项目赚钱，您可以免费使用它。因此，只要您没有通过您的程序获得任何利润，您就不必支付任何费用。但是，如果事实证明您将出售您的产品，则您必须购买许可证，共享软件产品的起价为 100 美元。有关更多详细信息，请查看 fmod.org 上的官方许可价格表。

- 下载地址：[http://www.fmod.org](http://www.fmod.org/)
- Fmod 用法和原理这里不讨论，直接看效果。

#### Fmod变声总结

- Fmod可以改变生成声音的
- 虽然通过Fmod 对内置TTS语音处理之后，略微有所改变，但由于原始声音的不圆润（不像人），处理之后的声音还是不甚理想！
- 暂时放弃

### 以上总结

- 高德、讯飞、百度、google 等语音对中文的支持不错，但默认语音包比较生硬
- 通过Fmod 等对语音进行处理，得到的效果并不理想
- 我们使用内置TTS引擎，联合使用Fmod 对语音进行处理，并没达到我们预想结果
- 所以失败了！！阿西吧！
- 虽然失败了，但并不是没有意义，我们学会了内置TTS 用法，尝试了自定义引擎。











是否下期？？

### 理想型-Natural TTS之深度神经网络模型技术

先看效果[效果展示Natural TTS Synthesis by Conditioning WaveNet on Mel Spectrogram Predictions](https://google.github.io/tacotron/publications/tacotron2/)

- 深度神经网络模型技术 Tacontron+WaveNet

- 训练模型

- 深度学习

- WaveNet 是时域波形的生成式模型，它生成的语音的保真度开始可以与真人语音媲美，该模型已经应用到一些完整的语音合成系统中。然而WaveNet的输入数据（语言学特征，预测的对数基频（F0），以及音素时长）却需要大量的领域专门知识才能生成，需要一个详尽的文本分析系统，还要一个健壮的语音字典（发音指南）。

  Tacotron 是一个从字符序列生成幅度谱图的seq2seq架构 [13]，它仅用输入数据训练出一个单一的神经网络，用于替换语言学和声学特征的生成模块，从而简化了传统语音合成的流水线。为了最终合成出幅度谱图，Tacotron使用Griffin-Lim [14] 算法估计相位，然后施加一个短时傅里叶逆变换。作者们指出，相比WaveNet中使用的方法，Griffin-Lim算法会产生特有的人工痕迹并且合成的语音保真度较低，所以这只是一个临时方法，将来要替换成神经声码器。

### WaveNet 和 Tacotron2 是什么？

WaveNet 和 Tacotron 是神经网络模型，它们解决了上述管道的*一个步骤。*具体来说，WaveNet 是一种*神经声码器*，负责流水线的“波形合成”步骤。Tacotron 是用于频谱图合成的序列到序列模型，并解决了“高级音频合成”步骤。

用于语音合成的深度学习模型，例如 Google 的 WaveNet 和 Tacotron，并不是完整的TTS系统。它们每一个都只是大量模型和启发式方法的一部分，这些模型和启发式方法共同构成了一个文本到语音的引擎。WaveNet 不是文本转语音引擎。Tacotron 也不是。

在过去的几年里，研究人员设计了许多用于合成音频的神经网络架构。最常被引用的是 Google 的 WaveNet 和 Tacotron，但还有很多很多其他的，例如 Google 的 WaveRNN、百度的 Deep Voice 论文、NVidia 的 WaveGlow、SampleRNN、微软的 FastSpeech 等等。

这些论文在公司网站和科技新闻网站上大张旗鼓地宣布，并附有精彩的音频样本和有趣的演示。这给人的印象是该论文描述了一个*完整*的文本到语音系统。Google 甚至将其在 Google Cloud 中的神经声码器语音标记为 WaveNet 语音，这可能会掩盖 TTS 引擎管道的重要部分在两个引擎之间共享的事实。结果，很多在线人最终（可以理解！）感到困惑并提到“WaveNet TTS 系统”或“Tacotron TTS 系统”，或者假设可以使用重新实现其中一个的 Github 存储库构建完整的语音合成引擎。

现在我们有了这些区别，我们可以更具体地问：这个管道的每个阶段都开发了哪些模型？

- **规范化：**规范化与机器学习方法有关，但人们正在努力。例如，[这个](https://arxiv.org/pdf/1712.06994.pdf)和[这个](https://arxiv.org/pdf/1611.00068.pdf)。
- **音素转换：**相对于这个列表中的其他问题，这个问题没有那么难，所以没有那么多的研究专门研究它。您可以找到一些可靠的论文，例如[this](https://arxiv.org/abs/1708.01464)和[this](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43264.pdf)。
- **音频合成**：除了[Tacotron](https://arxiv.org/abs/1703.10135)（和[Tacotron ](https://arxiv.org/abs/1809.08895)[2](https://ai.googleblog.com/2017/12/tacotron-2-generating-human-like-speech.html)），还有[Deep Voice](https://arxiv.org/abs/1710.07654)、[FastSpeech](https://arxiv.org/abs/1905.09263)、[ParaNet](http://research.baidu.com/Blog/index-view?id=116)等。
- **波形合成**：除了[WaveNet 之外](https://arxiv.org/abs/1609.03499)，还有[WaveGlow](https://arxiv.org/abs/1811.00002)、[WaveRNN](https://arxiv.org/abs/1802.08435)、[ClariNet](https://arxiv.org/abs/1807.07281)、[WaveFlow](https://arxiv.org/abs/1912.01219)、[SampleRNN](https://arxiv.org/abs/1612.07837)等。

- **相关论文： [tacotron2.pdf](../Downloads/Tacotron-2-Chinese-mandarin-biaobei/papers/tacotron2.pdf)  [wavenet.pdf](../Downloads/Tacotron-2-Chinese-mandarin-biaobei/papers/wavenet.pdf)** 

移动端如何使用Tacotron2？

### TensorFlow&TensorFlowTTS

<img src="https://camo.githubusercontent.com/aeb4f612bd9b40d81c62fcbebd6db44a5d4344b8b962be0138817e18c9c06963/68747470733a2f2f7777772e74656e736f72666c6f772e6f72672f696d616765732f74665f6c6f676f5f686f72697a6f6e74616c2e706e67" alt="image-20220328153517917" style="zoom:50%;" />

TensorFlow 是用于机器学习的端到端开源平台。它拥有一个由工具、库和社区资源组成的全面、灵活的生态系统，让研究人员能够推动 ML 的最新技术，开发人员可以轻松构建和部署基于 ML 的应用程序。 TensorFlow 最初是由 Google 机器智能研究组织内的 Google Brain 团队的研究人员和工程师开发的，用于进行机器学习和深度神经网络研究。该系统具有足够的通用性，也适用于各种其他领域

历史小课堂：Google Brain：是一个[深度学习](https://zh.wikipedia.org/wiki/深度学习)与[人工智能](https://zh.wikipedia.org/wiki/人工智慧)科研项目团队，其母公司为[谷歌](https://zh.wikipedia.org/wiki/谷歌)。2014年1月26日，多家媒体机构报道谷歌以未公布的价格并购了英国[DeepMind](https://zh.wikipedia.org/wiki/DeepMind)科技公司。**AlphaGo**（“Go”为日文“碁”字发音转写，是[围棋](https://zh.wikipedia.org/wiki/围棋)的西方名称），直译为**阿尔法围棋**，亦被音译为**阿尔法狗**[[1\]](https://zh.wikipedia.org/wiki/AlphaGo#cite_note-1)[[2\]](https://zh.wikipedia.org/wiki/AlphaGo#cite_note-2)、**阿法狗**[[3\]](https://zh.wikipedia.org/wiki/AlphaGo#cite_note-3)、**阿发狗**[[4\]](https://zh.wikipedia.org/wiki/AlphaGo#cite_note-4)[[5\]](https://zh.wikipedia.org/wiki/AlphaGo#cite_note-5)等，是于2014年开始由[英国](https://zh.wikipedia.org/wiki/英国)伦敦[Google DeepMind](https://zh.wikipedia.org/wiki/Google_DeepMind)开发的[人工智能](https://zh.wikipedia.org/wiki/人工智能)[围棋软件](https://zh.wikipedia.org/wiki/圍棋軟體)。2017年，关于AlphaGo的电影纪录片《[AlphaGo世纪对决](https://zh.wikipedia.org/wiki/AlphaGo世紀對決)》正式上映。[AlphaGo vs 柯洁](https://www.youtube.com/watch?v=uPBzDk-OmoU)

[TensorFlowTTS](https://github.com/TensorSpeech/TensorFlowTTS)：简介Real-Time State-of-the-art Speech Synthesis for Tensorflow 2,TensorFlowTTS 基于 TensorFlow 2 提供实时最先进的语音合成架构，例如 Tacotron-2、Melgan、Multiband-Melgan、FastSpeech、FastSpeech2。借助 Tensorflow 2，我们可以加快训练/推理进度，优化器进一步通过使用[fake-quantize 感知](https://www.tensorflow.org/model_optimization/guide/quantization/training_comprehensive_guide)和[剪枝](https://www.tensorflow.org/model_optimization/guide/pruning/pruning_with_keras)，使 TTS 模型可以比实时更快地运行，并能够部署在移动设备或嵌入式系统上

#### 特征

- 语音合成的高性能。
- 能够对其他语言进行微调。
- 快速、可扩展且可靠。
- 适合部署。
- 易于实现基于抽象类的新模型。
- 如果可能的话，混合精度以加速训练。
- 支持单/多GPU梯度累积。
- 在基础培训师课程中支持单/多 GPU。
- 所有支持的模型的 TFlite 转换。
- 安卓示例。
- 支持多国语言（目前支持中文、韩文、英文、法文、德文）
- 支持 C++ 推理。
- 支持将部分模型的权重从 PyTorch 转换为 TensorFlow 以加快速度。





本篇到此结束，还未学习到的内容有，如何训练语音（TFlite模型怎么来？？），语音相关知识的深度学习理解，如果有可能，我们下期再分享。





余雪亭

https://www.aispeech.com/core/tts





https://blog.csdn.net/weixin_41763134/article/details/90900539

THCS-30

**标识符：** SLR18

**摘要：** CSLT@清华大学发布的免费中文语音语料库

**类别：**演讲

**许可证：** Apache 许可证 v.2.0

**下载（使用离您更近的镜子）：**
[data_thchs30.tgz](https://www.openslr.org/resources/18/data_thchs30.tgz) [6.4G]（语音数据和成绩单） 镜像： [[US\] ](https://us.openslr.org/resources/18/data_thchs30.tgz) [[EU\] ](https://openslr.elda.org/resources/18/data_thchs30.tgz) [[CN\] ](https://openslr.magicdatatech.com/resources/18/data_thchs30.tgz) 
[test-noise.tgz](https://www.openslr.org/resources/18/test-noise.tgz) [1.9G]（标准 0db 噪声测试数据）镜像： [[US\] ](https://us.openslr.org/resources/18/test-noise.tgz) [[EU\] ](https://openslr.elda.org/resources/18/test-noise.tgz) [[CN\] ](https://openslr.magicdatatech.com/resources/18/test-noise.tgz) 
[resource.tgz](https://www.openslr.org/resources/18/resource.tgz) [24M]（补充资源，包括训练数据词典、噪声样本） 镜像： [[US\] ](https://us.openslr.org/resources/18/resource.tgz) [[EU\] ](https://openslr.elda.org/resources/18/resource.tgz) [[CN\]](https://openslr.magicdatatech.com/resources/18/resource.tgz) 



**关于此资源：**

THCHS30 是由清华大学语音与语言技术中心（CSLT）发布的开放式汉语语音数据库。原录音是2002年在清华大学计算机系智能与系统国家重点实验室，由王东在朱晓燕教授的指导下进行的，原名“TCMSD”，代表“清华连续”普通话语音数据库”。时隔13年出版，由王东博士发起，朱小燕教授支持。我们希望为语音识别领域的新研究人员提供一个玩具数据库。因此，该数据库对学术用户完全免费。您可以使用以下 BibTeX 条目引用数据：

```
@misc{THCHS30_2015,
  title={THCHS-30 : A Free Chinese Speech Corpus},
  作者={王东，张学伟，张志勇}，
  年={2015}，
  网址={http://arxiv.org/abs/1512.01882 }
}
```

人们

王东，张学伟，张志勇@CSLT，清华大学。

接触器

- 汪东 [wangdong99@mails.tsinghua.edu.cn](mailto:wangdong99@mails.tsinghua.edu.cn)
- 张学伟 [zxw@cslt.riit.tsinghua.edu.cn](mailto:zxw@cslt.riit.tsinghua.edu.cn)
- 张志勇 [zhangzy@cslt.riit.tsinghua.edu.cn](mailto:zhangzy@cslt.riit.tsinghua.edu.cn)

ROOM1-303，大厦 FIT

清华大学 CSLT

[http://cslt.org](http://cslt.org/)

[http://cslt.riit.tsinghua.edu。cn](http://cslt.riit.tsinghua.edu.cn/)

**外部网址：**
[http](http://data.cslt.org/thchs30/README.html) ://data.cslt.org/thchs30/README.html  （原网址来自CSLT）
http://pan.baidu.com/s/1hqKwE00  （百度盘）

效果展示

- https://google.github.io/tacotron/publications/tacotron2/

### 引用

https://my.oschina.net/stephenyng

上中下

实战

高德 https://lbs.amap.com/api/android-navi-sdk/guide/voice/speech-synthesizer

阿里语音 https://help.aliyun.com/product/54853.html

实战二

原理

https://www.gamedev.net/reference/articles/article2098.asp

https://github.com/TensorSpeech/TensorFlowTTS

https://github.com/TensorSpeech/TensorFlowTTS.git

https://blog.csdn.net/ss182172633/article/details/109851660

https://github.com/tulasiram58827/TTS_TFLite

https://github.com/benjaminwan/ChineseTtsTflite

https://blog.csdn.net/qq_28662689/article/details/90177757?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-90177757.pc_agg_new_rank&utm_term=github+tts%E8%AF%AD%E9%9F%B3%E5%90%88%E6%88%90&spm=1000.2123.3001.4430











### TTS - TensorFlowTTS

https://www.tensorflow.org/lite/guide/model_maker



https://lonepatient.top/2021/01/20/awesome-pretrained-chinese-nlp-models

https://huggingface.co/uer

https://github.com/babysor/MockingBird    AI拟声: 5秒内克隆您的声音并生成任意语音内容

https://github.com/babysor/MockingBird/blob/main/README-CN.md

https://zhuanlan.zhihu.com/p/425692267



[aidatatang_200zh.tgz ](https://www.openslr.org/resources/62/aidatatang_200zh.tgz)

https://openslr.org/62/





https://github.com/kuangdd/ttskit







https://www.readspeaker.com/text-to-speech-demo/