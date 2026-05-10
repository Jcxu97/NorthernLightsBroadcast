# NorthernLightsBroadcast - Handoff

## What This Is

A MelonLoader mod for **The Long Dark** (Unity 6, IL2CPP, Wwise audio) that adds functional TVs to the game. Players can browse local video files and play them on in-game television objects with full OSD UI (play/pause/stop/seek/volume/file browser).

## Architecture

### Audio Pipeline (the hard part)

**Problem**: Wwise completely hijacks Unity's audio output. `AudioClip.Create()` returns null, `VideoPlayer` audio modes (AudioSource/Direct) produce silence, and `UnityWebRequestMultimedia` doesn't exist in this IL2CPP build.

**Solution**: NAudio `WasapiOut` → bypasses Unity/Wwise entirely → outputs directly to Windows default audio device.

```
VideoPlayer (video only, audioOutputMode=None)
    ↓ frame sync
WasapiAudioPlayer (NAudio MediaFoundationReader → VolumeSampleProvider → WasapiOut)
    ↓ direct
Windows Audio Device
```

### Key Files

| File | Role |
|------|------|
| `WasapiAudioPlayer.cs` | NAudio wrapper: Open/Play/Pause/Resume/Stop/Seek/Volume/Close |
| `TVManager.cs` | State machine (Off/Static/Preparing/Playing/Paused/Resume/Ended/Error) |
| `TVPlayer.cs` | VideoPlayer event hooks (prepareCompleted/loopPointReached/error) |
| `TVUI.cs` | Full OSD: file browser, playback controls, volume slider, progress bar |
| `tvComponentPatcher.cs` / `tvComponentDeserializePatcher.cs` | Harmony patches to inject TVManager onto TV game objects |
| `SaveLoad.cs` | Per-TV persistence (state, last played file, volume, folder) |
| `NorthernLightsBroadcastMain.cs` | Mod entry point, asset loading |

### State Machine Flow

```
User picks file → TVUI.Prepare() → sets videoPlayer.url → SwitchState(Preparing)
    → VideoPlayer.Prepare() fires
    → TVPlayer.PrepareCompleted → SwitchState(Playing)
        → wasapiPlayer.Open(file) + Seek(savedTime) + Play()
        → videoPlayer.Play()
```

### Audio-Video Sync

Audio and video are opened independently from the same file. `MediaFoundationReader` and `VideoPlayer` both decode from disk. On seek (progress bar drag), both are repositioned:
- `videoPlayer.frame = targetFrame`
- `wasapiPlayer.Seek(targetFrame / frameRate)`

No drift correction is implemented. In practice sync is within ~100ms for local files.

### Volume Control

- `TVUI.VolumeSlider()` sets both `staticAudio._audioSource.volume` (for TV static sound from AssetBundle) and `wasapiPlayer.Volume`
- Mute toggles wasapiPlayer volume to 0 / restores from slider value
- Static audio (TV noise) uses AudioManager's `Shot` system which works fine (AssetBundle clips only)

## Dependencies

- **NAudio 2.2.1** (NuGet) — `NAudio.dll`, `NAudio.Core.dll`, `NAudio.Wasapi.dll`, `NAudio.WinMM.dll`
- **AudioManager.dll** — companion mod for spatial audio (`Shot` class)
- **ModSettings.dll** / **ModData.dll** — settings/save framework
- Unity 6000.0.60f1 IL2CPP assemblies (referenced via `TldRoot` in csproj)

## Build

```bash
dotnet build   # auto-deploys to $(TldRoot)\Mods and $(TldRoot)\UserLibs
```

Set `<TldRoot>` in the csproj to your TLD install path.

## Known Limitations

- Audio output goes to Windows default device regardless of in-game audio settings
- No drift correction between video and audio (acceptable for local files)
- `WasapiOut` doesn't respect Unity's spatial audio — TV audio is always stereo from speakers
- The `playerAudio` Shot field and `videoAudioSource` alias exist only for TVUI volume slider compat — they don't produce actual sound
