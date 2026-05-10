# NorthernLightsBroadcast

The Long Dark 电视机 mod。在游戏内的电视上播放本地视频文件，带完整 OSD 界面（播放/暂停/停止/拖动进度/音量/文件浏览器）。

## 安装

**前提：已安装 MelonLoader (net6)**

将以下文件全部放入游戏目录的 `Mods/` 文件夹：

| 文件 | 说明 |
|------|------|
| `NorthernLightsBroadcast.dll` | 主模组 |
| `NorthernLightsBroadcast.modcomponent` | 电视机 3D 模型和蓝图 |
| `NAudio.dll` | 音频引擎 |
| `NAudio.Core.dll` | 音频引擎核心 |
| `NAudio.Wasapi.dll` | Windows 音频输出 |
| `NAudio.WinMM.dll` | Windows 多媒体支持 |
| `AudioManager.dll` | 前置 mod（空间音频） |
| `ModData.dll` | 前置 mod（存档系统） |

所有文件在 [release/](./release/) 文件夹里可以直接下载。

`AudioManager.dll` 和 `ModData.dll` 如果你装了整合包大概率已经有了，检查一下 Mods 目录即可。

## 使用方法

1. 游戏内找到电视机（或通过蓝图制作 `GEAR_TV_CRT` / `GEAR_TV_LCD` / `GEAR_TV_WALL`）
2. 按红色电源按钮开机
3. 点击屏幕打开 OSD 菜单
4. 点文件浏览器图标，导航到你电脑上的视频文件
5. 选择文件开始播放

支持格式：Windows Media Foundation 能解码的所有格式（mp4/mkv/avi/wmv 等）

## 音频说明

由于 The Long Dark 使用 Wwise 音频中间件完全接管了 Unity 音频输出，本 mod 通过 NAudio WasapiOut 直接输出到 Windows 默认音频设备。这意味着：

- 电视音频从你的电脑音箱/耳机直接出来，不受游戏内音频设置影响
- OSD 上的音量滑块可以调节电视音量
- 游戏内距离电视远近不影响音量（非空间音频）

## 构建（开发者）

```bash
# 修改 csproj 中的 <TldRoot> 为你的游戏安装路径
dotnet build  # 自动部署到 Mods/ 和 UserLibs/
```
