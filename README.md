# 基于 RuoYikuangjia 的后台管理系统

基于 RuoYi 框架构建的后台管理系统，实现顶部导航与左侧菜单联动交互，优化系统操作体验。

![image](https://github.com/Sherlock-Homles/picx-images-hosting/raw/master/20250327/image.2a59a7yr5y.webp)

## 项目特色

✨ **双菜单联动架构**

- 顶部主菜单与左侧功能菜单智能联动
- 动态切换业务模块，提升导航效率
- 采用 Vue Router 实现路由层级控制

## 技术栈

| 类别     | 技术实现             |
| -------- | -------------------- |
| 前端框架 | RuoYi-Vue 3.8        |
| 核心库   | Vue 2.x + Element UI |
| 状态管理 | Vuex                 |
| 构建工具 | Webpack 4.x          |
| 网络请求 | Axios + RESTful API  |

## 快速启动

````bash
# 克隆项目
git clone https://github.com/Sherlock-Homles/ruoyi-academy-admin.git

# 安装依赖
npm install

# 开发模式
npm run dev

# 生产构建
npm run build

浏览器访问 http://localhost:80

## 发布

```bash
# 构建测试环境
npm run build:stage

# 构建生产环境
npm run build:prod
````

## 项目结构

```bash
src/
├─ layouts/           # 布局组件
│  ├─ TopNav.vue      # 顶部导航栏
│  └─ SideMenu.vue    # 动态侧边栏
├─ views/
│  ├─ system/         # 系统管理模块
│  ├─ monitor/        # 监控中心
│  └─ components/     # 业务组件
├─ router/            # 路由配置
└─ store/             # Vuex状态管理
```

## 注意事项

1. 需配置后端 API 地址：`.env.development`
2. 生产环境开启权限校验：`src/permission.js`
3. 表格组件支持响应式布局，建议最小分辨率 1280x768
4. 用户状态切换需同步更新 Vuex 中的权限信息
