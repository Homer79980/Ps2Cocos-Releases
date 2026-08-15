# Ps2Cocos Releases

Ps2Cocos 是 Cocos Creator `3.8.5+` 的编辑器扩展，用于把 Photoshop 导出的 PS2UI Package 转换为可编辑 UI Prefab。

## 先安装 Photoshop 导出端

Ps2Cocos ZIP 不包含 Photoshop 插件。请先从 [PS2UI Photoshop Releases](https://github.com/Homer79980/PSD2Unity-Releases/releases/latest) 下载最新的 `PS2UI-Photoshop-<版本>.ccx`，安装并重启 Photoshop。

在 Photoshop 中打开 `插件 -> PS2UI`，设置模块名与九宫边界并导出 Package。

## 安装

1. 从 [最新 Release](https://github.com/Homer79980/Ps2Cocos-Releases/releases/latest) 下载 `Ps2Cocos-<版本>.zip`。
2. 在 Cocos Creator 中打开 `Extension -> Extension Manager`。
3. 选择 `Project` 或 `Global`，点击 `+` 并选择 ZIP。
4. 启用 `Ps2Cocos`；若菜单没有出现，重启 Creator。

## 导入

1. 运行 `Extension -> Ps2Cocos -> 导入 Photoshop Package...`。
2. 选择包含 `layout.json` 的 Package 根目录，不要选择 `sprites` 子目录。
3. 在 `assets/ps2cocos/{module}/prefabs` 中打开生成的 Prefab。

普通图导入为 `Sprite`，九宫使用 `Sprite.Type = SLICED`，文字导入为 `Label`。没有字体 JSON 或项目字体映射时仍会完整生成 Prefab，并保留 Package 中的布局、字号、行高、对齐和缩放数据。

## 项目资源复用

- 每次手动导入时扫描 `assets` 中已有 PNG；解码像素完全相同的资源自动复用，不依赖文件名。
- 不同尺寸、镜像或高相似候选由用户确认；九宫只有 SpriteFrame Border 兼容时才复用。
- 不修改被复用的 SpriteFrame 或 `.meta`，也不注册全局资源刷新或构建钩子。

## 字体映射

- `Extension -> Ps2Cocos -> 配置项目字体映射...` 可把 Photoshop 字体名绑定到项目 Font 资产。
- `导入 PS2UI 字体目录...` 和 `导出字体目录给 PS2UI...` 用于团队共享稳定 `fontId`。
- 项目映射保存在 `settings/ps2cocos/font-map.json`；字体目录是可选增强项，不是导入前置条件。

## 查看版本

在 `Extension -> Extension Manager` 中选择 `Ps2Cocos` 查看版本，或打开扩展目录的 `package.json` 查看 `version`。

## 拉伸九宫

选择九宫节点，确认 `Sprite.Type = SLICED`、`Size Mode = CUSTOM`，然后拖动矩形边框或修改 `UITransform.Width/Height`。不要修改 `Scale X/Y`，否则固定边角也会被整体缩放。

生成的 Prefab 可能在重导时被覆盖，业务脚本应挂在引用生成 Prefab 的外壳 Prefab 上。

完整源码和详细说明见 [PSD2Unity-Source](https://github.com/Homer79980/PSD2Unity-Source/tree/main/CocosCreatorExtension)。
