# ChatGPT MV Skill

适用于 Codex / GPT-6.1 的程序化手绘水彩动画与歌词 MV skill。

保留 p5.js、p5.brush 动画引擎、角色工具、分镜示例、节拍分析与歌词同步，通过 Chrome 逐帧渲染，由 ffmpeg 编码视频。无需图像或视频生成 API。

## 安装

将 `skills/painted-animation` 目录复制到 `~/.codex/skills/painted-animation`。如果已有同名 skill，先备份后再更新。下一轮对话即可使用。

## 使用

```text
使用 $painted-animation，制作一个 15 秒的手绘水彩动画。
```

制作歌词 MV 时提供歌曲音频、LRC 或已标注时间的歌词，以及期望的故事和风格。Skill 不会切换模型，请自行选择 GPT-6.1。

## 运行依赖

- Node.js / npm
- Chrome 或 Chromium
- ffmpeg
- Python 3 和 numpy（节拍分析）
- 可执行代码并查看预览图片的运行环境

仅有文本聊天能力的环境不能直接完成视频渲染。

## 项目结构

- `skills/painted-animation/SKILL.md`：工作流程与使用指令
- `skills/painted-animation/agents/openai.yaml`：界面元数据
- `skills/painted-animation/template/`：可运行的动画工程模板
- `skills/painted-animation/scripts/`：工程初始化、节拍分析
- `skills/painted-animation/references/`：音乐视频制作指南
- `skills/painted-animation/examples/`：完整分镜与场景示例

已验证 skill 格式、脚本语法与打包完整性；尚未进行完整视频渲染验证。

## 来源与许可

基于 [Joker-chichi/claude-mv-skill](https://github.com/Joker-chichi/claude-mv-skill) 转换，来源提交与调整说明见 `skills/painted-animation/SOURCE.md`。

原作者与引擎作者的 MIT 版权声明保留在根目录 `LICENSE` 及模板 `LICENSE` 中。Clawd 为继承的示例角色，可替换为其他角色。
