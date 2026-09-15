# dsh-strata · 定制版

**把 DSH Web GUI 的对话滚动条，变成一张整个会话的地图。**

滚动条不再只是干巴巴的滑块——它按每条消息**真实渲染高度**等比压缩成一张彩色地图，你自己发的话被高亮，点一下就能跳回那一轮。鼠标停到右边缘，弹出「历史提问」卡片墙，滚一格翻一条。

---

> ### 关于这个仓库
>
> 本仓库是 **[jsdvjx/dsh-strata](https://github.com/jsdvjx/dsh-strata)** 的**定制衍生版**。
>
> 原版由 **[@jsdvjx](https://github.com/jsdvjx)** 创作，MIT 许可。地图引擎、数据通道、整体设计思路全部来自原作者——**感谢原作者**。
>
> 本版本在原作者 v0.12.1 的基础上重做了交互层。如果你想要原汁原味的版本，请直接使用[上游仓库](https://github.com/jsdvjx/dsh-strata)。

---

## 它做什么

- **比例地图**：每一条消息按它**实际的渲染高度**占据地图上的一段，所以地图是对话滚动范围的等比压缩，而不是等分刻度。
- **颜色区分**：用户消息、助手消息、工具调用各有一种颜色，你自己说的话最显眼。
- **点击跳转**：点地图上的色块 / 锚点圆点，直接滚到那一轮。
- **历史提问面板**：鼠标停到右边缘的蓝色按钮上，弹出本会话**每一条提问**的卡片列表。
- **自动加载历史**：往回滚时会自动把更早的历史加载进来，地图跟着长。
- **零数据通道**：不读会话状态、不发请求。几何和语义全部来自对话视图本来就发布的锚点属性，所以它是一张**地图**，而不是一根刻度尺。

## 交互（本版取向）

| 操作 | 效果 |
| --- | --- |
| 鼠标移到右边缘蓝点 | 展开「历史提问」卡片墙 |
| 滚轮 | **一格 = 一条消息**，逐条步进 |
| 滚到列表尽头 | 弹一句提示（"你底到我啦" / "你顶到我啦"），**不吞掉滚轮事件**，页面仍可正常滚动 |
| 拖动蓝点 | 按钮可拖到任意位置，位置记在 `localStorage`，刷新后保留 |
| 点击卡片 | 跳回该轮，该卡片显示蓝色焦点框 |
| 移动端 / 窄屏 | 卡片强制单列 |

## 安装

这是一个 DSH **profile 插件包**（`dsh.bundle` + `dsh.client` 双声明，Node 半边故意留空，全部逻辑在浏览器侧）。

```bash
# 方式一：从 GitHub 直接装入 profile
dsh plugin --profile web add github:Moleitau-WorldSaver/dsh-strata-custom

# 方式二：clone 下来，用本地路径装
git clone https://github.com/Moleitau-WorldSaver/dsh-strata-custom
dsh plugin --profile web add file:/绝对路径/dsh-strata-custom
```

装完重启 DSH Web GUI（或热重载插件）即可看到效果。

### ⚠️ 注意

- **包名仍是 `dsh-strata`**，因此**不能与上游原版同时安装** —— 两者会争抢同一个 `cordis.patch.yml` 里的 `id: strata` 装配位。装之前请先卸掉另一个。
- 本包**没有构建步骤**：`client.js` 就是手写源码，由 DSH 的 client-modules 直接下发给浏览器。改完刷新页面即刻生效，不需要 `npm run build`。

## 兼容性

- 需要 DSH Web GUI（`dsh web`）。
- 对等依赖：`@deepseek-ai/cordis`。
- 浏览器侧注入：`@deepseek-ai/dsh-client-runtime`、`-ui-layout`、`-ui-conversation`、`-ui-chat`。

## 许可

MIT，见 [LICENSE](./LICENSE)。

原始版权归 **jsdvjx** 所有；本衍生版的改动部分版权归 **Moleitau-WorldSaver**。按 MIT 要求，两者的版权声明均予保留。
