---
name: douyin-upload
description: 抖音创作者平台视频上传发布。触发条件：用户要求上传视频到抖音、发布抖音视频、自动上传视频到抖音创作者平台
---

# douyin-upload

使用 OpenClaw Browser 工具自动上传视频到抖音创作者平台。

## 前置要求

- OpenClaw Gateway 已运行
- 电脑已安装 Chrome 浏览器
- **在 Chrome 中登录抖音账号**（只需一次，之后自动继承登录态）

## 核心原则

**只用 `target="host"`，不要填 `profile` 参数。**

- ✅ `target="host"` - 继承 Chrome 登录态
- ❌ `profile="user"` - 需要开启 remote debugging，不稳定
- ❌ `profile="chrome-relay"` - HTTP 404
- ❌ `target="sandbox"` - 独立沙盒，无登录态

## 工作流程

### Step 1: 打开上传页面

```javascript
browser(action="navigate", target="host", url="https://creator.douyin.com/creator-micro/content/upload")
```

### Step 2: 关闭弹窗

如果页面出现弹窗，先关闭：

```javascript
// 1. 获取当前页面所有可交互元素
browser(action="snapshot", target="host", compact=true, depth=2)

// 2. 在返回结果中查找包含或匹配"我知道了"的按钮，拿到其 ref
// 3. 点击该按钮
browser(action="act", target="host", request={"kind": "click", "ref": "<找到的ref>"})
```

**关键：不要硬编码 ref！** 先 snapshot，再从结果中根据文字内容找到对应元素的 ref。

### Step 4: 准备视频文件

浏览器 tool 只能上传到 `/tmp/openclaw/uploads` 目录：

```bash
mkdir -p /tmp/openclaw/uploads
cp <视频路径> /tmp/openclaw/uploads/
```

### Step 5: 点击上传按钮

```javascript
// 1. 获取上传页面的元素列表
browser(action="snapshot", target="host", compact=true, depth=2)

// 2. 查找包含或匹配"上传视频"文字的按钮，拿到 ref
// 3. 点击上传按钮
browser(action="act", target="host", request={"kind": "click", "ref": "<找到的ref>"})
```

### Step 6: 上传文件

点击上传按钮后会弹出文件选择框：

```javascript
// 1. 先 snapshot 找到 Choose File 或文件输入/上传框的 ref
browser(action="snapshot", target="host", compact=true, depth=2)

// 2. 上传文件
browser(action="upload", target="host", paths=["/tmp/openclaw/uploads/<文件名>"], ref="<找到的ref>")
```

### Step 7: 等待视频解析

页面自动跳转到发布编辑页，显示进度条"0% 文件解析中，请稍等..."，等待视频解析完成后继续。

### Step 8: 填写标题

```javascript
// 1. 获取当前页面元素
browser(action="snapshot", target="host", compact=true, depth=2)

// 2. 查找标题输入框（placeholder 包含或匹配"标题"或"输入标题"的输入框）
// 3. 点击输入框并输入标题
browser(action="act", target="host", request={"kind": "click", "ref": "<找到的ref>"})
browser(action="act", target="host", request={"kind": "type", "ref": "<找到的ref>", "text": "<用户提供的标题>"})
```

### Step 9: 设置作品权限

```javascript
// 1. 获取当前页面元素
browser(action="snapshot", target="host", compact=true, depth=2)

// 2. 查找包含或匹配"公开""好友可见""仅自己可见"或类似文字的单选框, 默认选择"公开"
// 3. 点击选中
browser(action="act", target="host", request={"kind": "click", "ref": "<找到的ref>"})
```

### Step 10: 发布

```javascript
// 1. 获取当前页面元素
browser(action="snapshot", target="host", compact=true, depth=2)

// 2. 查找包含或匹配"发布"文字的按钮
// 3. 点击发布
browser(action="act", target="host", request={"kind": "click", "ref": "<找到的ref>"})
```

成功标志：页面显示「共 X 个作品」「已发布」状态。

## 完整流程模板

```javascript
// 1. 打开上传页面
browser(action="navigate", target="host", url="https://creator.douyin.com/creator-micro/content/upload")

// 2. 关闭弹窗
browser(action="snapshot", target="host", compact=true, depth=2)
// → 从结果中找"我知道了"按钮的 ref，然后点击

// 3. 准备视频文件（需在 terminal 执行）
// mkdir -p /tmp/openclaw/uploads && cp <path> /tmp/openclaw/uploads/

// 4. 点击上传按钮
browser(action="snapshot", target="host", compact=true, depth=2)
// → 从结果中找"上传视频"按钮的 ref，然后点击

// 5. 上传文件
browser(action="snapshot", target="host", compact=true, depth=2)
// → 从结果中找文件输入框的 ref，然后上传
browser(action="upload", target="host", paths=["/tmp/openclaw/uploads/<文件名>"], ref="<ref>")

// 6. 等待解析后填写标题
sleep 15
browser(action="snapshot", target="host", compact=true, depth=2)
// → 从结果中找标题输入框的 ref，然后点击并输入

// 7. 设置仅自己可见
browser(action="snapshot", target="host", compact=true, depth=2)
// → 从结果中找"仅自己可见"选项的 ref，然后点击

// 8. 发布
browser(action="snapshot", target="host", compact=true, depth=2)
// → 从结果中找"发布"按钮的 ref，然后点击
```

## 重要提示

1. **每次操作前都要先 snapshot** - ref 是动态的，每次快照都不同
2. **从 snapshot 结果中根据文字内容查找 ref** - 不要硬编码
3. **页面跳转后需要重新 snapshot** - 元素 ref 会失效
4. **视频必须复制到 `/tmp/openclaw/uploads/`** - 浏览器 tool 的限制
5. **当用户还需要未提及的操作时, 也是同样的方法匹配对应的ref, 然后找到匹配的选项**

## 常见问题

**Q: 为什么提示需要登录？**
A: 请确保 Chrome 已登录抖音。使用 `target="host"` 会自动继承 Chrome 的登录态。

**Q: Element not found 错误？**
A: 页面可能跳转了，需要重新执行 snapshot 获取最新元素。

**Q: 为什么 ref 每次都不一样？**
A: 抖音页面动态渲染，每次 snapshot 生成的 ref 不同。这是正常现象。

