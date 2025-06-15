<!--Copyright © 适用于[License](https://github.com/chenzomi12/AISystem)版权许可-->

# 基本介绍

分布式训练是一种模型训练模式，它将训练工作量分散到多个工作节点上，从而大大提高了训练速度和模型准确性。虽然分布式训练可用于任何类型的 AI 模型训练，但将其用于大模型和计算要求较高的任务最为有利。

本节将围绕在 PyTorch2.0 中提供的多种分布式训练方式展开，包括并行训练，如：数据并行（Data Parallelism, DP）、模型并行（Model Parallelism, MP）、混合并行（Hybrid Parallel），可扩展的分布式训练组件，如：设备网格（Device Mesh）、RPC 分布式训练以及自定义扩展等。每种方法在特定用例中都有独特的优势。

具体来说，这些功能的实现可以分为三个主要组件：

1. 分布式数据并行训练（DDP）是一种广泛采用的单程序多数据训练范式。在 DDP 中，模型会在每个进程上复制，每个模型副本将接收不同的输入数据样本。DDP 负责梯度通信以保持模型副本同步，并将其与梯度计算重叠以加速训练。

2. 基于 RPC 的分布式训练（RPC）支持无法适应数据并行训练的通用训练结构，例如分布式流水线并行、参数服务器范式以及 DDP 与其他训练范式的组合。它有助于管理远程对象的生命周期，并将自动微分引擎扩展到单个计算节点之外。

3. 提供了在组内进程之间发送张量的功能，包括集体通信 API（如 All Reduce 和 All Gather）和点对点通信 API（如 send 和 receive）。尽管 DDP 和 RPC 已经满足了大多数分布式训练需求，PyTorch 的中间表达 C10d 仍然在需要更细粒度通信控制的场景中发挥作用。例如，分布式参数平均，在这种情况下，应用程序希望在反向传播之后计算所有模型参数的平均值，而不是使用 DDP 来通信梯度。这可以将通信与计算解耦，并允许对通信内容进行更细粒度的控制，但同时也放弃了 DDP 提供的性能优化。

通过充分利用这些分布式训练组件，开发人员可以在各种计算要求和硬件配置下高效地训练大模型，实现更快的训练速度和更高的模型准确性。

## 本节视频

<html>
<iframe src="https://player.bilibili.com/player.html?isOutside=true&aid=263809674&bvid=BV1ve411w7DL&cid=925113485&p=1&as_wide=1&high_quality=1&danmaku=0&t=30&autoplay=0" width="100%" height="500" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
</html>
