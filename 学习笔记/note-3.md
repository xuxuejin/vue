## vue-cli打包添加版本号

1.在项目根目录下新建三个文件（关于文件名：必须以如下方式命名）

  .env 全局默认配置文件，不论什么环境都会加载合并

  .env.development 开发环境下的配置文件

  .env.production 生产环境下的配置文件
  
根据启动命令 vue 会自动加载对应的环境，vue 是根据文件名进行加载的，比如执行 npm run serve 命令，会自动加载 .env.development 文件

2.修改.env.* （属性名必须以VUE_APP_开头）

  .env.development 文件
  VUE_APP_VERSION = T0.01

  .env.production
  VUE_APP_VERSION = V0.01

3.修改打包脚本

  "build:development": "cross-env NODE_ENV=development vue-cli-service build",
  "build:production": "cross-env NODE_ENV=production vue-cli-service build",
  
4.修改 vue.config.js 文件

const Timestamp = new Date().getTime();

module.exports = {
    configureWebpack: {
        output: {
            filename: `[name].${process.env.VUE_APP_VERSION}.${Timestamp}.js`,
            chunkFilename: `[name].${process.env.VUE_APP_VERSION}.${Timestamp}.js`
        },
    }
}

通过版本号+时间戳的方式给静态资源添加版本号



