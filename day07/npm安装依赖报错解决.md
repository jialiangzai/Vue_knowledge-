## npm安装依赖报错解决

> 解决办法

* 方案1执行：

```bash
# 检查代理设置
npm config get proxy
npm config get https-proxy
# 如果返回值不为null，继续执行=>
# 这一步很重要，一定要保证两个命令的返回值都为null
npm config set proxy null
npm config set https-proxy null
```

* 方案2执行：

```bash
# 使用cnpm 安装依赖
npm config set registry  https://registry.npm.taobao.org/  #设置淘宝镜像地址
npm config get registry  #查看镜像地址

# 安装cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org
```



最后：

```bash
# 测试
npm i dayjs
```

