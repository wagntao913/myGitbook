# 第一章 gitbook 使用
#### gitbook安装
> 环境要求： 安装 node , npm， git

- 命令行运行 `npm install gitbook-cli -g`
- 安装完成后运行 `gitbook -V` 查看是是否安装成功

#### 创建自己的gitbook
- 在指定的位置创建文件夹，右键鼠标选择 *Git Bash Here*,打开git窗口  

- 在git窗口内，执行 `gitbook init`,初始化项目,会创建 `README.md` 和 `SUMMARY.md` 这两个文件  

- 我们输入 `gitbook serve` 命令，然后在浏览器地址栏中输入 `http://localhost:4000` 便可预览书籍

#### 结合 GitHub Pages 使用 GitBook
- GitHub Pages 是 GitHub 提供的静态网站托管服务。
  GitHub 上的每个仓库都可以拥有一个 GitHub Pages，对应的 URL 如下 `https://<username>.github.io/<repository>/`
- GitHub Pages 的静态资源支持下面 3 个来源：
  - `master` 分支
  - `master` 分支的 `/docs` 目录
  - `gh-pages` 分支
- 执行下面命令，将 `_book` 目录推送到 GitHub 仓库的 `gh-pages` 分支。
`git subtree push --prefix=_book origin gh-pages`
- 或者在生成静态网页时，将保存的目录指定为 `./docs`
`gitbook build ./ ./docs`
然后直接推送到 GitHub 仓库的。
` git push origin master`
