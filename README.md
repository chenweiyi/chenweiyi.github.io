## 简介

这是一个程序员的世界，记录一下学习和生活，希望可以坚持下去，写到退休...

### 文档使用

#### 启动项目

1. 切换项目分支 `git checkout hexo`
2. 进入项目文件夹 `cd cwy`
3. 安装依赖 `npm i`
4. 启动项目 `hexo server`, 需全局安装*hexo* `npm i hexo -g`

#### 写稿发布流程

1. 切换项目分支 `git checkout hexo`
2. 进入项目文件夹 `cd cwy`
3. 创建新文件 `hexo new [layout] 文件名`
4. 产生发布文件 `hexo generate`
5. 部署文件 `hexo deploy`, 以上两个命令可以一起写 `hexo generate --deploy` 或者 `hexo g -d`

#### 注意事项

1. 执行 `hexo generate` 产生的 index.html 为空文件?
   > 可能是 node 版本和 hexo 版本不匹配，现在用的 hexo 是 3.x 的版本，比较旧，对应 node 版本需要 12 以下
