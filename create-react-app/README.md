# [create-react-app](https://create-react-app.dev/docs/getting-started)

## Adding TypeScript and Alias

[How to use TypeScript with React 18 alpha](https://blog.logrocket.com/how-to-use-typescript-with-react-18-alpha/)

To add TypeScript to an existing Create React App project:

1. install it: `npm install --save typescript @types/node @types/react @types/react-dom @types/jest`
2. rename any file to be a TypeScript file, restart development server
3. add `tsconfig.json` to the root of the project
   ```json
   {
     "compilerOptions": {
       "target": "es5",
       "lib": ["dom", "dom.iterable", "esnext"],
       "allowJs": true,
       "skipLibCheck": true,
       "esModuleInterop": true,
       "allowSyntheticDefaultImports": true,
       "strict": true,
       "forceConsistentCasingInFileNames": true,
       "noFallthroughCasesInSwitch": true,
       "module": "esnext",
       "moduleResolution": "node",
       "resolveJsonModule": true,
       "isolatedModules": true,
       "noEmit": true,
       "jsx": "react-jsx"
     },
     "include": ["src"]
   }
   ```
4. [configure a path alias](https://plusreturn.com/blog/how-to-configure-a-path-alias-in-a-react-typescript-app-for-cleaner-imports/)
   1. modify tsconfig
      ```json
      {
        "compilerOptions": {
          "target": "es5",
          "lib": ["dom", "dom.iterable", "esnext"],
          "allowJs": true,
          "skipLibCheck": true,
          "esModuleInterop": true,
          "allowSyntheticDefaultImports": true,
          "strict": true,
          "forceConsistentCasingInFileNames": true,
          "noFallthroughCasesInSwitch": true,
          "module": "esnext",
          "moduleResolution": "node",
          "resolveJsonModule": true,
          "isolatedModules": true,
          "noEmit": true,
          "jsx": "react-jsx"
        },
        "include": ["src"],
        "extends": "./tsconfig.paths.json"
      }
      ```
   2. add tsconfig.path.json
      ```json
      {
        "compilerOptions": {
          "baseUrl": ".",
          "paths": {
            "@/*": ["./src/*"]
          }
        }
      }
      ```
   3. modify webpack configuration craco.config.js by CRACO
      ```js
      const path = require("path");
      module.exports = {
        webpack: {
          alias: {
            "@": path.resolve(__dirname, "src"),
          },
        },
      };
      ```
   4. ts 无法编译图片等资源：[Importing images in TypeScript React - "Cannot find module"](https://stackoverflow.com/questions/52759220/importing-images-in-typescript-react-cannot-find-module)，[typescript 项目中 import 图片时报错：TS2307: Cannot find module ‘...’](https://www.cnblogs.com/chen-cong/p/10445635.html)
   5. Why are we not putting this configuation in tsconfig.json right away instead? this is because on startup, react (and later, craco) cleans up tsconfig.json to only keep the configuration that it expects to be there for it to work properly. paths property is one of those that it removes when it starts up.

## Adding Router

1. `npm install --save react-router-dom`
2. [官方示例](https://v5.reactrouter.com/web/example/basic)，注意版本号
3. ts 支持 `npm i --save-dev @types/react-router-dom`

## Build

use CRACO to change the webpack configuration, which will change the output name from build to dist:

```js
const path = require("path");
module.exports = {
  webpack: {
    alias: {
      "@": path.resolve(__dirname, "src"),
    },
    configure: (webpackConfig, { env, paths }) => {
      paths.appBuild = "dist";
      webpackConfig.output = {
        ...webpackConfig.output,
        path: path.resolve(__dirname, "dist"),
        publicPath: "/",
      };
      return webpackConfig;
    },
  },
};
```
