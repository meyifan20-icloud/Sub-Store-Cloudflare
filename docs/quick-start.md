# 五分钟快速开始

目标：从一个空的 Cloudflare 账号部署 Sub-Store Cloudflare，创建第一个组合订阅，并复制到客户端。

## 准备

你需要：

- Cloudflare 账号；
- GitHub 或 GitLab 账号；
- 一个管理员 Token；
- 至少一个远程订阅 URL，或者一段本地节点文本。

这个项目按 Workers Free 使用边界设计，但 Cloudflare 账号的实际用量和计费状态仍以你的控制台为准。

## 第一步：生成管理员 Token

使用密码管理器生成一个管理员口令，或运行：

```bash
pnpm run token:generate
```

它只用于 `SUB_STORE_ADMIN_TOKEN`。订阅下载不设置全局总钥匙；每次复制 Source / Collection 订阅链接时，后台会自动在 D1 创建独立的 Scoped Download Grant。

## 第二步：部署到 Cloudflare

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/meyifan20-icloud/Sub-Store-Cloudflare)

在 Cloudflare 部署页确认：

1. 项目名称可以使用默认的 `sub-store-cloudflare`。
2. D1 选择自动创建的新数据库；只有升级已有部署时才选择原数据库。
3. `SUB_STORE_ADMIN_TOKEN` 填管理 Token。
4. 构建命令是 `pnpm run build`。
5. 部署命令是 `pnpm run deploy`。

Secret 输入框不应该出现可直接使用的公开默认值。如果看到 `replace-with-...` 一类固定字符串，请不要部署，并到仓库 issue 反馈。

点击部署后，Cloudflare 会自动 provision D1、运行 migrations、构建前端并部署 Worker。

## 第三步：进入管理页面

部署成功后，Cloudflare 会显示 Worker 地址，例如：

```text
https://sub-store-cloudflare.<你的子域>.workers.dev
```

第一次进入使用：

```text
https://sub-store-cloudflare.<你的子域>.workers.dev/?token=<admin-token>
```

前端读取 Token 后会把它从地址栏移除，并保存在当前浏览器中用于后续管理请求。

如果页面显示“数据加载失败”，输入 `SUB_STORE_ADMIN_TOKEN`。

## 第四步：添加第一个 Source

在首次使用卡片点击“添加第一个订阅源”。

远程订阅：

1. ID 使用小写字母、数字、下划线或连字符，例如 `airport-a`。
2. 类型选择远程订阅。
3. 粘贴订阅 URL。
4. 可以先加入“清理信息节点”操作。
5. 保存后回到订阅列表。

本地节点：

1. 类型选择本地内容。
2. 粘贴 `vless://`、`trojan://`、`ss://`、Mihomo YAML、JSON/JSON5 或其他支持内容。
3. 保存并确认预览能识别节点。

## 第五步：确认默认 Daily Collection

Migration 会预置一个包含全部已启用 Source 的 `Daily` Collection。添加 Source 后可以直接编辑它：

1. 不选择指定 Source 时，默认包含全部已启用 Source。
2. 模板推荐 `acl4ssr-mihomo`。
3. 建议先启用端点去重和名称排序。
4. 保存。

只有需要不同来源组合或不同模板时，才新建其他 Collection。

区域 include 过滤器会删除其他地区节点，第一次使用不要急着添加。

## 第六步：复制客户端链接

在 Collection 卡片打开快捷操作，选择需要的目标：

- Mihomo / Clash Meta：`mihomo`
- sing-box：`sing-box`
- Surge：`surge` 或 `surge-mac`
- Loon：`loon`
- Quantumult X：`qx`
- 通用 URI：`uri`

点击复制链接时会自动创建只对应当前 Source / Collection（以及所选输出格式）的 Scoped Download Grant。Admin Token 不会进入客户端订阅链接。

## 最后做两件事

1. 到“设置”页面导出一次完整配置备份。
2. 阅读 [升级指南](upgrading.md)，了解 Deploy Button 仓库副本如何获取新版本。

遇到问题先看 [故障排查](troubleshooting.md)。
