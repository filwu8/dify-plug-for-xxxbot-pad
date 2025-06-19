# Dify 插件 🤖

## 简介
本版本主要基于HenryXiaoYang的插件修改， 增加黑名单功能，支持屏蔽指定用户和群组的消息。🚫优化了在使用自己的微信绑定机器人的时候，设置自己为黑名单，自己主动说的话，不会被传递给dify处理了。



Dify 插件是为 XYBotV2 机器人框架设计的一个插件，它允许机器人与 Dify (一个 LLM 应用开发平台) 进行交互。通过这个插件，你可以让你的微信机器人具备强大的自然语言处理能力，例如文本生成、对话、语音处理和文件处理等。🚀

## 特性

- **多消息类型支持:** 支持文本、@消息、语音、图片、视频和文件消息的处理。💬
- **Dify 集成:** 无缝对接 Dify 平台，利用其强大的 LLM 能力。🔗
- **Agent 模式支持:** 支持 Dify 的 Agent 模式，可以处理复杂的多轮对话和工具调用。🧠
- **灵活的配置:** 允许配置 API 密钥、基础 URL、命令、提示语、价格、代理等。⚙️
- **积分系统集成:** 可以配置是否管理员和白名单用户忽略积分检查。💰
- **黑名单功能:** 支持用户和群组黑名单，屏蔽指定用户的消息，不转发给Dify处理。🚫
- **流式响应:** 使用 Dify 的流式响应模式，逐步返回结果，提升用户体验。✨
- **语音合成 (TTS) 支持:** 可选的 TTS 功能，将文本回复转换为语音消息。🗣️
- **文件上传:** 支持上传语音、图片、视频和文件到 Dify 进行处理。📤
- **媒体文件处理:** 自动识别并发送回复中的链接指向的媒体文件（语音、图片、视频）。🖼️
- **错误处理:** 完善的错误处理机制，当 Dify 返回错误时，能向用户提供清晰的错误信息。⚠️

## 安装

1.  确保你已经安装了 XYBotV2 机器人框架。 ✅
2.  将 `Dify` 插件文件夹复制到 XYBotV2 的 `plugins` 目录下。 📁

## 配置

1.  编辑 `main_config.toml` 文件，配置管理员列表：

    ```toml
    [XYBot]
    admins = ["your_wxid"] # 你的微信ID
    ```

2.  编辑 `plugins/Dify/config.toml` 文件，配置 Dify 插件：

    ```toml
    [Dify]
    enable = true          # 是否启用插件
    default-model = "学姐"  # 默认使用的模型
    commands = ["聊天", "AI"]
    # 聊天室功能已移除
    support_agent_mode = true  # 是否支持Agent模式
    
    # 黑名单设置
    blacklist_enable = true         # 是否启用黑名单功能
    blacklist_users = [             # 黑名单用户列表（微信ID）
        "wxid_example1",            # 示例：添加需要屏蔽的用户微信ID
        "wxid_example2",            # 可以添加多个用户ID
    ]
    blacklist_groups = [            # 黑名单群组列表（群组ID）
        "group_example1",           # 示例：添加需要屏蔽的群组ID
        "group_example2",           # 可以添加多个群组ID
    ]
    
    command-tip = """-----XYBot-----
    💬AI聊天指令：
    1. 切换模型（将会一直保持到下次切换）：
       - @学姐 切换：切换到学姐模型
       - @Gordon 切换：切换到Gordon模型
    2. 临时使用其他模型：
       - 学姐 消息内容：临时使用学姐模型
       - Gordon 消息内容：临时使用Gordon模型"""
    admin_ignore = true            # 管理员是否免积分
    whitelist_ignore = true        # 白名单用户是否免积分
    http-proxy = ""                # HTTP代理配置
    voice_reply_all = false        # 是否总是使用语音回复
    robot-names = ["毛球", "🥥", "智能助手"]

    [Dify.models]
    # 学姐模型配置
    [Dify.models."学姐"]
    api-key = "app-XXXqqQIi8bZfvmaFc56WPyYH"
    base-url = "http://XXXX/v1"
    trigger-words = ["@学姐"]
    wakeup-words = ["学姐"]
    price = 0

    # Gordon模型配置
    [Dify.models."Gordon"]
    api-key = "app-XXXqqQIi8bZfvmaFc56WPyYH"
    base-url = "http://XXXX/v1"
    trigger-words = ["@Gordon"]
    wakeup-words = ["Gordon"]
    price = 0
    ```

## 使用方法

