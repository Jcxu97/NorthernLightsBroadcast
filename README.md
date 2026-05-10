# NorthernLightsBroadcast

[中文说明](#中文) | [English](#english)

---

<a id="english"></a>
## English

A TV mod for The Long Dark. Play local video files on in-game televisions with a full OSD interface (play/pause/stop/seek/volume/file browser).

### Installation

**Prerequisite: MelonLoader (net6) installed**

Place all files from the [release/](./release/) folder into your game's `Mods/` directory:

| File | Description |
|------|-------------|
| `NorthernLightsBroadcast.dll` | Main mod |
| `NorthernLightsBroadcast.modcomponent` | TV 3D models & blueprints |
| `NAudio.dll` | Audio engine |
| `NAudio.Core.dll` | Audio engine core |
| `NAudio.Wasapi.dll` | Windows audio output |
| `NAudio.WinMM.dll` | Windows multimedia |
| `AudioManager.dll` | Dependency (spatial audio) |
| `ModData.dll` | Dependency (save system) |

If you use a modpack, you likely already have `AudioManager.dll` and `ModData.dll`.

### Usage

1. Find a TV in-game (or craft `GEAR_TV_CRT` / `GEAR_TV_LCD` / `GEAR_TV_WALL` via blueprints)
2. Press the red power button to turn on
3. Click the screen to open OSD
4. Click the file browser icon, navigate to your video files
5. Select a file to play

Supported formats: anything Windows Media Foundation can decode (mp4/mkv/avi/wmv, etc.)

### Audio

The Long Dark uses Wwise middleware which completely hijacks Unity's audio output. This mod bypasses it via NAudio WasapiOut, outputting directly to your Windows default audio device.

- TV audio comes from your PC speakers/headphones, unaffected by in-game audio settings
- Volume slider on OSD controls TV volume
- No spatial audio (volume doesn't change with distance)

### Build (developers)

```bash
# Set <TldRoot> in the csproj to your game install path
dotnet build  # auto-deploys to Mods/ and UserLibs/
```

---

<a id="中文"></a>
## 中文

漫漫长夜电视机 mod。在游戏内的电视上播放本地视频文件，带完整 OSD 界面（播放/暂停/停止/拖动进度/音量/文件浏览器）。

### 安装

**前提：已安装 MelonLoader (net6)**

将 [release/](./release/) 文件夹里的所有文件放入游戏目录的 `Mods/` 文件夹：

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

如果你用了整合包，`AudioManager.dll` 和 `ModData.dll` 大概率已经有了。

### 使用方法

1. 游戏内找到电视机（或通过蓝图制作 `GEAR_TV_CRT` / `GEAR_TV_LCD` / `GEAR_TV_WALL`）
2. 按红色电源按钮开机
3. 点击屏幕打开 OSD 菜单
4. 点文件浏览器图标，导航到视频文件
5. 选择文件开始播放

支持格式：Windows Media Foundation 能解码的所有格式（mp4/mkv/avi/wmv 等）

### 音频说明

漫漫长夜使用 Wwise 音频中间件完全接管了 Unity 音频输出，本 mod 通过 NAudio WasapiOut 直接输出到 Windows 默认音频设备：

- 电视音频从电脑音箱/耳机直接出来，不受游戏内音频设置影响
- OSD 上的音量滑块可以调节电视音量
- 无空间音频（距离电视远近不影响音量）

### 构建（开发者）

```bash
# 修改 csproj 中的 <TldRoot> 为你的游戏安装路径
dotnet build  # 自动部署到 Mods/ 和 UserLibs/
```
