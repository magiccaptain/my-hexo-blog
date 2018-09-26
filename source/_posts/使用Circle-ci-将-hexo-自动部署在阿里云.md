---
title: 使用Circle ci 将 hexo 自动部署在阿里云
date: 2018-09-20 15:43:52
tags: CI
---

# 前言

如果将hexo博客部署在云主机上,每次写完日志时都要 generate，deploy，这样比较麻烦，通过github和 circle可以完美实现自动化。

自动化部署的流程如下：

1. 将 hexo 提交到github
2. circle ci 自动执行 hexo generate 并将生成的 public 打包成docker，使用docker打包可以很方便的迁移博客。
3. 阿里云收到webhook 事件，完成docker的部署

# 配置 circle ci

```yaml
# Javascript Node CircleCI 2.0 configuration file
#
# Check https://circleci.com/docs/2.0/language-javascript/ for more details
#
version: 2

defaults: &defaults
  working_directory: ~/repo
  docker:
    - image: circleci/node:latest

jobs:
  build:
    <<: *defaults
    steps:
      - checkout

      # Download and cache dependencies
      - restore_cache:
          keys:
          - v1-dependencies-{{ checksum "package.json" }}
          # fallback to using the latest cache if no exact match is found
          - v1-dependencies-

      - run: yarn install

      - save_cache:
          paths:
            - node_modules
          key: v1-dependencies-{{ checksum "package.json" }}

      - run: yarn generate
      - persist_to_workspace:
          root: ~/repo
          paths: .

  release:
    <<: *defaults

    steps:
      - attach_workspace:
          at: ~/repo
      - setup_remote_docker
      - run:
          name: chmod sh
          command: chmod +x bin/build-docker.sh
      - run:
          name: build docker
          command: bin/build-docker.sh latest $DOCKER_USER $DOCKER_PASS $DOCKER_REGISTRY $DOCKER_REPO

workflows:
  version: 2
  build-deploy:
    jobs:
      - build:
          filters:
            tags:
              only: /.*/
      - release:
          requires:
            - build
```

# 阿里云服务设置

## 配置 docker 自动更新脚本

在每次博客更新时，构建好的镜像都会更新到 docker hub（阿里）上，因此，在我们的博客服务器上，需要对镜像进行更新，同时重新建立容器。







