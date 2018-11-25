# npm 的使用问题

- 用npm安装组件时可能会返回下列警告：

```bash
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@^1.0.0 (node_modules\chokidar\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.1.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
```

- 原因是因为：fsevent是mac osx系统的，在win或者Linux下使用了 所以会有警告，忽略即可。意思就是你已经安装成功，可以使用了。

- 将 npm 的注册表源设置为国内的镜像，可以大幅提升安装速度。
- 在终端中输入：

```bash
npm config set registry https://registry.npm.taobao.org
// 配置后可通过下面方式来验证是否成功
npm config get registry
// 或
npm info express
```

- 或通过cnpm使用

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org

// 使用
cnpm install express
```