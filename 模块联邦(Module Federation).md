# 模块联邦

注：

什么是模块联邦

模块联邦（Module Federation）是一种现代化的前端开发模式，它允许将多个独立的应用程序或库拆分成更小的、可复用的模块，并在运行时动态加载这些模块。这种模式特别适用于微前端架构，因为它可以实现多个独立应用程序之间的代码共享和协同工作。

模块联邦的核心概念包括：

- 模块：可以独立运行、测试和部署的代码单元。
- 共享：在不同应用程序之间共享模块的能力。
- 动态加载：在运行时根据需要加载模块，而不是在构建时静态链接。

模块联邦的主要优势包括：

- 代码复用：通过共享模块，可以减少重复代码，提高开发效率。
- 灵活性：可以根据需要动态加载模块，而不是在构建时静态链接。
- 可维护性：通过将代码拆分成更小的模块，可以提高代码的可维护性。
- 可扩展性：可以通过添加新的模块来扩展应用程序的功能。

## 一、docs

[Webpack Module Federation](https://webpack.docschina.org/concepts/module-federation/)

[Module Federation 微前端架构](https://github.com/module-federation/core)

## 二、Module Federation 微前端架构

项目背景：是字节旗下的一个开源项目，用于解决微前端架构中的代码共享问题。已在字节内部广泛使用，并被多个团队成功应用于生产环境。

### 1、创建项目

```bash
npm create module-federation@latest
```

### 2、整个项目抛出

[docs](https://module-federation.io/zh/practice/bridge/overview.html)

#### 1）下载插件

注：主、子项目，都要下载

```bash
# React 与模块联邦结合 设计的 桥接层
npm install @module-federation/bridge-react@latest
# 为 RSBuild（字节跳动开源的构建工具） 提供模块联邦的一键式构建配置支持，无需手动编写复杂的 Module Federation 构建配置，让 RSBuild 工程快速接入模块联邦
npm install @module-federation/rsbuild-plugin
# react 路由
npm install react-router
```

#### 2）子应用

##### a、创建容器

/src/export-app.tsx

```tsx
import App from './App';
// 也可使用 react-dom
import { BrowserRouter } from 'react-router';
import { createBridgeComponent } from '@module-federation/bridge-react/v19';

// 包装一层 Router，避免远程被宿主消费时缺少路由上下文
const RemoteApp = () => (
  <BrowserRouter>
    <App />
  </BrowserRouter>
);

export default createBridgeComponent({
  rootComponent: RemoteApp,
});
```

##### b、导出

```ts
import { defineConfig } from '@rsbuild/core';
import { pluginReact } from '@rsbuild/plugin-react';
import { pluginModuleFederation } from '@module-federation/rsbuild-plugin';

// Docs: https://rsbuild.rs/config/
export default defineConfig({
  server: {
    port: 3002
  },
  plugins: [
    pluginReact(),
    pluginModuleFederation({
      // name 起的名字，在主应用中，也要用
      name: 'remote1',
      // TODO 默认应该就是 remoteEntry.js，但是现在不定义 remoteEntry.js的话，http://localhost:3002/remoteEntry.js 无法访问
      filename: 'remoteEntry.js',
      exposes: {
        // key 的名字，代表是你要导出的模块，在夫应用中要使用，使用的方式为：remote1/export-app
        './export-app': './src/export-app.tsx', // 导出应用类型远程模块
      },
      bridge: {
        // 启用 Bridge Router 路由能力，默认为 true
        enableBridgeRouter: true, 
      }
    }),
  ],
});

```

#### 3）主容器

src/components/index.ts

```ts
export { default as ComponentLoading } from "./component-loading/index.tsx";
export { default as ComponentErrorFallback } from "./component-error-fallback/index.tsx";
```

##### a、创建子应用加载 loading

src/components/component-loading/index.tsx

```tsx
const ComponentLoading = (): React.ReactNode => (
  <div style={{ padding: '20px', textAlign: 'center' }}>
    <div>远程应用加载中...</div>
  </div>
);
export default ComponentLoading;
```

##### b、创建子应用加载失败组件

src/components/component-error-fallback/index.tsx

```tsx
const ComponentErrorFallback = ({ error }: { error: Error }) => (
  <div style={{ padding: '20px', border: '1px solid #ff6b6b', borderRadius: '8px' }}>
    <h3>远程应用加载失败</h3>
    <p>错误详情: {error.message}</p>
    <button onClick={() => window.location.reload()}>
      重新加载页面
    </button>
  </div>
);

export default ComponentErrorFallback;
```

##### c、基础配置

rsbuild.config.ts

```ts
import { pluginModuleFederation } from '@module-federation/rsbuild-plugin';
import { pluginReact } from '@rsbuild/plugin-react';
import { defineConfig } from '@rsbuild/core';

export default defineConfig({
  plugins: [
    pluginReact(),
    pluginModuleFederation({
      name: '导入时的唯一名，全局唯一',
      remotes: {
        // remote1 子应用导出时的名字
        // http://localhost:3002 子应用地址
        // /remoteEntry.js 是子应用中配置
        remote1: 'remote1@http://localhost:3002/remoteEntry.js',
      },
      bridge: {
        // 是否开启导航
        enableBridgeRouter: true,
      },
    }),
  ],
});
```

##### d、子应用渲染插件

/src/remotes/remote-app/indextsx

```tsx
import { createRemoteAppComponent } from '@module-federation/bridge-react';
import { loadRemote } from '@module-federation/runtime';
import { ComponentLoading, ComponentErrorFallback } from '@module-federation/runtime';

export const Remote1App = createRemoteAppComponent({
  // remote1 是子应用导出时的名字，export-app 是具体的 key
  loader: () => loadRemote('remote1/export-app'),
  loading: <ComponentLoading />,
  fallback: ComponentErrorFallback,
});

```

##### d、渲染子应用

```tsx
import { Remote1App } from './remotes/Remote1App';
import { BrowserRouter, Route, Routes } from 'react-router';

const App = () => {
  return (
    <div className="">
      <BrowserRouter>
        <Routes>
          {/* 路由配置，推荐，当然也可以 path="/*" */}
          <Route
            path="/remote1/*"
            // 使用 Remote1App 组件, 将会被懒加载
            Component={() => (
              <Remote1App />
            )}
          />
        </Routes>
      </BrowserRouter>
    </div>
  );
};

export default App;
```

### 3、部分组件使用

[docs](https://module-federation.io/zh/practice/frameworks/react/index.html)

#### 1）子应用

##### a、更新入口文件

bootstrap.tsx

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const rootEl = document.getElementById('root');
if (rootEl) {
  const root = ReactDOM.createRoot(rootEl);
  root.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>,
  );
}
```

index.tsx

```tsx
import('./bootstrap');
```

##### b、安装插件

```bash
npm i @module-federation/enhanced
npm i @module-federation/rsbuild-plugin --save-dev
```

##### c、新增一个按钮

demo-02/src/components/button/index.tsx

```tsx
const Button = (props?: {
  onClick?: () => void;
  children?: React.ReactNode;
}) => (
  <button onClick={props?.onClick}>{props?.children}</button>
);

export default Button;
```

##### d、导出

/module-federation.config.ts

```ts
import { createModuleFederationConfig } from '@module-federation/rsbuild-plugin';

export default createModuleFederationConfig({
  name: 'remote',
  exposes: {
    './Button': './src/components/button/index.tsx',
  },
  filename: 'remoteEntry.js',
  shared: {
    react: {
      singleton: true,
    },
    'react-dom': {
      singleton: true,
    },
  },
});
```

##### e、集成到打包

rsbuild.config.ts

```ts
import { defineConfig } from '@rsbuild/core';
import { pluginReact } from '@rsbuild/plugin-react';

import { pluginModuleFederation } from '@module-federation/rsbuild-plugin';
import mfConfig from './module-federation.config';

// Docs: https://rsbuild.rs/config/
export default defineConfig({
  server: {
    port: 3001
  },
  plugins: [
    pluginReact(), 
    pluginModuleFederation(mfConfig)
  ],
});
```

#### 2）父应用

##### a、下载依赖

```bash
npm install @module-federation/rsbuild-plugin
```

##### b、进程子应用模块的类型文件

/src/remotes.d.ts

```ts
declare module 'remote/Button' {
  import Button from 'remote/Button';
  export default Button;
}
```

##### c、导入子应用

rsbuild.config.ts

```ts
// rsbuild.config.ts
import { pluginModuleFederation } from '@module-federation/rsbuild-plugin';
import { pluginReact } from '@rsbuild/plugin-react';
import { defineConfig } from '@rsbuild/core';

export default defineConfig({
  plugins: [
    pluginReact(),
    pluginModuleFederation({
      name: 'test-01',
      remotes: {
        // 避免别名前缀冲突，改成与 remote1 不相关的前缀
        remote: 'remote@http://localhost:3001/remoteEntry.js',
      },
      bridge: {
        enableBridgeRouter: true,
      },
    }),
  ],
});
```

##### d、使用

```tsx
import Button from 'remote/Button';

export default function Demo02() {
  return <div>
    <Button>按钮</Button>
  </div>;
}
```

### 4、处理父子应用公用一个 UI 库时样式丢失问题

[docs](https://module-federation.io/zh/configure/shared.html#singleton)

#### 1）子应用

rsbuild.config.ts

```ts
import { defineConfig } from '@rsbuild/core';
import { pluginReact } from '@rsbuild/plugin-react';
import { pluginModuleFederation } from '@module-federation/rsbuild-plugin';

// Docs: https://rsbuild.rs/config/
export default defineConfig({
  server: {
    port: 3002
  },
  plugins: [
    pluginReact(),
    pluginModuleFederation({
      name: 'remote1',
      filename: 'remoteEntry.js',
      exposes: {
        './export-app': './src/export-app.tsx', // 导出应用类型远程模块
      },
      shared: {
        react: { singleton: true },
        'react-router': { singleton: true },
        antd: { singleton: true },
        '@ant-design/cssinjs': { singleton: true },
      },
      bridge: {
        // 启用 Bridge Router 路由能力，默认为 true
        enableBridgeRouter: true, 
      }
    }),
  ],
});
```

#### 2）父应用

rsbuild.config.ts

```ts
// rsbuild.config.ts
import { pluginModuleFederation } from '@module-federation/rsbuild-plugin';
import { pluginReact } from '@rsbuild/plugin-react';
import { defineConfig } from '@rsbuild/core';

export default defineConfig({
  plugins: [
    pluginReact(),
    pluginModuleFederation({
      name: 'test-01',
      remotes: {
        remote1: 'remote1@http://localhost:3002/remoteEntry.js'
      },
      bridge: {
        enableBridgeRouter: true,
      },
      shared: {
        react: { singleton: true },
        'react-router': { singleton: true },
        antd: { singleton: true },
        '@ant-design/cssinjs': { singleton: true },
      },
    })
  ],
});
```

<div style="color:red;">注：<br />1、保持相同依赖为单例模式；<br /> 2、一定要确保 主依赖为单例模式。</div>

