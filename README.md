# 抖音/Douyin 自动发布skill

开箱即用! 自动上传视频并发布到抖音创作者平台。适用于Openclaw, Claude Cowork, Codex等AI Agent应用.

## 安装 Skill

### OpenClaw

```bash
# 方式一：使用 ClawHub CLI 安装
clawhub install douyin-upload

# 方式二：手动安装
# 将 skill 文件夹复制到 ~/.openclaw/skills/
```

### Claude Code

```bash
# 将 skill 文件夹复制到 ~/.claude/skills/
```

### Antigravity

```bash
# 全局安装：将 skill 文件夹复制到 ~/.gemini/antigravity/skills/
# 项目安装：将 skill 文件夹复制到 <项目目录>/.agent/skills/
```

### Cursor

```bash
# 全局安装：将 skill 文件夹复制到 ~/.cursor/skills/
# 项目安装：将 skill 文件夹复制到 <项目目录>/.cursor/skills/
```

### Trae

```bash
# 项目安装：将 skill 文件夹复制到 <项目目录>/.agent/skills/
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
A: 目前仅支持立即发布。

## 相关链接

- [OpenClaw 文档](https://docs.openclaw.ai)
- [抖音创作者平台](https://creator.douyin.com/creator-micro/home)
