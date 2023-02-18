## 高德底图添加 WMS 标准图层

### 开发准备

- 注册账户，官网地址 https://lbs.amap.com/

- `应用管理` => `创建新应用` => 设置应用，注意`服务平台`项勾选`Web端（JS API）`，即可获得 key 和安全密钥。

- 按照项目组件`Map.vue`中的写法，把 key 和密钥填入。

### 快速上手

- 终端进入项目根目录，输入命令`yarn && yarn dev`（本机 node 版本 v19.4.0）,启动项目。

- 参考代码注释在地图中加入自己的图层。