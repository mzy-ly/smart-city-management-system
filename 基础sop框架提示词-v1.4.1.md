<!--以下提示词分为2步，请分开执行
1、项目初始化、布局与样式规范
2、组件功能、路由、状态管理
-->
# 第1步：项目初始化、布局与样式规范

## 项目概述
搭建一套最基础的各行业通用项目框架。

## 技术栈
- Node.js
- Vue 3 (Composition API)
- Vite 4
- TypeScript
- Element Plus

## 目录结构
src/
├── assets/ # 静态资源
│ └── images/# 图片
├── components/ # 公共组件
├── views/ # 页面视图
├── store/
│ └── auth.ts
├── types/ # TypeScript 类型定义
│ └── index.ts
├── styles/ # 全局样式
│ └── main.css
├── App.vue
└── main.ts

## 其他要求：
   - 我已经创建好当前项目的文件夹，你无需再次创建最外层目录
   - 创建项目目录要一个个创建，不要一次性创建多个目录
   - 检测是否安装node.js，如果未安装则安装，若已安装则无需再次安装
   - 安装必要的依赖，包括 Node.js、Element Plus、Vue Router、Pinia
   - 安装 TypeScript 及相关类型定义包
   - 安装 @vue/tsconfig 配置 tsconfig.json 文件
   - 安装 @vitejs/plugin-vue 插件并配置支持 TypeScript 的 vite.config.ts
   - 创建一个简洁的 README.md 文件，包含项目介绍和启动方法
   - 确保代码是响应式的，不需要兼容移动端，兼容主流浏览器,如Google、Edge、Safari等
   - Element Plus 组件按需导入
   - logo请用颜色代替即可，无需放真实的图片
   - 执行完后，检查一下是否有报错的文件，如果有请修复，没有则继续

## App.vue中根据路由元信息显示布局（示例）：
  ```vue
  <template>
    <AppHeader v-if="showLayout" />
    <AppSidebar v-if="showLayout" />
    <main :class="{'main-content': showLayout}">
      <router-view />
    </main>
  </template>

  <script setup>
    import { computed } from 'vue'
    import { useRoute } from 'vue-router'

    const route = useRoute()
    const showLayout = computed(() => route.meta.requiresLayout !== false)
  </script>
  ```
## 配置 tsconfig.json (避免 extends 错误):
     ```json
     {
       "compilerOptions": {
         "target": "ESNext",
         "module": "ESNext",
         "moduleResolution": "Node",
         "strict": true,
         "jsx": "preserve",
         "sourceMap": true,
         "resolveJsonModule": true,
         "esModuleInterop": true,
         "lib": ["ESNext", "DOM"],
         "types": ["vite/client"],
         "baseUrl": "./",
         "paths": {
           "@/*": ["src/*"]
         }
       },
       "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"]
     }
     ```
## 配置 vite.config.ts:
     ```ts
     import { defineConfig } from 'vite'
     import vue from '@vitejs/plugin-vue'
     import path from 'path'

     export default defineConfig({
       plugins: [vue()],
       resolve: {
         alias: {
           '@': path.resolve(__dirname, './src')
         }
       },
       server: {
         port: 3000
       }
     })
     ```
## Element Plus 按需导入配置 (在 main.ts):
   - 在main.ts中确保正确注册Element Plus:
   - 导入并全局注册ElementPlus
   - 导入并全局注册所有ElementPlus图标组件
   - 使用app.use(ElementPlus)注册组件库
     ```ts
     import { createApp } from 'vue'
     import App from './App.vue'
     import ElementPlus from 'element-plus'
     import 'element-plus/dist/index.css'

     const app = createApp(App)
     app.use(ElementPlus)
     ```

# 布局与样式规范

