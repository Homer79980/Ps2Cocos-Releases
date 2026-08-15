# Ps2Cocos Releases

Ps2Cocos `0.1.1` 是 Cocos Creator `3.8.5+` 编辑器扩展，用于把 PS2UI Photoshop Package 转换为可编辑 UI Prefab。Creator 2.4 使用不同的扩展与序列化系统，本版本不支持。

## 前置：安装 PS2UI

本 ZIP 不包含 Photoshop 插件。先从 [PS2UI Releases](https://github.com/Homer79980/PS2UI-Releases/releases/latest) 安装 `PS2UI-Photoshop-<版本>.ccx`，然后在 Photoshop 中导出包含 `layout.json` 和 `sprites/` 的 Package。

## 安装

1. 从 [最新 Release](https://github.com/Homer79980/Ps2Cocos-Releases/releases/latest) 下载 `Ps2Cocos-0.1.1.zip`。
2. 在 Creator 打开 `Extension -> Extension Manager`。
3. 选择 `Project` 或 `Global`，点击 `+` 并选择 ZIP。
4. 启用 `Ps2Cocos`；菜单未出现时重启 Creator。

手工安装时，把 ZIP 解压为项目的 `extensions/ps2cocos`，该目录下应直接看到 `package.json`、`dist/` 和 `node_modules/pngjs/`，不能再多套一层目录。

## 导入操作

1. 执行 `Extension -> Ps2Cocos -> 导入 PS2UI 导出包...`。
2. 选择 Package 根目录，不要选择 `sprites` 子目录。
3. “字体与导入”会列出项目字体、已记住默认和待处理字体；可立即绑定，也可暂用 Cocos 默认字体继续。
4. 等待“Ps2Cocos 导入完成”提示。
5. 在 `assets/ps2cocos/{module}/prefabs` 打开生成的 Prefab。

Schema 1 生成一个 Prefab，Schema 2/3 为每个可见画板生成一个 Prefab。普通图片是 `Sprite`，九宫使用 `Sprite.Type = SLICED`，文字是 `Label`，按钮保留 `Button` 组件。

## 字体映射

PS2UI Package 已包含当前设计使用的字体身份、字号、行高、字距、对齐和文字矩形，不需要在导入前额外准备字体 JSON。

- 项目字体精确且唯一时可自动匹配；同名候选超过一个时必须人工选择。
- 没有项目字体时仍完整生成 Prefab，文字暂用默认字体并保留待绑定身份。
- 映射保存在 `settings/ps2cocos/font-map.json`，建议随项目版本控制。
- 已删除或失效的 AssetDB 字体引用会回到待处理状态。
- “高级字体目录”只用于团队共享稳定字体身份和样式，不包含字体文件，也不是日常导入前置条件。

## 项目资源复用

- 解码后的 RGBA 像素与尺寸完全一致：自动复用项目已有 SpriteFrame，不依赖名称。
- 不同尺寸、镜像或高相似候选：由用户确认是否复用。
- 九宫只有 SpriteFrame Border 完全兼容时才复用。
- 复用资源和 `.meta` 保持只读；插件不注册全局资源刷新或构建钩子。
- 指纹与确认结果缓存在项目根目录 `.ps2cocos/`。

## 九宫拉伸

选择九宫节点，确认 `Sprite.Type = SLICED`、`Size Mode = CUSTOM`，再拖动矩形边框或修改 `UITransform.Width/Height`。不要修改 `Scale X/Y`，否则固定边角也会整体缩放。

## 查看版本

- 在 `Extension -> Extension Manager` 的 Ps2Cocos 详情中查看 Version。
- 或查看 `extensions/ps2cocos/package.json` 的 `version`。
- Extension 菜单只保留一个中文导入入口。

## 升级、卸载与排错

- 升级：在 Extension Manager 停用旧版，用新版 ZIP 覆盖安装后重启 Creator。
- 卸载：停用扩展并删除 `extensions/ps2cocos`；生成的 `assets/ps2cocos` 可按项目需要保留。
- 菜单未出现：确认 Creator 版本至少为 3.8.5，并检查扩展目录根部是否直接包含 `package.json`。
- 重导可能覆盖生成 Prefab，业务脚本应挂在引用生成 Prefab 的外壳 Prefab 上。

## 校验下载

```powershell
Get-FileHash .\Ps2Cocos-0.1.1.zip -Algorithm SHA256
```

输出应与 `Ps2Cocos-0.1.1-SHA256.txt` 一致。
