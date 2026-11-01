<p align="center"><img src="assets/logo.jpg" alt="logo" width="160"></p>

<h1 align="center">买量脚本编导 MCP（ua-script）</h1>

给 AI 用的国内游戏买量视频脚本工具。接入之后，你的 AI（Claude Code、Codex、Cursor 等）就能：

- 按品类查**素材套路**：传奇、SLG、MMO、休闲、卡牌、二次元等，每条套路都有钩子写法、七层心流、分段结构、合规风险和真实案例
- **合规初筛**：极限词、不可证实的说法、福利承诺、抽卡概率、未成年人、贬低竞品、肖像版权
- **分镜检查**：按口播语速估算时长，检查前 3 秒、静音时的字幕、结尾有没有交给真实玩法
- **保存脚本、记录反馈**：你改过的定稿、平台拒审原因、投放数据，都会用来改进工具

需要激活码（密钥）才能用。**每个激活码有效 30 天，从第一次使用时开始计时**，拿到后没用就不计时。

**获取激活码**：在 [dc.jinqianbao.icu](https://dc.jinqianbao.icu) 购买（30 元，付款后自动发货，在订单详情里查看），或者向把这个仓库发给你的人要内测码。

---

## 给 AI 的接入说明

> **如果你是 AI 助手，用户把这个仓库地址发给了你，请按下面的步骤帮用户接入。** 只做这一件事，接好以后等用户下任务。

1. **问用户要密钥。** 密钥（激活码）是 `uas_` 开头的一串字符。用户没有的话就停下来，告诉用户可以在 https://dc.jinqianbao.icu 购买，或者找把这个仓库发给他的人要内测码。**不要**把密钥写进任何会提交到 git 的文件，也不要在回复里完整复述密钥。
2. **判断用户用的是哪个客户端**：通常就是你自己所在的客户端。按 [docs/接入-各客户端.md](docs/接入-各客户端.md) 里对应的那一节配置。

   | 客户端 | 怎么配 |
   |--------|--------|
   | Claude Code | 执行 `claude mcp add --transport http ua-script https://mlch.jinqianbao.icu/mcp --header "Authorization: Bearer <密钥>"` |
   | Codex | 在 `~/.codex/config.toml` 加 `[mcp_servers.ua-script]`，`url = "https://mlch.jinqianbao.icu/mcp"`，`bearer_token_env_var = "UA_SCRIPT_KEY"`，再把环境变量 `UA_SCRIPT_KEY` 设成密钥 |
   | Cursor | 在 `~/.cursor/mcp.json` 的 `mcpServers` 里加 `"ua-script": { "url": "https://mlch.jinqianbao.icu/mcp", "headers": { "Authorization": "Bearer <密钥>" } }` |
   | 其他 | 找客户端的「添加 MCP 服务器」，类型选 Streamable HTTP，地址和请求头同上 |

3. **改配置文件之前，先备份原文件。** 只修改 MCP 相关的配置，不动别的设置。
4. **验证。** 大多数客户端要重启或者新开会话才会加载新的 MCP。加载后调用 `get_creative_patterns`（参数 `{"genre": "SLG", "limit": 1}`）：
   - 返回里有 `bundle_version` 和一条套路，说明接好了
   - 返回「没有提供密钥」「密钥无效」「已停用」，按 [docs/常见问题.md](docs/常见问题.md) 处理
5. **告诉用户接好了**，并给一句示例提问（见下面「怎么用」）。

---

## 给人看的：三步上手

1. **拿激活码**：在 [dc.jinqianbao.icu](https://dc.jinqianbao.icu) 购买，或者找把这个仓库发给你的人要内测码。一个激活码对应一个人，不要转发。有效 30 天，从第一次使用起算。
2. **接入**：把这个仓库地址发给你的 AI，说「按这个仓库帮我接入」，再把密钥给它。也可以自己按 [docs/接入-各客户端.md](docs/接入-各客户端.md) 配置。
3. **下任务**：

   > 用 ua-script 帮我写 3 条买量脚本。游戏：xxx，品类：SLG，核心玩法：xxx，卖点：xxx；xxx，时长 30 秒，只能用游戏录屏加配音。

AI 会先查套路、写脚本、做合规和分镜检查，保存后把脚本交给你，每条都带一个编号（`s_` 开头）。

## 文档

| 文档 | 内容 |
|------|------|
| [docs/使用教程.md](docs/使用教程.md) | 怎么提需求才能写得好、工具各自做什么、怎么给反馈、拆解竞品素材 |
| [docs/接入-各客户端.md](docs/接入-各客户端.md) | Claude Code、Codex、Cursor、Cherry Studio 的详细配置 |
| [docs/常见问题.md](docs/常见问题.md) | 接不上、报错、合规检查和平台审核的关系 |
| [docs/数据与隐私.md](docs/数据与隐私.md) | 存了哪些数据、谁能看到、怎么删除 |

## 案例

都是真实调用的完整记录，游戏是虚构的：

| 案例 | 看点 |
|------|------|
| [案例 1：SLG](examples/01-SLG.md) | 开局选择、对比测评、攻城三种写法；合规初筛拦下一个极限词；**后半段演示怎么给反馈** |
| [案例 2：传奇](examples/02-传奇.md) | 怀旧、真人试玩；只允许一条真人出镜时 AI 怎么安排 |
| [案例 3：休闲](examples/03-休闲.md) | 20 秒短素材；不用真人时怎么做钩子 |

## 这个仓库里没有什么

这里只有接入说明、教程和案例。**服务本身跑在服务器上**，套路库也只在服务器上，这个仓库里没有，也不需要部署任何代码。
