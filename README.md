# Feature PR Flow  

面向多环境，多特性并行开发的 Git 分支管理及自动化 CI/CD 部署流程。

## 规则

* 组名: `GROUP_NAME`
* 服务名: `SERVICE_NAME`
* 特性名: `FEATURE_NAME`
* 部署环境: `ENV`（默认为 demo, 当为 demo 时，也将省略）

```
// Git Repo 命名规则
<GIT_REPO>/<GROUP_NAME>/<SERVICE_NAME>

// k8s namespace
<GROUP_NAME>[--<ENV>]

// k8s Service / Deployment 名称
<SERVICE_NAME>[--<FEATURE_NAME>]

// 如果提供服务，访问域名为（域名规则略诡异，因为好买证书）
https?://<SERVICE_NAME>[--<FEATURE_NAME>]__<GROUP_NAME>[--<ENV>].<BASE_HOSTNAME>
```

* `master` 为稳定分支，只有当代码通过测试和 Code Review 后方可合并进入，对应环境为 `demo`。
    * `develop` 为开发分支，对应环境为 `staging`。 
    * `feat/<FEATURE_NAME>` 为特性 branch/tag，当需求敲定，开始开发时，由 `develop` 分支切出，进行开发，该分支的对应环境为 `staging`、
* 部署到 TEST 或 DEMO 环境，只需打上带 环境名后缀的 tag，如
    * `feat/<FEATURE_NAME>.test`
    * `feat/<FEATURE_NAME>.demo`
* 服务将自动注册，并可以通过上面访问域名的规则进行访问
* 开发过程中，如 `master` 有更新，需要 merge/rebase 相关更新
* feature 之间若有关联性，需要预先合并关联 `feature`，并测试
* feature 开发不用一开始就命名 `feat/<FEATURE_NAME>` 分支，当需要集成部署才打上上面的 tag 即可。