# Hands-On Data Visualization

[toc]

## 前言

在了解 mapshaper.org 和 geojson.io 的过程中看到的一本书，该书探索数据可视化（可能是毕业方向之一），感觉讲的非常好。[书籍地址](https://handsondataviz.org/)

## Introduction: Why Data Visualization

在本书中，我们将通过融合设计原则和一步一步的章节来学习如何创建真实且有意义的数据可视化，以使你的基于分析和论点更具有洞察力和吸引力。正如句子通过来源注释变得更有说服力一样，当与适当的表格、图表或地图配对时，数据驱动会变得更加强大。文字告诉我们故事，但可视化通过将定量、关系或空间模式转化为图像来向我们展示数据故事。当可视化设计得很好时，它们会将我们的注意力吸引到数据中最重要的部分。

如何设计交互式表格、图表和地图，并将它们嵌入到网络中。交互式可视化通过邀请用户与数据交互、探索用户感兴趣的模式、根据需要下载文件并分享结果，来扩大更广泛的受众。数据可视化在互联网上广泛传播，但是快速增长也带来了严重的问题。“信息时代”现在与“虚假信息时代”重叠，几乎人人都可以在网上发布信息，你如何做出明智的决定？当看到关于社会不平等或气候变化等分裂性政策问题的相互矛盾的数据故事时候，你选择相信哪一个？数据可视化照亮了我们追求真理的道路，但它也使我们能够欺骗和撒谎。

### What Can You Believe?

- Point 1：*自 1970 年代以来，美国的经济不平等急剧上升。*
- Point 2：*1970 年，按今天的美元计算，美国前 10% 的成年人平均收入约为 135,000 美元，而后 50% 的人平均收入约为 16,500 美元。根据世界不平等数据库的数据，这种不平等差距在接下来的 50 年里急剧扩大，最高阶层的收入攀升至约 350,000 美元，而底部的一半收入几乎没有上升到约 19,000 美元。*[5](https://handsondataviz.org/believe.html#fn5)
- Point 3：

| 美国收入等级 | 1970年       | 2019年       |
| ------------ | ------------ | ------------ |
| 前 10%       | 136,308 美元 | 352,815 美元 |
| 中间 40%     | 44,353 美元  | 76,462 美元  |
| 底部 50%     | 16,515 美元  | 19,177 美元  |

注：以 2019 年不变美元表示。20 岁及以上个人的国民收入，在税收和转移之前，但包括养老金缴款和分配。资料来源：[世界不平等数据库，2020 年访问](https://wid.world/share/#0/countrytimeseries/aptinc_p50p90_z;aptinc_p90p100_z;aptinc_p0p50_z/US/2015/kk/k/x/yearly/a/false/0/400000/curve/false)

What can you believe?

### Some Pictures Are More Persuasive

- Figure 0.1:

<iframe src="https://datawrapper.dwcdn.net/LtRbj/" width="672" height="400px" style="-webkit-font-smoothing: antialiased; box-sizing: border-box; -webkit-tap-highlight-color: transparent; text-size-adjust: none; border: 0px; color: rgb(51, 51, 51); font-family: &quot;Helvetica Neue&quot;, Helvetica, Arial, sans-serif; font-size: 20px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: 0.2px; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(193, 230, 198); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

Figure 0.1: Explore the [interactive line chart](https://datawrapper.dwcdn.net/LtRbj/) of US adult income inequality over time.

- Figure 0.2:

<iframe src="https://datawrapper.dwcdn.net/JsxEp/" width="672" height="400px" style="-webkit-font-smoothing: antialiased; box-sizing: border-box; -webkit-tap-highlight-color: transparent; text-size-adjust: none; border: 0px; color: rgb(51, 51, 51); font-family: &quot;Helvetica Neue&quot;, Helvetica, Arial, sans-serif; font-size: 20px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: 0.2px; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(193, 230, 198); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

Figure 0.2: Explore the [alternative version of the interactive line chart](https://datawrapper.dwcdn.net/JsxEp/) of US adult income inequality over time, using the same data as the first version.

图 0.1 和图 0.2 包含了相同的数据，为什么它们看起来如此的不同？不平等差距的显著增长发生了什么，现在似乎已经被消除了，危机消除了吗？还是说这是一个骗局？

尽管这是同一份数据，但是图 0.2 y 轴上的金额是用对数刻度构建的，这最适合显示指数增长。在 Covid 病毒大流行期间也是使用的对数刻度，它们适当地说明了病例非常高的增长率，当时传统的线性刻度已经很难显示病例的增长了。可是图2使用对数刻度是具有误导性的，因为并没有充分的理由要使用对数刻度来解释这个收入数据，除非制度者是故意想要隐瞒事实。人们可以用图表来阐明真相，也可以用图表来掩盖真相

### Different Shades of  the Truth

- Point 4: *美国的收入不平等更为严重，目前最富有的 1% 的人口获得了国民收入的 20%。相比之下，在大多数欧洲国家，最富有的 1% 的人获得的份额较小，在国民收入的 6% 到 15% 之间*

- Figure 0.3

<iframe src="https://datawrapper.dwcdn.net/NLMLg/" width="672" height="405px" style="-webkit-font-smoothing: antialiased; box-sizing: border-box; -webkit-tap-highlight-color: transparent; text-size-adjust: none; border: 0px; color: rgb(51, 51, 51); font-family: &quot;Helvetica Neue&quot;, Helvetica, Arial, sans-serif; font-size: 20px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: 0.2px; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(193, 230, 198); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

Figure 0.3: Explore the [interactive map](https://datawrapper.dwcdn.net/NLMLg/) of world income inequality, measured by the share of national income held by the top 1 percent of the population, based on the most recent data available. Source: [World Inequality Database 2020](https://wid.world/world/#sptinc_p99p100_z/US;FR;DE;CN;ZA;GB;WO/last/eu/k/p/yearly/s/false/5.070499999999999/30/curve/false/country).

- Figure 0.4:

<iframe src="https://datawrapper.dwcdn.net/o5f9Q/" width="672" height="405px" style="-webkit-font-smoothing: antialiased; box-sizing: border-box; -webkit-tap-highlight-color: transparent; text-size-adjust: none; border: 0px; color: rgb(51, 51, 51); font-family: &quot;Helvetica Neue&quot;, Helvetica, Arial, sans-serif; font-size: 20px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: 0.2px; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(193, 230, 198); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

Figure 0.4: Explore an [alternative version of the interactive map](https://datawrapper.dwcdn.net/o5f9Q/) of world income inequality, using the same data as the map above.

为什么要用地图而不是表格或者图表了？

虽然我们可以创建观点 4 的表格或图表，但是这并不是集中快速显示 120 多个国家信息的最有效方法。同时这是空间数据，我们可以将其转换为交互式地图，以帮助用户探索全球的收入水平

图 0.3 是不是比观点 4 更有说服力？

虽然地图和文本提供了关于美国和欧洲收入不平等的相同数据，按理说并没有区别。但是交互式地图将用户带入了一个强有力的数据故事，它生动地说明了贫富差距，地图上的颜色预示着危机，美国（以及俄罗斯和巴西）的收入不平等在最高水平上以深红色突出显示——前 1% 的人占据了国民收入的 19% 或更多，相比之下，当我们的目光飘向大西洋的时，几乎所有的欧洲国家都是呈现出较浅的米色或橙色，这表明它们的富人在国民收入的占比较小

图 0.3 和 图 0.4 你更相信哪张地图？

图 0.4 看起来和 图 0.3 是不是有些不同？美国现在不是深红色，而是中蓝色，在光谱上更加接近加拿大和大多数欧洲国家。收入不平等的危机是不是从美国消失了？哪张地图是真的？

两张地图都没有误导，且都已合理的设计对数据做出真实的解释，尽管它们在我们的眼中产生了截然不同的印象。请仔细看地图的图例。第一张地图将国家分为了三类：小于 13%， 13-19%， 19及以上，而第二张地图以绿蓝色渐变显示整个范围。由于美国的份额为 20.5%，因此在第一张图中，它以最深的红色显示，但在第二张图中，它以中蓝色更接近中间的某个位置。然后，这两张地图同样有效，因为既没有违反地图设计中的明确规则，也没有故意延时数据，人们可能因地图而产生误导，但也有可能对真相进行多幅描绘

数据可视化的解释性是一项严峻的挑战。创建真实而有意义的图表和地图，培养良好设计的原则和深思熟虑的思维习惯，并尝试以身作则。数据可视化相比科学来说，有时更像是一门艺术，我们知道图表和地图可以被操纵，就像是文字一样被用来误导你的观众，我们应该知道常见的欺骗技巧，防止被欺骗的同时避免自己的作品中出现欺骗。我们可能会因为同时存在多个看似正确的答案而沮丧，但是我们需要做的只是不断的寻找更好的答案，而不必期望找到一个正确的答案，尤其是随着可视化和工具的不断发展，新的方式会展示新的答案

### Origanization of  the Book

该书分为四个部分：

1. 培养关于设想数据故事的基础技能，以及讲述它所需的工具和数据。1-5 章：选择一个工具来讲述你的数据故事；加强你的电子表格技能；查找和质疑你的数据；清理凌乱的数据；进行有意义的比较
2. 使用易于学习的拖放工具构建大量可视化，并找出哪种类型最适合什么样的数据故事。6-9 章：绘制你的数据；映射你的数据；列出你数据的开始与强调的解释；嵌入Web以交互式可视化
3. 使用更加强大的工具，特别是代码模板，这些工具是你可以更好的自定义可视化的外观以及在线托管它们的位置。10-13 章：使用GitHub编辑和托管代码；Chart.js 和 Highchart 模板；Leaflet 地图模板；转换地图数据中的更高级的空间工具
4. 回归到本书的中心主题：用数据讲述真实而有意义的故事，总结你培养的所有可视化技能。14-15 章：学习如何使用图表和地图撒谎，从而更好地讲真话；最后，讲述和展示你的数据故事，数据可视化的目标不仅仅是制作关于数字的图片，而是制作一个真实的叙述，让读者相信你的解释并重视你的解释

该书的目标是让读者学习融合通过交互式数据可视化讲述真实而有意义的故事，同时注意人们可以利用它们来误导的方式。

## Chapter 1 Choose Tools to Tell Your Story

如果你对当今可用的大量数字工具感到不知所措，那么你并不孤单。但是当你只是尝试做你的日常工作时，那么跟上最新的技术开发可能会让你感觉干了一份没有报酬的兼职。如果你喜欢尝试不同的选择，那么这将是一个好消息

