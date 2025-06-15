<!--Copyright © 适用于[License](https://github.com/chenzomi12/AISystem)版权许可-->

# 昇腾推理引擎 MindIE

本节将介绍华为昇腾推理引擎 MindIE 的详细内容，包括其基本介绍、关键功能特性以及不同组件的详细描述。

本节内容将深入探讨 MindIE 的三个主要组件：MindIE-Service、MindIE-Torch 和 MindIE-RT，以及它们在服务化部署、大模型推理和推理运行时方面的功能特性和应用场景。通过本节的介绍，读者将对 MindIE 有一个全面的了解，包括其如何支持 AI 业务的高效运行和模型的快速部署。

## MindIE 基本介绍

MindIE（Mind Inference Engine，昇腾推理引擎）是华为昇腾针对 AI 全场景业务的推理加速套件。通过分层开放 AI 能力，支撑用户多样化的 AI 业务需求，使能百模千态，释放昇腾硬件设备算力。支持多种主流 AI 框架，提供多层次编程接口，帮助用户快速构建基于昇腾平台的推理业务。

业界标准 RPC 接口高效对接业务层，支持 Triton 和 TGI 等主流推理服务框架，实现小时级应用部署。提供针对 LLM（transformer）和文生图（SD 模型）的加速参考代码和预置模型，开箱性能业界领先。少量代码实现训练向推理平滑迁移，昇腾训推同构小时级模型迁移，以及 GPU 模型向昇腾 2 人周高效迁移。

昇腾推理引擎支持请求并发调度和模型多实例并发调度，支持多种异步下发，多流水执行，实现高效的推理加速。支持从 PyTorch 和昇思对接从训练模型转换推理模型的过程，支持多种推理服务框架和兼容接口。提供基于昇腾架构亲和加速技术，覆盖推理全流程的图转换、组网、编译、推理执行、调试调优接口。

已发布 MindIE Service、MindIE Torch、MindIE RT 三个组件。

### MindIE-Service

MindIE-Service 针对通用模型的推理服务化场景，实现开放、可扩展的推理服务化平台架构，支持对接业界主流推理框架接口，满足大语言模型、文生图等多类型模型的高性能推理需求。

MindIE-Server 作为推理服务端，提供模型服务化能力；MindIE-Client 提供服务客户端标准 API，简化用户服务调用。MindIE-Service 向下调用了 MindIE-LLM 组件能力。

### MindIE-Torch

MindIE-Torch 是针对 Pytorch 框架模型的推理加速插件。Pytorch 框架上训练的模型利用 MindIE-Torch 提供的简易 C++/Python 接口，少量代码即可完成模型迁移，实现高性能推理。MindIE-Torch 向下调用了 MindIE-RT 组件能力。

### MindIE-RT

MindIE-RT 是面向昇腾 AI 处理器的推理加速引擎，提供模型推理迁移相关开发接口及工具，能够将不同的 AI 框架（PyTorch、ONNX 等）上完成训练的算法模型统一为计算图表示，具备多粒度模型优化、整图下发以及推理部署等功能。集成 Transfomer 高性能算子加速库 ATB，提供基础高性能算子，和高效的算子组合技术（Graph）便于模型加速。

## 关键功能特性

### 服务化部署

MindIE-Service 是面向通用模型的推理服务化场景，实现开放、可扩展的推理服务化平台架构，支持对接业界主流推理框架接口，满足大语言模型、文生图等多类型模型的高性能推理需求。它的组件包括 MindIE-Server、MindIE-Client、Benchmark 评测工具等，一方面通过对接昇腾的推理加速引擎带来大模型在昇腾环境中的性能提升，另一方面，通过接入现有的主流推理框架生态，逐渐以性能和易用性牵引存量生态的用户向全自研推理服务化平台迁移。

支持的特性：

- 支持大模型服务化快速部署。

-	提供了标准的昇腾服务化接口，兼容 Triton/OpenAI/TGI/vLLM 等第三方框架接口。

-	支持 Continuous Batching，PagedAttention。

-	支持基于 Transformer 推理加速库（Ascend Transformer Boost）的模型接入，继承其加速能力，包括融合加速算子、量化等特性。

### 大模型推理

提供大模型推理能力，支持大模型业务全流程，逐级能力开放，使能大模型客户需求定制化。

1. Pytorch 模型迁移

对接主流 Pytorch 框架，实现训练到推理的平滑迁移，提供通用的图优化并行推理能力，提供用户深度定制优化能力。MindIE-Torch 是推理引擎组件中针对 Pytorch 框架模型的推理加速插件。Pytorch 框架上训练的模型利用 MindIE-Torch 提供的简易 C++/Python 接口，少量代码即可完成模型迁移，实现高性能推理。

2. MindIE-Torch TorchScript 支持以下功能特性

-	支持 TorchScript 模型的编译优化，生成可直接在昇腾 NPU 设备加速推理的 TorchScript 模型。

- 支持静态输入和动态输入，动态输入分为动态 Dims 和 ShapeRange 两种模式。

- 编译优化时支持混合精度、FP32 以及 FP16 精度策略。

- 支持用户自定义 converter 和自定义 pass。

- 支持异步推理和异步数据拷贝。

- 支持与 torch_npu 配套使用，算子可 fallback 到 torch_npu 执行。

- 支持多语言 API（C++、Python）。

3. MindIE-Torch ExportedProgram 支持以下功能特性：

- 支持 ExportedProgram 的编译优化，生成可直接在昇腾 NPU 设备加速推理的 nn.Module 模型。

- 支持静态输入和动态 ShapeRange 输入。

- 编译优化时支持混合精度、FP32、FP16 精度策略。

- 支持异步推理和异步数据拷贝。

- 支持 Python API。

### 推理运行时

集成推理应用接口及 Transformer 加速库，提供推理迁移相关开发接口及工具，提供通用优化及并行推理能力》。MindIE-RT（Mind Inference Engine RT，昇腾推理引擎运行时）是针对昇腾 AI 处理器的推理加速引擎，提供 AI 模型推理场景下的商业化部署能力，能够将不同的 AI 框架上完成训练的算法模型统一为计算图表示，具备多粒度模型优化、整图下发以及推理部署等功能。

MindIE-RT 集成昇腾高性能算子加速库 ATB，为实现基于 Transformer 的神经网络推理加速引擎库，库中包含了各类 Transformer 类模型的高度优化模块，如 Encoder 和 Decoder 部分。

MindIE-RT 专注于为用户提供快速迁移、稳定精度以及极致性能的推理服务，让用户能够脱离底层硬件细节和不同平台框架的差异，专注于推理业务本身，实现高效的模型部署开发。并且专门针对大模型下的 Transformer 架构，提高 Transformer 模型性能，提供了基础的高性能的算子，高效的算子组合技术（Graph），方便模型加速。目前 MindIE-RT 已实现动态输入推理，解析框架模型等功能特性。

1. MindIE-RT 支持以下功能特性

- 支持多语言 API（C++, Python）：详情参见 C++编程模型和 Python 编程模型。

- 提供 parser，支持直接导入 AI 框架 ONNX 模型，详情参见解析框架模型。

- 支持 Transformer 算子加速库，集成基础高性能算子，详情可见 ATB 高性能加速库使用。

- 支持丰富的编译时优化方法和运行时优化方法，用户可以在昇腾 AI 处理器上占用较少的内存，部署更高性能的推理业务，提供的优化方法如：精度优化和常量折叠。

2. 应用场景

MindIE-RT 是基于昇腾 AI 处理器的部署推理引擎，适用于通过 NPU、GPU、CPU 等设备训练的算法模型，为其提供极简易用且灵活的接口，实现算法从训练到推理的快速迁移。目前 MindIE-RT 的快速迁移能力已支持以下业务场景：

- 计算机视觉。

- 自然语言处理。

- 推荐、检索。

- 大模型对话。

## 小节与思考

- MindIE 概述：MindIE 是华为昇腾推出的 AI 推理加速套件，提供多层次编程接口和支持多种 AI 框架，实现高效推理加速和快速模型部署。

- 关键功能特性：MindIE 具备服务化部署、大模型推理能力，以及集成推理应用接口和 Transformer 加速库，支持多语言 API，优化模型迁移和推理性能。

- 应用场景：MindIE 适用于计算机视觉、自然语言处理、推荐检索以及大模型对话等多种 AI 应用场景，提供从训练到推理的快速迁移和高性能部署能力。
