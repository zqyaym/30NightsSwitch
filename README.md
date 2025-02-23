# 健康睡眠计划追踪应用

一个帮助用户建立健康睡眠习惯的Vue3应用，通过任务追踪、数据可视化和成就系统来激励用户坚持30天的睡眠改善计划。

## 功能特点

```mermaid
graph TD
    A[主要功能] --> B[任务追踪]
    A --> C[数据统计]
    A --> D[激励系统]
    
    B --> B1[每日任务清单]
    B --> B2[完成度追踪]
    
    C --> C1[睡眠数据图表]
    C --> C2[进度环形图]
    
    D --> D1[成就徽章]
    D --> D2[连续打卡]
    
    style A fill:#4B89DC,stroke:#2C3E50,color:#FFF
    style B fill:#2ECC71,stroke:#2C3E50,color:#FFF
    style C fill:#F1C40F,stroke:#2C3E50,color:#FFF
    style D fill:#E74C3C,stroke:#2C3E50,color:#FFF
```

### 核心功能

1. **任务追踪系统**
   - 每日睡眠任务清单
   - 实时进度追踪
   - 任务完成度统计

2. **数据可视化**
   - 环形进度指示器
   - 睡眠数据趋势图表
   - 直观的完成度展示

3. **激励机制**
   - 成就徽章系统
   - 连续打卡记录
   - 每日激励语录

## 技术栈

```mermaid
flowchart LR
    Vue3[Vue 3] --> Vite[Vite]
    Vue3 --> VueUse[VueUse]
    Vue3 --> GSAP[GSAP]
    
    style Vue3 fill:#42b883,stroke:#35495e,color:#FFF
    style Vite fill:#646cff,stroke:#535bf2,color:#FFF
    style VueUse fill:#2C3E50,stroke:#34495E,color:#FFF
    style GSAP fill:#88CE02,stroke:#2C3E50,color:#FFF
```

- **Vue 3**: 核心框架
- **Vite**: 构建工具
- **VueUse**: 实用组合式API集合
- **GSAP**: 动画效果库

## 项目结构

```
├── src/
│   ├── components/        # 可复用组件
│   │   ├── ProgressRing   # 进度环形图
│   │   ├── TaskList      # 任务列表
│   │   └── ...
│   ├── views/            # 页面组件
│   └── App.vue          # 根组件
├── public/              # 静态资源
└── package.json        # 项目配置
```

## 使用指南

1. 安装依赖：
```bash
npm install
```

2. 启动开发服务器：
```bash
npm run dev
```

3. 构建生产版本：
```bash
npm run build
```

## 数据流

```mermaid
sequenceDiagram
    participant U as 用户
    participant C as 组件
    participant S as 状态管理
    participant L as 本地存储
    
    U->>C: 完成任务
    C->>S: 更新状态
    S->>L: 保存进度
    L-->>S: 读取数据
    S-->>C: 更新UI
    C-->>U: 显示反馈
```

## 贡献

欢迎提交问题和改进建议！

## 许可

MIT License