# wecode-vue-template

wecode-vue-template是基于 vue-cli3.0 制作的We码开发模板，符合 WLK-CLI 集成规范，允许开发者自由定制。

## 项目目录结构

```shell
|—— build
│   ├── common                         // JS API公共文件目录
          ├── /html/                   // html文件
          ├── /js/                     // JS API脚本文件
          ├── /css/                    // 公共组件库样式文件
          ├── /assets/                 // 资源目录
               ├── /images/            // 公共图片资源
               ├── /media/             // 公共媒体资源
│   └── apps                           // 开发的We码应用包文件目录
          ├── /local/        // 应用名称
               ├── /0/        // 应用的构建环境。打包时，cli 会打包该文件夹下的文件
                    ├── /html/         // 应用的html文件
                    ├── /js/           // 应用的js文件
                    ├── /css/          // 应用的css样式文件
├── public                             // 静态资源目录，内容会直接拷贝到打包目录中
├── src                                // 源代码目录
│   ├── common                         // 公共资源，包括 css/js  等
│   ├── components                     // 组件
│   ├── config                         // 配置项，包括 api 等
│   ├── i18n                           // 国际化
│       ├── en_US                      // 英文配置信息
│       └── zh_CN                      // 中文配置信息
│   ├── router                         // 路由
│   ├── store                          // store，页面数据按模块存储
│   ├── utils                          // 提供一些小工具
│   ├── views                          // 页面
│   ├── App.vue                        // 主页
│   ├── index.html                     // 主页 html
│   ├── main.js                        // webpack打包入口
├── test                               // jest测试集
└── vue.config.js                      // vue-cli3.0 配置文件
```
