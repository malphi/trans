---
author: "Stela Udovicic"
translator: "zhangsean"
reviewer: ["haiker2011"]
original: "https://medium.com/@ustela101/envoy-service-mesh-and-observability-best-practices-for-enterprises-6b72bbaa723c"
title: "Envoy、服务网格和可观察性之企业最佳实践"
summary: "通过对Envoy创始人Matt Klein的采访，Matt分享了他对企业开始使用微服务部署Envoy所遇到的挑战以及可观察性的看法和选择。"
categories: "译文"
tags: ["Microservices","Service Mesh","Istio","Envoy","Observability"]
originalPublishDate: 2019-03-11
publishDate: 2019-04-30
---

[编者按]
> Envoy 创始人 Matt Klein 分享了他对企业开始使用 Envoy 部署微服务所遇到的挑战以及可观察性的看法和选择，他认为 Service Mesh 还处于早期阶段，企业应该逐步推进，同时最好选择商业解决方案。

Service Mesh 的受欢迎程度正在飙升，尽管它还处于初期阶段。在为部署 Envoy 的企业寻求有关服务网格和可观察性最佳实践的建议时，我与 [Envoy](https://www.envoyproxy.io/) 的创建者和[云原生计算基金会](https://www.cncf.io/)（CNCF）技术监督委员会（TOC）的代表 Matt Klein 聊了聊。在下面的讨论中，Matt 分享了他对企业开始使用 Envoy 部署微服务所遇到挑战的看法，同时也谈到了可观察性，以及在与 Envoy 一起选择可观察性平台时可以做出的选择。

**Stela Udovicic：您认为在采用微服务时遇到什么样的痛点表明应该去考虑 Envoy？**

**Matt Klein**：我认为当人们采用微服务时，他们会看到普遍存在的问题，其中很多问题都与网络和可观察性有关。有关网络稳定性的问题，有关重试和断路等相似的问题都是常见的例子。所以，这将通过用他们最终使用的任何语言编写一堆代码来解决，或者他们可能会采用一些外部进程代理。

**Stela Udovicic：对于在生产中部署 Envoy 的人来说，他们将面临哪些挑战，以及学习哪些经验教训可以更顺利地采用 Envoy？**

**Matt Klein**：从 Envoy 的角度来看，我认为这并不容易，它有数据平面，还有控制平面。在我们的组织（Lyft）中，我们将不可避免地最终需要两者，并且 Envoy 没有自己的控制平面。然而，许多项目和系统正在涌现，现在，所有这些都是控制平面，无论是像 Istio，AWS App Mesh 这样的云管理服务，还是来自 Solo.io 的 Gloo 等等。所以我认为人们必须决定的第一件事是他们是否想直接使用 Envoy，或者他们是否想尝试使用更加整体的解决方案。再说一次，无论是Istio，Consul Connector App Mesh还是其他什么类似的方案，我个人建议大多数客户开始尝试其中一个项目。我认为他们应该选择一个比 Envoy 知识更少但是功能更全面的方案，我想说，普通用户更容易接受。

接下来的事情是考虑逐步部署。它不应该是一个大爆炸，所以您要考虑好您想要解决的问题，就从那个问题开始，首先搞定如何部署一组服务，并看看它是如何工作的，然后从那里展开。所以我认为逐步部署是一个关键。显然，逐步部署和回滚，并试图弄清楚 Envoy 是应该单独使用还是应该将它作为更大产品或系统的一部分使用。

**Stela Udovicic：在您的会议中，您提到 Envoy 提供的最有价值的东西之一就是可观察性。了解延迟的分布至关重要，不仅适用于云原生组织，也适用于小型公司和更传统的企业。您能详细说明信任测量数据的重要性以及尾端延迟的负面影响吗？**

**Matt Klein**：嗯，我认为大家对于用户体验或者您尝试提供的 SLA 或 SLO 都有高度关注。虽然知道 P50 可能很有用，但事情通常往往会有一个非常夸张的曲线，而且是1％的用户可能会遇到10倍的延迟 ——— 从产品的角度来看，这可能是非常重要的事情。这就是准确测量 P99 有重要意义的地方。它让您了解边缘的质量标准。

如果您正在努力实现非常高质量和非常可靠的服务，那么您肯定不会希望每一百个请求中的一个会出错。我想大多数人会认为这是一个非常高的错误率，这就是为什么它很重要。

**Stela Udovicic：那么，开发人员如何信任他们的测量数据呢？**

**Matt Klein**：在获得可靠数据方面，有一个理论是观察行为会改变观察结果。此外，我们在计算系统的影响中看到了很多这样的影响，因此很难通过观察而不改变观察结果，但是您要尽可能多地尝试 —— 获得可靠的数据。这就是为什么您关心这些类型的代理和测量系统的性能 —— 因为如果代理本身或它们运行的​​系统有任意收集暂停或有明显的性能缺陷，您就不能信任这些数据做出更高层次的关键决策。

**Stela Udovicic：在处理时间敏感的应用程序时，观察者的影响变得重要。我们是否可以假设 Envoy 已经足够优化，不会给大多数用户的服务带来性能负担？**

**Matt Klein**：我认为我们对 Envoy 最常见的问题是：“它有多快？性能开销怎样？“ 因为，确实，有一些性能开销。从项目的角度来看，我们不会仅仅因为很难做到而公布官方性能数据。它非常依赖于用户的环境以及它们正在运行的配置等。也就是说，我们知道 Envoy 受到了很多性能关注，我们总是试图让它变得更好，所以永远不会有任何完美的情况，但我们确实觉得它在大多数场景下的表现已经足够快了。

**Stela Udovicic：谈到微服务，有很多统计/指标和追踪要收集。可伸缩性有多重要 —— 当人们想要分析 Envoy 数据时？**

**Matt Klein**：我认为伸缩有两个方面。我认为只有一些指标点。当然，还有很多不同的指标。因此，Envoy 输出20个不同的参数，Envoy 大规模地发出如此多的指标，例如如果选择跟踪基于每个主机的所有指标，代价可能变得昂贵。因此，选择测量内容的灵活性变得很重要。我认为在主机总数，数据点总数，是否应该过滤某些指标以及如何扩展指标平台方面需要考虑不同的事情。根据扩展问题，了解对后端可观察性系统的影响非常重要。

**Stela Udovicic：当团队开始采用 Envoy 和微服务时，在选择与 Envoy 一起使用的平台时，考虑基数问题有多重要？**

**Matt Klein**：如果在主机太多的情况下尝试对每个主机进行很多的度量，那么不考虑基数方面可能会成为问题。另外，如果您正在进行服务度量并且您现在拥有数千种服务，并且您正在尝试执行包含可能数千种不同内容的查询，那么您可能会发现这会成为问题。服务量以及您是否尝试执行许多以每个主机为基准的指标，这些是最常见的因素。此外，当您具有高度动态的时序内容（如容器实例ID）时，基数问题肯定会影响可观察性。

**Stela Udovicic：对于那些与 Kubernetes 一起部署 Envoy 的人来说，您对“最佳实践”的建议是什么？**

**Matt Klein**：这是一个非常大的问题。再次，它归结为控制平面。因此，在这些高度动态的环境中，您最终使用的控制平面会最终影响工作的最终结果。例如，如果您正在尝试使用 Envoy 作为负载均衡器并且您想要一个 Kubernetes 入口控制器，您可以看看 Contour，您可以可以看看 Datawire 的 Ambassador —— 有两种不同的解决方案。

在服务网络方面，有 Istio，也有人关注在 Kubernetes 上运行的 Consul Connect，以及其他几个解决方案。我的建议是针对大多数人来说 —— 我们仍处于早期阶段，但对于大多数企业或今天即将到来的人来说，我们可能已经走得足够远了 —— 可能不值得推动他们的控制平面解决方案。这就是为什么我建议查看已经开发的解决方案。

**Stela Udovicic：对于那些开始部署 Envoy 并发展其微服务环境的企业：当选择一个可观察性平台与 Envoy 一起使用时，他们应该选择开源解决方案还是供应商？**

**Matt Klein**：我认为我的建议可能是大多数人与供应商合作。在我看来，尝试在家里做一些可以与供应商提供的东西相媲美的东西或许是不可能的。我想现在一些供应商正在构建更全面的解决方案。一些供应商一直在积极确保 Envoy 运作良好。此外，它们还提供指标，跟踪等。了解开发人员想要的功能类型非常重要。他们想要什么类型的用户控件，什么样的仪表板，跟踪，告警 —— 并且可能从那里挑选供应商。在这方面，我不能建议人们使用自己的系统。

**Stela Udovicic：整合 Kafka 和 Envoy 现在是什么状态？**

**Matt Klein**：我们处于非常早期的阶段。我们现在有一个PR还是开启的。我希望一旦合并，我们将会有进一步的动作。不管怎样，我认为现在它还处于起步阶段。

**Stela Udovicic：您认为整合 Kafka 对 Envoy 的用户有何影响？**

**Matt Klein**：我认为还不太清楚。我认为人们想要用它做很多事情。目前还不完全清楚什么才是银弹。我认为最初可能是围绕统计数据。因此，从可观察性开始，然后可能进入负载均衡或客户端替换或升级或那些类型的事情。不过，我认为我们可能会从可观察性开始。

**Stela Udovicic：您今天使用哪些您开始没有的工具来完成 Lyft 服务网格的工作，Istio 是一个例子吗？**

**Matt Klein**：是的，我的意思是，这就是我认为 Lyft 的情况可能不是，在这一点上，甚至不一定要考虑。因为我们开发了 Envoy，所以我们开发了许多一次性工具。那不是通用系统。我的意思是它与 Lyft 非常相关。所以我认为我们还处于服务网格的早期阶段，产品仍然很难使用。目前还没有一个杀手级的解决方案。

我确实认为很多公司和组织在这个领域都在迅速迭代。就像我们已经谈过的那样，例如 AWS 上的 App Mesh，或 Istio 开源项目，或 Consul Connect，或来自 Solo.io 的 Gloo，或 Cilium。他们有很多，也有创业公司。所以我认为在接下来的几年里会有很多竞争。在那个领域会有一些赢家和一些输家。

不幸的是，对于我向人们推荐的内容，现在没有唯一答案。然而，我建议人们保持开放的心态，看看不同的选择，并且可能不会发挥自己的作用。除非他们的工程组织具有足够的规模和复杂性，他们也许可以实现这一目标。我们确实看到很多组织都在自己成长，但这些组织往往是规模要大得多的工程组织，我想说，与典型的企业不同，这些组织拥有更多的技术人才。因此，我鼓励典型的企业更多地寻找供应商解决方案。

**Stela Udovicic：谢谢您，马特，谢谢花时间分享您的指导和丰富的知识。**