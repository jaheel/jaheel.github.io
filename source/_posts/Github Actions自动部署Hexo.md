# Github Actions自动部署Hexo

## 用处

用github actions自动部署hexo后，可不用在本地生成静态网页后再上传。只需要上传文档即可触发自动部署。



## Hexo

确认本地Hexo能够正常运行

```bash
hexo clean
hexo g
hexo d
```



确认_config.yml文件有github pages配置：

```yaml
deploy:
	type: git
	repository: git@github.com:[username]/[repository].git
	branch: master
```



## 生成密钥

该密钥公钥只用于Hexo的部署，更安全。

```bash
ssh-keygen -t rsa -b 4096 -C "Hexo Deploy Key" -f github-deploy-key -N ""
```

在当前目录生成两个文件：

* github-deploy-key —— 私钥
* github-deploy-key.pub —— 公钥



## 配置密钥

私钥：存放在Hexo原始文件的代码仓库里面，用于触发Actions时使用。（Hexo仓库 Settings -> Secrets 添加私钥）

公钥：放在Github Pages对应的代码仓库里面，用于Hexo部署时的写入工作。（Hexo仓库Settings -> Deploy Keys添加公钥）



## 创建Workflow

Hexo仓库新建一个文件： .github/workfows/deploy.yml（随便取，但一定放在.github/workflows目录中）

内容：

```yaml
name: Hexo Deploy

on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-18.04
    if: github.event.repository.owner.id == github.event.sender.id

    steps:
      - name: Checkout source
        uses: actions/checkout@v2
        with:
          ref: master

      - name: Setup Node.js
        uses: actions/setup-node@v1
        with:
          node-version: '12'

      - name: Setup Hexo
        env:
          ACTION_DEPLOY_KEY: ${{ secrets.HEXO_DEPLOY_KEY }}
        run: |
          mkdir -p ~/.ssh/
          echo "$ACTION_DEPLOY_KEY" > ~/.ssh/id_rsa
          chmod 700 ~/.ssh
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan github.com >> ~/.ssh/known_hosts
          git config --global user.email "xxx@xxx.com"
          git config --global user.name "xxx"
          npm install hexo-cli -g
          npm install

      - name: Deploy
        run: |
          hexo clean
          hexo g
          hexo deploy
```



PS: 

1. secrets.HEXO_DEPLOY_KEY就是我们之前设置的私钥，名字不能搞错