## 布局排版要求
采用经典「顶部导航 + 左侧菜单 + 右侧内容区」布局模式，三者背景均为白色：
1. 100%视口高度布局，无全局滚动条
2. AppHeader.vue (顶部标题栏)：固定定位，高度60px，100%宽度，z-index: 100，背景色：白色(#FFFFFF)
  - 标题栏结构：左侧显示[logo] [系统名称] [面包屑] 右侧显示[通知图标] [用户头像] [用户名] 
  - 交互：触发区域：用户头像 + 用户名组合区域，交互方式：点击后显示下拉弹窗
  - 下拉弹窗内容：个人信息选项、退出登录选项（点击退出按钮清除认证状态并跳转登录页、外部点击自动关闭菜单）、分隔线区分功能区域
  - 使用Element plus 的icons-vue
  - 下拉弹窗位置：用户区域正下方，右对齐，宽度：160px，样式：白色背景 + 阴影 + 圆角，动画：0.2秒淡入+下拉动画

3. AppSidebar.vue (左侧导航栏)：固定定位，宽度200px，高度 calc(100vh - 60px)，z-index: 90，背景色：白色(#FFFFFF)
  - 路由激活状态高亮显示
  - 支持多级菜单（可选）
  - 一级菜单项包含图标+文字，二级菜单项只有文字，如果没有图标时，文字要与一级菜单的文字居左对齐。
  - 使用el-menu组件，设置:router="true"实现路由导航
  - 正确嵌套el-menu-item和el-sub-menu组件
  - 为每个菜单项添加图标和文本
  - 导航栏包含一键折叠功能

4. AppBreadcrumb.vue (面包屑导航)：固定定位
  - 位置：位于主内容区顶部，在router-view上方
  - 数据源：基于路由元信息自动生成层级路径
  - 交互：
    - 最后一级为当前页面（不可点击）
    - 前面层级可点击返回对应页面
    - 一级菜单是不可点击的，防止用户点击标记为不可点击的菜单项
  - 响应式：在窄屏时自动折叠中间层级（显示...）
  - 样式规范：
    - 字体大小：14px
    - 分隔符：/
    - 当前页文字加粗
    - 颜色使用主题色变量

5. 主内容区布局规范：
  1. **位置与边距**：
    - 左侧：距离导航栏30px
    - 顶部：距离面包屑区域30px（面包屑位于标题栏下方）
    - 内边距：20px（四周统一）

  2. **动态尺寸**：
    - 宽度：calc(100% - 30px)（全屏宽度减去左侧间距）
    - 高度：calc(100% - 标题栏高度 - 面包屑高度 - 30px)

  3. **视觉样式**：
    - 背景色：白色(#FFFFFF)
    - 内容居中显示"Hello"文字
    - 使用 .main-content 类控制样式
        ```css
      .main-content {
        overflow-x: hidden;
        max-width: calc(100vw - var(--sidebar-width));
        box-sizing: border-box;
      }
      ```

  4. **层级关系**：
    - 位于标题栏之下
    - 与导航栏同级

  5. **注意事项**：
    - 确保边距计算包含标题栏和面包屑高度
    - 内边距不影响内容居中
    - 需响应式适配不同屏幕

6. 在components内完成AppHeader.vue顶部标题栏组件

7. 在components内完成AppSidebar.vue左侧菜单导航栏组件

8. 在components内完成AppBreadcrumb.vue面包屑导航，位于标题栏中

9. 样式规范：
   - 使用 CSS 变量统一管理颜色和尺寸
   - 组件样式使用 scoped 属性限制作用范围
   - 全局样式只定义通用样式，避免与组件样式冲突
   - 布局相关的样式应该只在一个地方定义

## 布局要点
1. 使用box-sizing: border-box确保尺寸计算准确
2. 固定定位元素使用z-index分层管理
3. 主内容区使用margin避开固定定位元素
4. 使用calc()函数计算动态高度
5. 所有间距使用统一CSS变量保证一致性
6. 重要：不要在多处定义相同的 CSS 类，如 app-container 等

## 精确布局要点
1. 尺寸控制：使用CSS变量统一关键尺寸,主内容区使用calc(100vh - var(--header-height))确保高度正确,固定定位元素使用精确的z-index值。
2. 溢出处理：侧边栏设置overflow-y: auto防止内容溢出,主内容区使用max-width限制最大宽度,卡片使用minmax()函数确保响应式布局。
3. 对齐方式：使用Flexbox确保元素居中对齐,网格布局保证卡片等宽,统一的内边距和外边距值。
4. 视觉一致性：统一阴影效果（box-shadow），统一边框圆角（border-radius），统一颜色变量。

### 布局结构应为:
   <div class="app-container">
     <AppHeader />
     <AppSidebar />
     <main class="main-content">
       <AppBreadcrumb />
       <router-view />
     </main>
   </div>

### CSS变量控制关键尺寸：
   ```css
   :root {
     --header-height: 60px;
     --sidebar-width: 200px;
     --content-padding: 20px;
   }
   ```

### 盒模型控制：
  ```css
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  ```
### 分层管理：
  ```css
  .app-header { z-index: 100; }
  .app-sidebar { z-index: 90; }
  ```
### 动态计算尺寸
  ```css
  .app-sidebar {
    height: calc(100vh - var(--header-height));
  }
  ```
### 间距一致性
  ```css
  :root {
    --gap-sm: 8px;
    --gap-md: 16px;
    --gap-lg: 24px;
  }
  ```
## 启动方式
```bash
npm install
npm run dev
```
完成后运行项目并提供访问地址

# 第2步：组件功能、路由、状态管理

## 首页实现细节
首页应包含以下内容：
1. 顶部统计卡片区域：
   - 用户总数卡片（蓝色，图标：User）：显示"1,234"
   - 访问量卡片（绿色，图标：View）：显示"45,678"
   - 文章数卡片（橙色，图标：Document）：显示"356"
   - 系统消息卡片（红色，图标：Setting）：显示"12"

2. 中部图表区域：
   - 左侧访问统计图表，包含"本周/本月/全年"切换选项
   - 右侧用户分布图表

3. 底部表格区域：
   - 最近活动表格，包含时间、用户、操作、状态列
   - 表格数据包含5条记录，显示不同用户的系统操作记录
   - 状态列使用el-tag组件，成功为绿色，失败为红色

## Login.vue (登录页)功能要求：
1. 独立布局（无顶部栏/侧边栏）
2. 全屏flex布局，左侧占比50%，右侧占比50%
   - 左侧：渐变背景（从#667eea到#764ba2，135度角）
   - 右侧：浅灰背景(#f5f7fa)，居中显示登录卡片

3. 登录卡片结构与样式：
   - 宽度：420px，高度自适应
   - 内边距：40px
   - 背景色：白色
   - 圆角：8px
   - 阴影：0 2px 12px 0 rgba(0, 0, 0, 0.1)
   - 响应式：在小屏幕(768px以下)时隐藏左侧区域，卡片宽度为90%

4. 卡片内容自上而下排列：
   a. 标题区：
      - 主标题："管理系统"，28px，#303133，居中
      - 副标题："欢迎使用，请登录"，14px，#909399，居中
      - 下方间距：20px
   
   b. 提示区：
      - Element Plus的Alert组件，info类型
      - 内容："示例账号：admin / 密码：123456"
      - 不可关闭
      - 下方间距：20px
   
   c. 表单区：
      - 用户名输入框：前缀图标User，placeholder="用户名"
      - 密码输入框：前缀图标Lock，type="password"，placeholder="密码"，支持显示/隐藏
      - 验证码区域：
        * 左侧输入框，占比较大
        * 右侧验证码图片(120x50px)，可点击刷新
        * 验证码为4位随机数字，蓝底白字
      - 记住我选项与忘记密码链接在同一行，两端对齐
      - 登录按钮：100%宽度，40px高，主题色，圆角4px

5. 交互功能实现：
   - 表单验证规则：
     * 用户名：必填，长度5-10位
     * 密码：必填，长度5-10位
     * 验证码：必填，长度4位
   - 验证码生成与验证：
     * 使用Math.floor(1000 + Math.random() * 9000)生成4位随机数
     * 存储在localStorage中用于验证
     * 使用dummyimage.com服务生成验证码图片
     * 点击图片可刷新验证码
   - 登录逻辑：
     * 先验证表单
     * 再验证验证码是否匹配
     * 模拟1秒API请求延迟
     * 验证用户名密码(admin/123456)
     * 登录成功后存储token并跳转首页
     * 失败时显示错误信息并刷新验证码

```typescript
logout() {
  // 清除状态
  this.id = null
  this.name = null
  this.avatar = null
  this.token = null
  this.isAuthenticated = false
  
  // 清除本地存储
  localStorage.removeItem('token')
  localStorage.removeItem('userInfo')
  
  // 关键代码：直接跳转到登录页
  setTimeout(() => {
    router.push('/login')
  }, 0)
}
```
# 路由与状态管理

## 路由配置要求

- 使用Vue Router实现路由管理
- 增加目录
   ├── router/ # 路由配置
   │ └── index.ts

```javascript
// 路由守卫逻辑
interface RouteMeta {
  requiresAuth?: boolean;
  requiresLayout?: boolean;
  isPublic?: boolean;
}

const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    component: Home,
    meta: { 
      requiresAuth: true,
      requiresLayout: true
    } as RouteMeta
  },
  {
    path: '/login',
    component: Login,
    meta: { 
      isPublic: true,
      requiresLayout: false
    } as RouteMeta
  },
  {
    path: '/404',
    component: NotFound,
    meta: { requiresLayout: false }
  }
]

// 路由守卫逻辑 - 现代写法（Vue Router 4.x）
router.beforeEach((to: RouteLocationNormalized, from: RouteLocationNormalized) => {
  const auth = useAuthStore();
  
  // 需要认证但未登录 → 重定向登录页
  if (to.meta.requiresAuth && !auth.isAuthenticated) {
    return { path: '/login', query: { redirect: to.fullPath } };
  }
  
  // 已登录但访问登录页 → 重定向首页
  if (to.path === '/login' && auth.isAuthenticated) {
    return { path: '/' };
  }
});
```

## 状态管理要求

- 使用Pinia(状态管理)进行状态管理
- 增加目录
├── store/
│ └── auth.ts

auth.ts 模块
状态属性：
- isLoggedIn: 登录状态布尔值
- user: 当前用户对象

方法：
1. login(): 处理登录逻辑，存储认证状态
2. logout(): 
   - 清除认证状态
   - 清除用户数据
   - 跳转到登录页

## 路由守卫测试
```TypeScript 
// 路由守卫router/index.ts
router.beforeEach((to: RouteLocationNormalized, from: RouteLocationNormalized) => {
  const auth = useAuthStore()
  
  // 需要认证但未登录 → 重定向登录页
  if (to.meta.requiresAuth && !auth.isAuthenticated) {
    return { path: '/login', query: { redirect: to.fullPath } }
  }
  
  // 已登录但访问登录页 → 重定向首页
  if (to.path === '/login' && auth.isAuthenticated) {
    return { path: '/' }
  }
})
```
## 路由认证与登录跳转专项规范
1. 初始化顺序
  - `main.ts` 中，先 `use(pinia)`，再 `use(router)`，最后 `app.mount`
  - 确保 store 初始化先于路由系统，避免路由守卫中访问未初始化的 store

2. 退出登录处理规范
  - 退出登录时，先清空 `localStorage`，再重置 store 状态
  - 对于需要完全重置应用状态的退出操作，考虑使用 `window.location.href = '/login'` 进行强制刷新
  - 避免在退出流程中使用复杂的异步操作或多层嵌套回调
  - 点击完退出登录按钮后，需要跳转到登录页

3. 状态一致性保障
  - 确保 `localStorage` 和 store 中的认证状态始终同步
  - `logout` 方法应同时清空 `token`、`user` 和相关的 `localStorage` 项
  - 关键状态变更后，确认状态已更新再执行后续操作

## 其他要求
  - 整体执行完，需要检查内容区页面是否有嵌套问题，尤其是APP.vue等文件。