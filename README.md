## 创建 react+typescript 项目

```bash
## 安装脚手架
npm install create-react-app -g
## 创建项目名为my-project的项目
create-react-app my-project --scripts-version=react-scripts-ts

## 启动项目
cd my-project
yarn start
```

查看是否运行: [localhost:3000](http://localhost:3000)

## 加入 antd 引用

```bash
yarn add antd
## 注意: 这里暂时不要安装react-app-rewired 2.0版本的, 后续命令行会出错 这里安装的是1.6.2版本的
yarn add ts-import-plugin react-app-rewired@1.6.2 react-app-rewire-less --dev
```

## 配置修改

1. 在根目录下添加 config-overrides.js 文件

   ```js
   const tsImportPluginFactory = require('ts-import-plugin');
   const { getLoader } = require('react-app-rewired');
   const rewireLess = require('react-app-rewire-less');

   module.exports = function override(config, env) {
     const tsLoader = getLoader(
       config.module.rules,
       rule => rule.loader && typeof rule.loader === 'string' && rule.loader.includes('ts-loader')
     );

     tsLoader.options = {
       getCustomTransformers: () => ({
         before: [
           tsImportPluginFactory({
             libraryDirectory: 'es',
             libraryName: 'antd',
             style: true,
           }),
         ],
       }),
     };

     config = rewireLess.withLoaderOptions({
       javascriptEnabled: true,
       modifyVars: {
         '@primary-color': '#1DA57A', // 主题色
       },
     })(config, env);

     return config;
   };
   ```

2. 接下来可以使用 antd 了

   ```tsx
   import { Button } from 'antd';

   class App extends React.Component {
     public render() {
       return (
         <div className="App">
           <header className="App-header">
             <img src={logo} className="App-logo" alt="logo" />
             <h1 className="App-title">Welcome to React</h1>
           </header>
           <p className="App-intro">
             <Button type="primary">Button</Button>
           </p>
         </div>
       );
     }
   }
   ```
