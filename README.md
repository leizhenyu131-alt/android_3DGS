# Mobile-Vulkan-3DGS（暂时未完成）

这是一个专门为 Android 移动平台设计和优化的高性能 3D Gaussian Splatting (3DGS) 实时渲染引擎。项目基于 **Vulkan API** 与 **Android NDK (C++)** 构建，利用 GPU Compute Shader (计算管线) 与 SSBO 技术，在移动端（如 Qualcomm Adreno 830 等现代 GPU 架构）上实现数百万级高斯点云的低延迟、高帧率点投影与光栅化渲染。

## 🚀 核心特性

- **原生 Android Vulkan 驱动**：摆脱传统的跨平台层包装，直接通过 `ANativeWindow` 与 Vulkan Surface 绑定，最大化榨干移动端 GPU 性能。
- **Compute Pipeline 加速**：基于 Compute Shader 实现高斯点云的前向投影（Preprocess）、协方差矩阵计算（Cov3D）以及并行排序。
- **高效内存管理**：采用 Staging Buffer（中转缓冲）机制，将 PLY 数据从 CPU 异步安全地搬运至 GPU 专用的 `VK_MEMORY_PROPERTY_DEVICE_LOCAL_BIT` 高速显存（SSBO）中。
- **JNI 编排架构**：Java/Kotlin 负责生命周期、UI 与 Asset 资源调度，C++ NDK 核心层负责点云解析与极致的 Vulkan 渲染循环。

## 🛠️ 技术栈

- **前端/宿主**：Kotlin / Android SDK (Min API 26+)
- **图形核心**：C++17 / Vulkan SDK 1.1+ / Android NDK
- **数学计算**：GLM (OpenGL Mathematics)
- **数据解析**：PLY 格式点云解析器

## 📁 项目结构

```text
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── assets/
│   │   │   │   └── models/
│   │   │   │       └── demo_fox_gs.ply      # 默认测试高斯点云模型
│   │   │   ├── cpp/
│   │   │   │   ├── 3dgs/                    # 3DGS 核心逻辑与场景解析
│   │   │   │   │   ├── GSScene.h / .cpp
│   │   │   │   ├── vulkan_impl/             # Vulkan 基础设施封装
│   │   │   │   │   ├── VulkanCore.h / .cpp  # Instance/Device/Buffer 管理
│   │   │   │   │   └── Renderer.h / .cpp    # 渲染中枢与数据上传
│   │   │   │   ├── CMakeLists.txt           # NDK 编译配置文件
│   │   │   │   └── native-lib.cpp           # JNI 交互核心桥梁
│   │   │   └── java/com/example/vulkan_test/
│   │   │       └── MainActivity.kt          # 应用程序入口与 Surface 生命周期
