# ITraffic-monitor-for-mac 项目结构与内容总结

## 项目概述

ITraffic-monitor-for-mac 是一个 macOS 状态栏应用程序，用于监控和显示进程级别的网络流量。该项目使用 SwiftUI 构建，macOS 版本需要 >= 10.15。

## 主要功能

1. 按进程显示网络速度
2. 支持深色模式
3. 使用 nettop 的 delta 模式使统计更准确

## 目录结构

```
ITraffic-monitor-for-mac/
├── ITrafficMonitorForMac/          # 主要源代码目录
│   ├── AppDelegate.swift           # 应用程序入口和生命周期管理
│   ├── ContentView.swift           # 弹出窗口的主视图
│   ├── StatusBarView.swift         # 状态栏显示视图
│   ├── MenuItem.swift              # 菜单项组件
│   ├── Network.swift               # 网络监控核心逻辑
│   ├── ProcessEntity.swift         # 进程数据实体
│   ├── Store.swift                 # 共享存储管理
│   ├── Utils.swift                 # 工具函数
│   ├── Model/                      # 数据模型目录
│   │   ├── GlobalModel.swift       # 全局状态模型
│   │   ├── ListViewModel.swift     # 进程列表视图模型
│   │   └── StatusDataModel.swift   # 状态栏数据模型
│   ├── Assets.xcassets/            # 资源文件
│   ├── Base.lproj/                 # 本地化资源
│   ├── Preview Content/            # SwiftUI 预览资源
│   ├── dependence-resource/        # 依赖资源
│   │   └── nettop-line             # nettop 命令行工具
│   ├── Info.plist                  # 应用配置文件
│   └── ITrafficMonitorForMac.entitlements  # 权限配置
├── ITrafficMonitorForMac.xcodeproj/  # Xcode 项目配置
├── README.md                       # 项目说明文档
├── LICENSE                         # MIT 许可证
├── snapshot.png                    # 应用截图
└── .gitignore                      # Git 忽略配置
```

## 核心组件详解

### 1. AppDelegate.swift - 应用程序代理

- 管理应用程序生命周期
- 创建和管理状态栏项目
- 处理弹出窗口的显示/隐藏逻辑
- 初始化网络监控

### 2. Network.swift - 网络监控模块

- 使用 `nettop-line` 工具监听网络流量
- 解析网络数据并更新视图模型
- 实现省电模式（深度睡眠）以减少资源消耗
- 使用 `shellPipe` 方法执行命令行操作

### 3. ContentView.swift - 主内容视图

- 显示进程列表及其网络流量
- 每个进程显示：图标、名称、上传速度、下载速度
- 包含 GitHub 链接和退出按钮

### 4. StatusBarView.swift - 状态栏视图

- 在 macOS 状态栏显示总上传/下载速度
- 紧凑布局，适应状态栏空间

### 5. Store.swift - 状态管理

- 使用 `SharedStore` 枚举管理全局状态
- 提供 `ListViewModel`、`StatusDataModel`、`GlobalModel` 的共享实例

### 6. Model 目录

- **GlobalModel**: 管理视图显示状态、控制器释放状态、深度睡眠状态
- **ListViewModel**: 管理进程列表数据，包含排序和内存优化逻辑
- **StatusDataModel**: 管理状态栏显示的总流量数据

### 7. Utils.swift - 工具函数

- `formatBytes()`: 将字节数格式化为可读的速度字符串
- `getAppInfo()`: 获取应用程序图标和名称（带缓存）
- `resize()`: 图片缩放处理

### 8. ProcessEntity.swift - 进程实体

数据结构包含：
- `pid`: 进程 ID
- `name`: 进程名称
- `inBytes`: 下载字节数
- `outBytes`: 上传字节数
- `icon`: 应用图标

## 技术栈

- **语言**: Swift
- **UI 框架**: SwiftUI
- **最低系统要求**: macOS 10.15
- **网络监控**: 基于 nettop 命令行工具
- **状态管理**: ObservableObject + @Published

## 安装方式

1. 从 [Releases 页面](https://github.com/foamzou/ITraffic-monitor-for-mac/releases/latest) 下载 zip 文件
2. 使用 Homebrew：
   - 安装：`brew install itraffic`
   - 更新：`brew update && brew upgrade itraffic`

## 许可证

MIT License - 允许自由使用、修改和分发
