# auto-check-in Releases

本仓库承载 `auto-check-in` 的公开发布自动化，不包含私有源码。

## 发布流程

- `sync-private-tags` 每 5 分钟读取私有源码仓库的版本 Tag，也支持手动指定 Tag。
- `docker-image` 检出指定的私有 Tag，构建 `linux/amd64`、`linux/arm64` 镜像并发布到 GHCR。
- 构建成功后在本仓库创建同名 GitHub Release，作为已发布标记并记录源码提交、镜像摘要。

私有源码仅在受信任的 GitHub-hosted runner 中临时检出，不会写入本公开仓库、公共缓存或公开构建产物。工作流不启用 GitHub Actions/Docker 构建缓存，也不生成 SBOM 或来源证明。Python 应用源码仅存在于仍为私有的 GHCR 镜像层中。

## 镜像

```text
ghcr.io/davidzhang12138/auto-check-in:<version>
ghcr.io/davidzhang12138/auto-check-in:latest
```

该镜像包继续保持私有。部署机器需要登录 GHCR 并具有包读取权限：

```bash
echo "$CR_PAT" | docker login ghcr.io -u <github 用户名> --password-stdin
```

首个自动同步版本为 `v0.1.1`。
