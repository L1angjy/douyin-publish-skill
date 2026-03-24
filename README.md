# 抖音/Douyin 自动发布skill

- 开箱即用! 自动上传视频并发布到抖音创作者平台。适用于Openclaw, Claude Cowork, Codex等AI Agent应用.
- 该Skill不包含任何外部指令, 可放心使用. 同时也可以根据不同工作流/业务逻辑进行调整.

## 安装 Skill

### OpenClaw

```bash
# 将 skill 文件夹复制到 ~/.openclaw/skills/
```

### Claude Code / Claude CoWork

```bash
# 将 skill 文件夹复制到 ~/.claude/skills/
```

### Codex

```bash
# 将 skill 文件夹复制到 ~/.codex/skills/
```

### 其他 AI Agent 应用

Skill 本质上是一个包含 `SKILL.md` 文件的文件夹。大多数 AI Agent 应用都支持以下目录结构：

```
~/.claude/skills/          # Claude Code/CoWork 全局
~/.codex/skills/          # Codex 全局
~/.openclaw/skills/       # OpenClaw 全局
.agent/skills/            # 项目级 skill（多个应用通用）
```

## 准备工作（只需做一次）

### 在 Chrome 登录抖音

1. 打开电脑上的 Chrome 浏览器
2. 访问 [抖音创作者平台](https://creator.douyin.com/creator-micro/home)
3. 使用抖音账号登录
4. 建议保持 Chrome 的运行状态

**只需登录一次**，之后 AI 会自动继承你的登录状态。

## 使用方法

告诉 AI：「帮我上传视频到抖音」并提供：

- 视频文件路径
- 视频标题
- 可见范围（公开/仅自己可见）

AI 会自动完成全部流程。

## 示例

```
用户: 帮我上传 ~/Videos/my_video.mp4 到抖音，标题是「汽车保养小技巧」，公开可见
```

## 工作流程

```
1. 打开抖音上传页面
2. 关闭弹窗（如有）
3. 上传视频文件
4. 填写标题
5. 设置可见范围
6. 发布
```

## 常见问题

**Q: 为什么提示需要登录？**
A: 请确保 Chrome 已登录抖音创作者平台。AI 使用你 Chrome 的登录状态。

**Q: 支持哪些视频格式？**
A: 与抖音网页版支持格式相同，通常为 MP4、MOV 等常见格式。

**Q: 可以设置定时发布吗？**
A: 可以通过 Cron Job 等方式让AI Agent定时调用skill即可。

## 相关链接

- [OpenClaw 文档](https://docs.openclaw.ai)
- [抖音创作者平台](https://creator.douyin.com/creator-micro/home)
