# Mermaid 示例集

面向遥感、深度学习、SLAM、3D视觉方向的实用 Mermaid 模板。

## 示例 1: SLAM 系统流程（ORB-SLAM 风格）

```mermaid
flowchart TD
    A[RGB-D / Stereo 输入] --> B[ORB 特征提取]
    B --> C{跟踪线程}
    C -->|成功| D[局部建图]
    C -->|丢失| E[重定位]
    E -->|找回| D
    E -->|失败| F[重置]
    D --> G[回环检测]
    G -->|检测到回环| H[全局 BA 优化]
    G -->|无回环| I[继续跟踪]
    H --> I
```

## 示例 2: 3D Gaussian Splatting 训练管线

```mermaid
flowchart TD
    A[COLMAP 稀疏重建] --> B[初始化 3D Gaussians]
    B --> C{迭代优化}
    C --> D[光栅化渲染]
    D --> E[计算 Loss]
    E --> F[梯度回传]
    F --> G{自适应密度控制}
    G -->|需克隆| H[Clone Gaussians]
    G -->|需分裂| I[Split Gaussians]
    G -->|需剪枝| J[Prune Gaussians]
    H --> C
    I --> C
    J --> C
    E -->|收敛| K[导出 .ply]
    K --> L[实时渲染 & 评估]
```

## 示例 3: BEV 感知管线

```mermaid
flowchart LR
    subgraph 多模态输入
        A1[6 路环视图像]
        A2[LiDAR 点云]
        A3[IMU/GPS]
    end
    subgraph 编码器
        B1[图像 Backbone<br/>ResNet/Swin]
        B2[点云 Voxelization]
        B3[位姿编码]
    end
    subgraph 视角变换
        C[LSS / Transformer<br/>PV2BEV]
    end
    subgraph BEV 特征
        D[BEV Feature Map]
    end
    subgraph 任务头
        E1[3D 检测]
        E2[BEV 分割]
        E3[轨迹预测]
    end

    A1 --> B1
    A2 --> B2
    A3 --> B3
    B1 & B2 & B3 --> C
    C --> D
    D --> E1
    D --> E2
    D --> E3
```

## 示例 4: 深度学习训练通用流程

```mermaid
flowchart TD
    A[数据集] --> B[数据加载<br/>DataLoader]
    B --> C[数据增强<br/>Augmentation]
    C --> D{Mode?}
    D -->|训练| E[前向传播]
    E --> F[计算 Loss]
    F --> G[反向传播]
    G --> H[优化器更新]
    H --> I{下一个 epoch?}
    I -->|是| E
    I -->|否| J[保存 checkpoint]
    D -->|验证| K[前向传播 no_grad]
    K --> L[计算指标]
    L --> M{早停?}
    M -->|是| N[结束训练]
    M -->|否| I
    J --> I
```

## 示例 5: 多传感器融合架构

```mermaid
flowchart TD
    subgraph 传感器层
        S1[Camera]
        S2[LiDAR]
        S3[Radar]
        S4[IMU]
    end
    subgraph 前融合
        F1[点云着色<br/>LiDAR-Camera 标定]
        F2[时间同步]
    end
    subgraph 特征融合
        M1[图像特征]
        M2[点云特征]
        M3[BEV 池化]
    end
    subgraph 后融合
        L1[单模态检测]
        L2[卡尔曼滤波<br/>匈牙利匹配]
        L3[融合轨迹]
    end

    S1 & S2 --> F1
    S3 & S4 --> F2
    F1 --> M1
    F1 --> M2
    M1 & M2 --> M3
    M3 --> L1
    L1 --> L2
    L2 --> L3
```

## 示例 6: 算法选型决策树

```mermaid
flowchart TD
    A[任务需求] --> B{需要实时性?}
    B -->|是| C{场景规模}
    B -->|否| D{精度优先}
    C -->|小场景| E[ORB-SLAM3<br/>RGB-D 模式]
    C -->|大场景| F[VINS-Mono<br/>+ 回环]
    D -->|新视角合成| G[3D Gaussian Splatting]
    D -->|表面重建| H[NeuS / VolSDF]
    D -->|大场景重建| I[Block-NeRF]
```

## 示例 7: 技术调研时序图

```mermaid
sequenceDiagram
    participant R as 研究者
    participant DS as 数据集
    participant M as 模型
    participant E as 评估

    R->>DS: 下载 KITTI/nuScenes
    DS-->>R: 数据就绪
    R->>DS: 预处理 & 划分
    R->>M: 配置训练参数
    M->>M: 训练
    M-->>R: checkpoint
    R->>E: 加载 checkpoint + 测试集
    E-->>R: mAP / ATE / PSNR
    alt 指标不达标
        R->>M: 调参 / 改结构
    else 达标
        R->>R: 写论文 / 导出结果
    end
```

## 示例 8: 类图 — 相机模型继承体系

```mermaid
classDiagram
    class Camera {
        <<abstract>>
        +Intrinsic K
        +Extrinsic RT
        +project() Point2D
        +backproject() Ray
    }
    class PinholeCamera {
        +float fx, fy, cx, cy
        +distortion()
    }
    class FisheyeCamera {
        +float k1, k2, k3, k4
        +equidistant()
    }
    class OmnidirectionalCamera {
        +float xi
        +unifiedModel()
    }

    Camera <|-- PinholeCamera
    Camera <|-- FisheyeCamera
    Camera <|-- OmnidirectionalCamera
```

## 技术栈关键词映射

当用户提及以下关键词时，推荐对应图表：

| 关键词 | 推荐图表 | 理由 |
|-------|---------|------|
| pipeline/管线/流程 | Flowchart | 步骤间的数据流 |
| 训练/优化/迭代 | Flowchart with loop | 循环 + 条件判断 |
| 对比/选型/A vs B | Flowchart decision tree | 分叉判断 |
| 调用/请求/通信 | Sequence Diagram | 时间顺序交互 |
| 架构/模块/系统 | Flowchart with subgraph | 模块划分 |
| 状态/生命周期 | State Diagram | 状态转移 |
| 继承/接口/基类 | Class Diagram | OOP 关系 |
| 时间线/排期/计划 | Gantt Chart | 时间规划 |
| 占比/分布/构成 | Pie Chart | 比例可视化 |

## 配合 other skills 使用

- **nature-paper2ppt**: 流程图代码块嵌入 Markdown 幻灯片，VS Code 预览后截图放入 PPTX
- **nature-writing**: 方法论部分描述算法流程时，配流程图增强可读性
- **nature-figure**: 流程图可作为论文 Figure 1（系统总览图），导出 SVG 后拼入多面板图
