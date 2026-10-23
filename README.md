# merran-lzcapp

[Merran](https://github.com/merran-app/merran) 的 LazyCat LPK 打包 —— 自托管家庭膳食规划（食谱 / 周计划 / 购物清单 / 储藏）。

## 形态

- 单容器镜像：`ghcr.io/merran-app/merran`，端口 3000，SQLite 存储（数据在 /data）
- 镜像交付：**direct + digest 固定**（mutable next tag 的 mirror 校验不可用，同 Nostos）
- 版本策略：`channel: custom` + `sort: created` + `tag_regex: ^next$` + `bump: patch`
  digest 变化才递增 patch，未变化则 no-op
- 仅喵喵商店；仅 PC 平台

## 首次使用

Merran 首次访问需要**日志中的 setup code** 创建账号与家庭：

```bash
docker logs merran | grep -i code   # 或在懒猫应用日志里查看
```

这是上游的动态安全设计，无法预置凭据。

## 持久化

`/lzcapp/var/data` → `/data`（数据库、照片、server secret —— 更新前务必备份）

## 可选服务（上游 compose profiles，本包未包含）

- Postgres（默认 SQLite 足够）
- Kokoro 语音（读菜谱步骤，需 1-2 GB 内存）
- Cloudflare Tunnel