1.  在微信中向机器人发送配置的命令，例如 `dify 你好` 或者 `@机器人 dify 你好` (在群聊中)。💬
2.  机器人会将你的消息发送到 Dify，并将 Dify 的回复返回给你。 🤖
3.  如果启用了 TTS，机器人会将文本回复转换为语音消息。 🗣️

### Agent 模式使用

如果你的 Dify 应用配置了 Agent 模式，本插件可以支持 Agent 的各种功能：

1. **工具调用**: Agent 可以调用各种工具来完成任务，如搜索网络、生成图片等。
2. **文件处理**: Agent 可以生成和处理各种文件，如图片、文档等。
3. **思考过程**: 插件会记录 Agent 的思考过程，便于调试和优化。

要使用 Agent 模式，请确保在配置文件中设置`support_agent_mode = true`，并且你的 Dify 应用已配置为 Agent 模式。

### 黑名单功能使用

黑名单功能允许你屏蔽特定用户或群组的消息，这些消息不会被转发给Dify处理：

#### 配置黑名单

在 `config.toml` 文件中配置黑名单：

```toml
[Dify]
# 黑名单设置
blacklist_enable = true         # 是否启用黑名单功能
blacklist_users = [             # 黑名单用户列表（微信ID）
    "wxid_example1",            # 添加需要屏蔽的用户微信ID
    "wxid_example2",            # 可以添加多个用户ID
]
blacklist_groups = [            # 黑名单群组列表（群组ID）
    "group_example1",           # 添加需要屏蔽的群组ID
    "group_example2",           # 可以添加多个群组ID
]
```

#### 黑名单效果

- **用户黑名单**: 被加入黑名单的用户发送的所有消息（文本、语音、图片、文件等）都会被屏蔽
- **群组黑名单**: 被加入黑名单的群组中的所有消息都会被屏蔽
- **日志记录**: 被屏蔽的消息会在日志中记录，便于管理员查看
- **无响应**: 黑名单用户的消息不会收到任何回复，完全静默处理

#### 获取微信ID

要获取用户的微信ID，可以通过以下方式：
1. 查看机器人日志中的消息记录
2. 使用其他插件或工具获取用户ID
3. 在群聊中，群组ID通常是群聊的标识符

#### 注意事项

- 黑名单功能对所有消息类型都有效（文本、语音、图片、文件、引用消息等）
- 管理员和白名单用户的特权不受黑名单影响
- 修改黑名单配置后需要重启插件才能生效

## 消息类型支持

- **文本消息:** 直接发送文本消息给机器人。 📝
- **@消息:** 在群聊中 @ 机器人并发送消息。 📢
- **语音消息:** 发送语音消息给机器人。 🎤
- **图片消息:** 发送图片消息给机器人。 🖼️
- **视频消息:** 发送视频消息给机器人。 🎬
- **文件消息:** 发送文件消息给机器人。 📄

## 积分系统

- 插件使用了 XYBotDB 来管理用户的积分。 📊
- 每次调用 Dify 插件会消耗用户一定数量的积分（可在配置文件中设置）。 💸
- 管理员和白名单用户可以配置为忽略积分检查。 🛡️

## 依赖

- XYBotV2 机器人框架
- `aiohttp`
- `filetype`
- `loguru`
- `tomllib` (Python 3.11+) or `toml` (Python < 3.11)
- `WechatAPI`
- `database.XYBotDB`
- `utils.decorators`
- `utils.plugin_base`

## Change Log

- **1.0.0** 初始版本 🐣
- **1.1.0 (2025-02-20)** 插件优先级，插件阻塞。 🚦
- **1.2.0 (2025-02-22)** 有插件阻塞了，other-plugin-cmd 可删了. 增加了语音合成(TTS)支持. 🎉
- **1.3.0 (2025-05-07)** 添加对 Dify Agent 模式的支持，支持处理 agent_thought 和 agent_message 事件。 🤖
- **1.4.0 (2025-05-08)** 移除聊天室功能，简化插件结构。 🧹
- **1.5.0 (2025-05-23)** 增加图片引用功能，支持在引用消息中处理图片。 🖼️
- **1.6.0 (2025-06-19)** 增加黑名单功能，支持屏蔽指定用户和群组的消息。🚫

## 注意事项

- 请确保你的 Dify API 密钥和基础 URL 配置正确。🔑
- 语音合成功能依赖于第三方 API，请确保 API 可用。 🌐
- 如果遇到问题，请查看 XYBotV2 的日志文件 `logs/xybot.log`。 🔍

