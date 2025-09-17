---
title: 隆重发布服务器自用Velocity聊天插件Horde!
published: 2025-09-15
description: '一款轻量、零折腾的 Velocity 聊天插件，支持全局/本地聊天、模板与权限前缀，以MIT协议开源。'
image: 'https://mc-img.teamhyena.org/hordeicon.webp'
tags: ['公告', '插件']
category: '公告'
draft: false 
lang: 'zh_CN'
pinned: false
---
一款轻量、零折腾的 Velocity 聊天插件 —— Horde。

Horde 提供全局与本地聊天、简洁可配的模板、基于权限的前缀，并尽量减少复杂度。下面是插件的概览、配置示例与用法说明。

## 功能

- 全局与本地聊天：可在整个代理广播，或仅限当前子服本地。
- 玩家默认模式：使用不带参数的 `/broadcast` 或 `/local` 在个人默认全局/本地之间切换。
- 极简配置：开箱即用的合理默认，同时保持高度可定制。
- 灵活格式：本地化、模板和聊天文本支持 MiniMessage 或传统颜色码（Legacy §/&）。
- 基于权限的前缀：通过 `prefix.yml` 在聊天模板中注入前缀。
- 名称映射：为服务器名展示易读的别名（如 `survival` -> `Survival`）。
- 可选的发送者 UUID 转发与聊天日志功能以提升兼容性与审计能力。

<img title="该插件实现的一种聊天样式" src="https://mc-img.teamhyena.org/hordetest1.webp" alt="">
<img title="该插件实现的另一种聊天样式" src="https://mc-img.teamhyena.org/hordetest2.webp" alt="">

## 环境要求

- Velocity 代理 3.x
- Java 21 及以上

## 安装

1. 构建或下载插件 JAR。
2. 将 JAR 放入 Velocity 的 `plugins/` 目录。
3. 启动一次 Velocity 以在 `plugins/horde/` 生成配置文件。
4. 按需编辑 `config.yml`、`localization.yml` 与 `prefix.yml`。
5. 使用 `/horde reload` 重载或重启代理。

## 配置速览

所有文件位于 `plugins/horde/`。

`config.yml`（要点）：

- `default-global-chat`（布尔，默认 `true`）：玩家默认是否为全局聊天。
- `log-messages`（布尔，默认 `false`）：是否将聊天镜像到代理日志。
- `enable-legacy-chat-formatting`（布尔，默认 `true`）：是否允许玩家在原始消息中使用 `&` 颜色码（Legacy）。
- `enable-legacy-localization-formatting`（布尔，默认 `false`）：是否将 `localization.yml` 与 `prefix.yml` 当作 Legacy (§) 解析；默认使用 MiniMessage。
- `forward-sender-uuid`（布尔，默认 `false`）：通过 Adventure 的 Identity 转发作者 UUID（在安全档案场景下可能受限）。

`localization.yml` 示例：

```yaml
GLOBAL_CHAT_TEMPLATE: "[{server}]{player}:" # 模板后会追加消息内容
LOCAL_CHAT_TEMPLATE: "{player}:"           # 模板后会追加消息内容

SERVER_NAME_MAPPINGS:
  lobby: "<aqua>Lobby</aqua>"
  survival: "<green>Survival</green>"
```

`prefix.yml` 示例：

```yaml
any_permission_prefix_set:
  - permission: "horde.prefix.moderator"
    prefix: "<red>[MOD]</red>"
  - permission: "horde.prefix.member"
    prefix: "<gray>[MEMBER]</gray>"
  - permission: "horde.prefix.default"
    prefix: "<white>[PLAYER]</white>"
```

在 `localization.yml` 模板中使用 `{any_permission_prefix_set}` 插入解析出的前缀（无匹配则为空）。

## 指令

- `/horde help` — 显示帮助
- `/horde reload` — 重载配置（权限：`horde.command.reload`）
- `/broadcast [message…]`（别名：`/b`、`/bc`）
  - 带消息：立即发送全局聊天。
  - 不带消息：将你的默认聊天模式设为“全局”。
  - 权限：`horde.command.broadcast`
- `/local [message…]`（别名：`/l`、`/lc`）
  - 带消息：立即发送本地（同服）聊天。
  - 不带消息：将你的默认聊天模式设为“本地”。
  - 权限：`horde.command.local`

## 格式化说明

- 本地化与模板（`localization.yml` / `prefix.yml`）默认使用 MiniMessage。要使用 Legacy（§），请在 `config.yml` 中将 `enable-legacy-localization-formatting: true`。
- 玩家输入的原始消息按 `enable-legacy-chat-formatting` 决定：为 `true` 则支持 `&` 颜色码（Legacy），否则按 MiniMessage 解析。
- 避免重复着色：不要在模板与名称映射/前缀中同时添加颜色代码，除非你有意要叠加样式。

## 工作原理简述

- Horde 监听玩家聊天事件并替换 Vanilla 的聊天投递逻辑。
- 如果玩家默认模式是“全局”并拥有 `horde.command.broadcast`，消息将代理范围广播；如果默认是“本地”并拥有 `horde.command.local`，消息只会发送到当前子服的玩家。
- 若所选模式缺少对应权限，Horde 会拦截消息并提示没有权限。

## 从源码构建

需要 JDK 21 与 Maven。

```powershell
# 在项目根目录执行
mvn -V -B clean package
```

- 构建产物 JAR 位于 `target/`。将其复制到你的 Velocity 代理的 `plugins/` 目录并启动服务器。

## 兼容性与注意事项

- 使用 Velocity 3.x 的 `PlayerChatEvent` 与指令 API。
- `forward-sender-uuid` 在启用安全档案（secure profiles）场景下可能受限；如要启用完整功能，后端可能需要设置 `enforce-secure-profiles=false`（风险自负）。

## 许可证

- MIT 许可证 —— 详见仓库 `LICENSE`。

---

需要帮助或有建议？欢迎提交 Issue 或 PR。祝你聊天愉快！

<script src="https://giscus.app/client.js"
                                data-repo="HyenaMC/blog-site-giscus"
                                data-repo-id="R_kgDOPeyQHQ"
                                data-category="Announcements"
                                data-category-id="DIC_kwDOPeyQHc4CuPDO"
                                data-mapping="pathname"
                                data-strict="0"
                                data-reactions-enabled="1"
                                data-emit-metadata="1"
                                data-input-position="bottom"
                                data-theme="preferred_color_scheme"
                                data-lang="zh-CN"
                                data-loading="lazy"
                                crossorigin="anonymous"
                                async>
</script>
