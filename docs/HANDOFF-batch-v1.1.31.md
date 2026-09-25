# 幻境 v1.1.31 交接与迁移说明

日期：2026-09-25。当前收官版本：1.1.31（versionCode 91）。包名：com.rpengine.app。以本文件为当前入口；旧版本说明仅供历史追溯。

## 交付物

- 幻境-v1.1.31.apk：包含现有幻境名称、启动图标及完整设置 UI 的可覆盖安装包。
- rp-android-batch-v1.1.31.zip：完整 Android 源码、资源、Gradle wrapper、说明文档、最终 APK、真机截图、测试记录。
- 对应 SHA-256 校验文件；LATEST.txt 指向本版。

## 当前产品状态

聊天顶部为中性玻璃角色栏，旁白星环与角色色点区分，胶囊按内容宽度排列，窄屏横向滚动。本轮顺序保持小号文字。背景使用完整图像与模糊补边。

底部三个按钮与卡片标题复用图标：羽毛笔、星点定位针、蝴蝶结缎带。记忆为聊天内功能卡片，按世界隔离，分待确认记忆、关系变化、已确认记忆。角色编辑为在场角色和快速编辑两张折叠卡，快速编辑支持可选身份及与玩家关系。场景态为三个紧凑单行字段，副标题显示实际保存的角色、情绪、目标和地点。右上菜单仅更新聊天页弹出样式。

设置页包含另外任务已完成的模型配置折叠卡片、当前配置概览和数据与备份区域。后续工作务必从本完整源码继续，不要用 1.1.19 或中间版本覆盖当前文件。

## 安装与数据迁移

同一设备直接安装本 APK 或 adb install -r，保留现有应用数据。当前交付沿用本机 debug 签名，属于可测试安装包，未配置商店发布签名。未来重建如需覆盖现有安装，必须使用同一签名；签名私钥不在迁移包内。

源码迁移包不包含手机中的世界、聊天数据库、私人立绘和接口密钥。换手机时先在旧设备设置「数据与备份」中导出世界，按需勾选会话，再在新设备安装 APK 并导入；接口配置按应用支持的方式重新设置。不要为了升级而卸载旧应用。

## 开发环境

JDK 21（本机 JBR 21.0.11），Android SDK 34，Gradle wrapper 随包提供。在新机器 android/local.properties 中设置自己的 sdk.dir；安装所需 SDK，首次构建允许下载依赖。

```powershell
cd android
.\gradlew.bat assembleDebug
```

本机已有缓存验证命令：assembleDebug assembleQa assembleQaAndroidTest --offline。QA 独立包名 com.rpengine.app.qa，测试包 com.rpengine.app.qa.test；10 项测试全部通过。正式包已覆盖安装到 Fold 6 并核对版本。

## 后续修改入口

- ui/screens/chat/ChatScreen.kt：共享功能图标、菜单、场景态实时副标题。
- ChatRoleEditor.kt、ChatFeatureDialog.kt、ChatFeatureHeader.kt：卡片母版。
- MemoryFeatureCards.kt、ChatSceneField.kt：记忆与场景态内容。
- ui/screens/settings/SettingsScreen.kt：已合入的设置页。
- drawable-nodpi/ic_memory_quill_generated.png、ic_scene_ai.png、ic_role_ai.png：三功能图标。

本版不改变数据库结构。测试记录和截图位于包内 reference/，各文件散列见 FILES.sha256。
