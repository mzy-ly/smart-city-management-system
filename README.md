# 智慧城市管理系统

基于 Vue 3 + TypeScript + Vite + Element Plus 构建的现代化智慧城市管理系统。

## 项目截图

### 登录页面
![登录页面](docs/images/login.png)

### 指挥中心
![指挥中心](docs/images/command-center.png)

### 生态环保
#### 扬尘监测
![扬尘监测](docs/images/dust-monitor.png)

#### 河道监测
![河道监测](docs/images/water-monitor.png)

#### 垃圾分类
![垃圾分类](docs/images/garbage-sort.png)

### 交通管理
#### 信号控制
![信号控制](docs/images/signal-control.png)

#### 独居老人关怀
![独居老人关怀](docs/images/elderly-care.png)

### 个人中心
![个人中心](docs/images/profile.png)

## 功能特点

### 1. 指挥中心
- 指挥中心：城市运营指挥中心，实时监控和调度
- 事件协同：跨部门事件处理和协作平台
- 城市总览：城市核心指标可视化展示

### 2. 交通管理
- 信号控制：智能交通信号灯管理
- 公交预测：公交到站时间预测
- 停车管理：城市停车场实时监控和管理

### 3. 生态环保
- 扬尘监测：工地和道路扬尘监测
- 河道监测：水质监测和预警
- 垃圾分类：垃圾分类管理和督导

### 4. 社区服务
- 养老关怀：老年人关怀服务管理
- 租赁监管：房屋租赁市场监管
- 物业服务：物业服务监管和评价

### 5. 系统管理
- 用户管理：系统用户权限管理
- 角色管理：角色权限配置

## 技术栈

- **前端框架**：Vue 3
- **开发语言**：TypeScript
- **构建工具**：Vite
- **UI 框架**：Element Plus
- **状态管理**：Pinia
- **路由管理**：Vue Router
- **图表库**：ECharts
- **地图组件**：高德地图 JSAPI
- **Markdown 编辑器**：v-md-editor

## 开发环境要求

- Node.js >= 16.0.0
- npm >= 8.0.0

## 项目设置

```bash
# 安装依赖
npm install

# 启动开发服务器
npm run dev

# 构建生产版本
npm run build
```

## 项目结构

```
src/
├── assets/          # 静态资源
├── components/      # 公共组件
├── router/          # 路由配置
├── store/           # 状态管理
├── styles/          # 全局样式
├── types/           # TypeScript 类型定义
└── views/          # 页面组件
    ├── community/   # 社区服务模块
    ├── environment/ # 生态环保模块
    ├── ioc/         # 指挥中心模块
    ├── system/      # 系统管理模块
    └── traffic/     # 交通管理模块
```

## 主要功能模块

### 指挥中心模块
- 城市运营指标监控
- 跨部门协同处理
- 应急事件快速响应
- 数据可视化展示

### 交通管理模块
- 实时路况监控
- 智能信号灯控制
- 公交到站预测
- 停车场管理

### 生态环保模块
- 空气质量监测
- 水质监测预警
- 垃圾分类管理
- 环境数据分析

### 社区服务模块
- 老年人关怀服务
- 房屋租赁监管
- 物业服务评价
- 社区事务处理

## 开发规范

### 代码规范
- 使用 ESLint 进行代码检查
- 使用 Prettier 进行代码格式化
- 遵循 Vue 3 组合式 API 风格指南

### 命名规范
- 组件名：PascalCase
- 文件名：kebab-case
- 变量名：camelCase
- 常量名：UPPER_CASE

### Git 提交规范
- feat: 新功能
- fix: 修复问题
- docs: 文档变更
- style: 代码格式
- refactor: 代码重构
- perf: 性能优化
- test: 测试相关
- chore: 构建过程或辅助工具的变动

## 注意事项

1. 开发前请确保已安装所有依赖
2. 使用高德地图功能需要配置 API Key
3. 注意保护敏感配置信息
4. 定期进行代码备份

## 贡献指南

1. Fork 本仓库
2. 创建特性分支
3. 提交变更
4. 推送到分支
5. 创建 Pull Request

## 许可证

[MIT](LICENSE)

## 联系方式

- 项目负责人：[姓名]
- 邮箱：[邮箱地址]
- 问题反馈：[Issue 链接]
