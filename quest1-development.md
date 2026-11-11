# Quest 1 cube VR starter

Verified on the original Quest on November 11, 2026. The user confirmed the cube and floor, natural head tracking, and right-A color changes. Device diagnostics independently recorded two-eye stereo, positional and rotational tracking, both controllers tracked at different sample times, and right-A input. Active rendering was about 71–72 FPS.

## Open, build, deploy, launch

Project: `D:\Quest1VRStarter`. Open with Unity **2021.3.45f2**. Scene: `Assets/CubeRoom.unity`.

- Double-click `Build-On-Windows.cmd` to regenerate the cube scene and build the APK. This is a reproducible starter generator; scene edits made directly in the Editor will be replaced by the generator when building. Edit `Assets/Editor/StarterBuild.cs` to change the generated scene.
- APK: `Builds/Android/Quest1VRStarter.apk`.
- Double-click `Launch-On-Quest.cmd` to check the specific USB-connected Quest and launch the installed app. It does not reinstall an existing app. If missing, it installs only the locally hash-verified APK.
- Run `Deploy-Quest.ps1` after rebuilding to explicitly update an existing installation. A first installation can use the launcher.
- Package: `com.creightoninteractive.quest1vrstarter`.
- The headset must show `device` in ADB. If unauthorized, allow USB debugging inside the headset. No system PATH or machine execution-policy changes are needed.

The cube starts 1.8 m in front of the initial viewing direction, 1.2 m above the floor. Blue/orange small boxes represent tracked left/right controllers. Right A toggles teal/orange cube color. Floor tracking is requested; if unsupported, device-origin tracking uses a 1.65 m height offset. No artificial locomotion is applied. Diagnostics log with `CUBEVR`.

## Pinned working environment

- Original Quest firmware: `user-396520.6170.115`, October 22, 2019; Android 7.1.1 / API 25.
- Unity 2021.3.45f2: `D:\DevelopmentTools\UnityEditors\2021.3.45f2`.
- Embedded Oculus XR **1.1.0**, XR Management **3.2.16**, legacy input helpers 2.1.11.
- Android ARM64, IL2CPP, GLES3 only, Oculus multiview, Linear color, minimum API 23 / target API 33, development APK, 2x MSAA, no shadows, 72 FPS target.
- Unity Android SDK with build-tools 34, NDK r21d (21.3.6528147), OpenJDK 11.0.14.1 remain installed with the Editor.
- ADB: `D:\DevelopmentTools\Android\platform-tools\adb.exe`.
- Existing modern Unity installations and unrelated projects remain separate.

## Preserved compatibility fixes

Oculus XR 1.7.0 failed to initialize on this firmware because its native runtime requested VrApi 1.1.40. The embedded 1.1.0 runtime works. Keep its native binaries and licenses intact. Two Editor-only adaptations are retained: the obsolete minimum-API enum is changed from API 19 to 22, and the manifest XML updater sets attributes by name instead of assuming the next sibling exists. Our actual minimum SDK is 23.

`QuestManifest.cs` adds the Oculus VR intent category, supported device metadata, focus awareness and required headtracking capability. `StarterBuild.cs` creates XR settings, materials and the scene through Unity APIs. `StarterExperience.cs` applies head/controller poses during Update and before rendering; it waits briefly after selecting floor origin. FPS sampling resets after pauses.

Use batch builds with graphics enabled. A previous no-graphics path caused ambient-probe warnings. The current cube build completed with **0 errors and 0 warnings**. The legacy Editor does not use the Unity 6 Pipeline package.

## Launching with a blank Oculus Library

USB plus `Launch-On-Quest.cmd` is the verified reliable method. The application runs locally; USB can be disconnected after launch. After quitting or rebooting, reconnect and launch again if Library remains blank. The launcher does not fix the shell.

No third-party launcher was installed. A statically inspected 2019-era QuestAppLauncher v0.10 is a plausible future research candidate, but installation alone does not solve its own entry point when Library is unavailable. It has a debug signing certificate, online metadata behavior and unverified compatibility. Keep this as research, not a recommendation or verified PC-free launch path. No accessibility service, boot receiver, replacement HOME app, wireless ADB listener or firmware changes were made.

## Evidence and limits

Build summaries, runtime diagnostics and visual evidence are retained locally; no assets or executables are published here. The headset screenshot includes the existing Guardian boundary overlay. Both controllers were observed tracked, but not necessarily simultaneously in every diagnostic sample; untracked controllers are hidden. No controller interaction beyond right-A color changes is implemented.

The starter contains procedural primitives and the required Unity/XR packages only; no human models, animation clips, clothing or downloaded scene art. This development build is for local experiments, not a store submission or public deployment. No firmware, account, bootloader, system application or unrelated project changes are part of this work.
