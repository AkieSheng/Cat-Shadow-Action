# 目录

1. [一、项目总架构](#一项目总架构)
2. [二、Data 数据资产表](#二data-数据资产表)
3. [三、接口 Interface 表](#三接口-interface-表)
4. [四、核心蓝图资产表](#四核心蓝图资产表)
5. [五、BP_RunManager 变量表](#五bp_runmanager-变量表)
6. [六、BP_RunManager 事件调度器表](#六bp_runmanager-事件调度器表)
7. [七、BP_RunManager 事件 / 函数表](#七bp_runmanager-事件--函数表)
8. [八、BP_RandomSpawnManager 变量表](#八bp_randomspawnmanager-变量表)
9. [九、BP_RandomSpawnManager 事件 / 函数表](#九bp_randomspawnmanager-事件--函数表)
10. [十、BP_NightCatCharacter 变量表](#十bp_nightcatcharacter-变量表)
11. [十一、BP_NightCatCharacter 事件 / 函数表](#十一bp_nightcatcharacter-事件--函数表)
12. [十二、保险箱蓝图变量与函数表](#十二保险箱蓝图变量与函数表)
13. [十三、交互物蓝图变量与函数表](#十三交互物蓝图变量与函数表)
14. [十四、机关蓝图变量与函数表](#十四机关蓝图变量与函数表)
15. [十五、SpawnPoint 点位蓝图表](#十五spawnpoint-点位蓝图表)
16. [十六、UI 蓝图变量与函数表](#十六ui-蓝图变量与函数表)
17. [十七、Mesh 模型表](#十七mesh-模型表)
18. [十八、Materials 材质表](#十八materials-材质表)
19. [十九、Textures 纹理表](#十九textures-纹理表)
20. [二十、VFX 特效表](#二十vfx-特效表)
21. [二十一、课程 Demo 资产清单](#二十一课程-demo-资产清单)

## 一、项目总架构

```text
Content
└─ CatShadowAction
   ├─ Audio
   ├─ Blueprints
   │  ├─ Character
   │  │  └─ BP_NightCatCharacter
   │  ├─ Core
   │  │  ├─ BP_RunManager
   │  │  └─ BP_RandomSpawnManager
   │  ├─ Interactables
   │  │  ├─ Doors
   │  │  │  └─ BP_AccessDoor
   │  │  ├─ Extractions
   │  │  │  └─ BP_ExtractionPoint
   │  │  ├─ Pickups
   │  │  │  └─ BP_AccessCardPickup
   │  │  ├─ SafeBoxes
   │  │  │  ├─ BP_SafeBox_Base
   │  │  │  ├─ BP_SafeBox_Normal
   │  │  │  ├─ BP_SafeBox_Encrypted
   │  │  │  └─ BP_SafeBox_Advanced
   │  │  └─ Traps
   │  │     ├─ BP_LaserTrap
   │  │     ├─ BP_CameraTrap
   │  │     └─ BP_AlertLight
   │  ├─ Interfaces
   │  │  └─ BPI_Interactable
   │  ├─ SpawnPoints
   │  │  ├─ BP_SafeBoxSpawnPoint
   │  │  └─ BP_PickupSpawnPoint
   │  └─ UI
   │     ├─ WBP_HUD
   │     ├─ WBP_StatusPanel
   │     ├─ WBP_Settlement
   │     └─ WBP_PauseMenu
   ├─ Data
   │  ├─ E_SafeBoxType
   │  ├─ E_LootType
   │  ├─ E_AlertStage
   │  └─ ST_LootInfo
   ├─ Maps
   │  └─ MAP_BankMaze_Demo
   ├─ Materials
   ├─ Meshes
   ├─ Textures
   └─ VFX
```

---

## 二、Data 数据资产表

| 资产名           | 类型               | 内容                                                     | 用途                        |
| ------------- | ---------------- | ------------------------------------------------------ | ------------------------- |
| E_SafeBoxType | Enumeration / 枚举 | Normal、Encrypted、Advanced                              | 区分普通保险箱、加密保险箱、高级保险箱       |
| E_LootType    | Enumeration / 枚举 | CommonItem、RareItem、AdvancedItem、AccessCard、CoreDrive  | 区分普通物品、稀有物品、高级物品、门禁卡、核心硬盘 |
| E_AlertStage  | Enumeration / 枚举 | Safe、Caution、AlarmOne、AlarmTwo、HighRisk、Extreme、Failed | 区分警戒阶段                    |
| ST_LootInfo   | Structure / 结构体  | LootType、DisplayName、Value、CapacityCost、AlertBonus     | 存储收益类型、显示名称、价值、容量占用、额外警戒值 |

---

## 三、接口 Interface 表

### BPI_Interactable

| 接口函数            | UE 节点显示                   | 输入                                               | 输出        | 用途            |
| --------------- | ------------------------- | ------------------------------------------------ | --------- | ------------- |
| Interact        | Interact (Message)        | Interactor：BP_NightCatCharacter Object Reference | 无         | 玩家按 F 后触发交互   |
| GetInteractText | GetInteractText (Message) | Interactor：BP_NightCatCharacter Object Reference | Text：Text | 返回 HUD 交互提示文本 |

---

## 四、核心蓝图资产表

| 蓝图名                   | 路径                        | 类型          | 主要职责                                       |
| --------------------- | ------------------------- | ----------- | ------------------------------------------ |
| BP_RunManager         | Blueprints/Core           | Actor       | 管理收益、容量、门禁卡、核心硬盘、警戒值、倒计时、撤离点数量、任务成功失败、结算评价 |
| BP_RandomSpawnManager | Blueprints/Core           | Actor       | 每局随机激活普通保险箱点位、门禁卡点位、激光、摄像头、撤离点、特殊区保险箱      |
| BP_NightCatCharacter  | Blueprints/Character      | Character   | 玩家移动、视角、奔跑、交互射线、按 F 交互、按 B 状态界面、Esc 暂停     |
| BP_SafeBox_Base       | Interactables/SafeBoxes   | Actor       | 保险箱父类，处理开箱、产出、警戒值、材质变化                     |
| BP_SafeBox_Normal     | Interactables/SafeBoxes   | Child Actor | 普通保险箱                                      |
| BP_SafeBox_Encrypted  | Interactables/SafeBoxes   | Child Actor | 加密保险箱                                      |
| BP_SafeBox_Advanced   | Interactables/SafeBoxes   | Child Actor | 高级保险箱                                      |
| BP_AccessCardPickup   | Interactables/Pickups     | Actor       | 门禁卡拾取物                                     |
| BP_AccessDoor         | Interactables/Doors       | Actor       | 门禁特殊区入口门                                   |
| BP_ExtractionPoint    | Interactables/Extractions | Actor       | 撤离点，判断是否开放、是否持有核心硬盘                        |
| BP_LaserTrap          | Interactables/Traps       | Actor       | 激光机关，玩家碰到后警戒值 +5                           |
| BP_CameraTrap         | Interactables/Traps       | Actor       | 摄像头机关，玩家停留 0.3 秒后警戒值 +10，随后 3 秒冷却          |
| BP_AlertLight         | Interactables/Traps       | Actor       | 根据警戒值变化灯光颜色和闪烁                             |
| BP_SafeBoxSpawnPoint  | SpawnPoints               | Actor       | 普通区或特殊区保险箱生成点                              |
| BP_PickupSpawnPoint   | SpawnPoints               | Actor       | 门禁卡生成点                                     |
| WBP_HUD               | Blueprints/UI             | User Widget | 主界面状态显示                                    |
| WBP_StatusPanel       | Blueprints/UI             | User Widget | B 键打开的简化状态界面                               |
| WBP_Settlement        | Blueprints/UI             | User Widget | 任务成功/失败结算界面                                |
| WBP_PauseMenu         | Blueprints/UI             | User Widget | Esc 暂停菜单                                   |

---

## 五、BP_RunManager 变量表

| 变量名                      | 类型                        |   默认值 | 用途                     |
| ------------------------ | ------------------------- | ----: | ---------------------- |
| TotalValue               | Integer                   |     0 | 当前收益                   |
| CurrentCapacity          | Integer                   |     0 | 当前背包容量                 |
| MaxCapacity              | Integer                   |    30 | 背包最大容量                 |
| AccessCardCount          | Integer                   |     0 | 门禁卡数量                  |
| bHasCoreDrive            | Boolean                   | false | 是否获得核心硬盘               |
| CurrentAlert             | Integer                   |     0 | 当前警戒值                  |
| CurrentAlertStage        | E_AlertStage              |  Safe | 当前警戒阶段                 |
| OpenedSafeCount          | Integer                   |     0 | 已开启保险箱数量               |
| OpenedSpecialSafeCount   | Integer                   |     0 | 已开启特殊区保险箱数量，用于第 10 个保底 |
| LaserTriggerCount        | Integer                   |     0 | 激光触发次数                 |
| CameraDetectedCount      | Integer                   |     0 | 摄像头发现次数                |
| RemainingExtractionCount | Integer                   |     0 | 剩余开放撤离点数量              |
| bCountdownStarted        | Boolean                   | false | 是否已经启动撤离倒计时            |
| EvacRemainingSeconds     | Integer                   |   180 | 撤离倒计时剩余秒数              |
| bClosedAt70              | Boolean                   | false | 70 警戒阈值是否已经关闭过撤离点      |
| bClosedAt80              | Boolean                   | false | 80 警戒阈值是否已经关闭过撤离点      |
| bClosedAt90              | Boolean                   | false | 90 警戒阈值是否已经执行过仅保留最后撤离点 |
| bMissionEnded            | Boolean                   | false | 任务是否已经结束               |
| RunStartTime             | Float                     |     0 | 本局开始时间                 |
| SpawnManagerRef          | BP_RandomSpawnManager Ref |  None | 随机刷新管理器引用              |
| CountdownTimerHandle     | Timer Handle              |  None | 撤离倒计时句柄                |

---

## 六、BP_RunManager 事件调度器表

| 调度器名           | 参数                                     | 用途                           |
| -------------- | -------------------------------------- | ---------------------------- |
| OnStatsChanged | 无                                      | 收益、容量、门禁卡、核心硬盘、撤离点数量变化时通知 UI |
| OnAlertChanged | NewAlert：Integer；NewStage：E_AlertStage | 警戒值或警戒阶段变化时通知 UI 和警戒灯        |
| OnTimerChanged | RemainingSeconds：Integer               | 撤离倒计时变化时通知 UI                |
| OnMessage      | Message：Text                           | 显示系统提示                       |
| OnMissionEnded | bSuccess：Boolean；RatingText：Text       | 任务成功或失败时通知结算 UI              |

---

## 七、BP_RunManager 事件 / 函数表

| 事件 / 函数名               | 类型           | 输入                   | 输出                   | 主要逻辑                                                  |
| ---------------------- | ------------ | -------------------- | -------------------- | ----------------------------------------------------- |
| Event BeginPlay        | Event        | 无                    | 无                    | 记录开始时间，获取 BP_RandomSpawnManager，调用 InitializeRun，刷新状态 |
| GetLootInfo            | Function     | LootType：E_LootType  | LootInfo：ST_LootInfo | 根据物品类型返回显示名、收益、容量                                     |
| CanAddLoot             | Function     | LootType：E_LootType  | bCanAdd：Boolean      | 判断当前容量是否还能装下该收益类型                                     |
| ApplySafeReward        | Function     | SafeBoxType、LootType | bSuccess：Boolean     | 应用开箱结果，增加收益/容量/门禁卡/核心硬盘/开箱数/警戒值                       |
| AddAlert               | Function     | AddValue：Integer     | 无                    | 增加警戒值，处理 60/70/80/90/100 阈值                           |
| UpdateAlertStage       | Function     | 无                    | 无                    | 根据警戒值更新 E_AlertStage                                  |
| StartEvacCountdown     | Function     | 无                    | 无                    | 首次达到 60 警戒值后启动 180 秒倒计时                               |
| EvacCountdownTick      | Custom Event | 无                    | 无                    | 每秒减少撤离倒计时，到 0 后任务失败                                   |
| AddAccessCard          | Function     | Count：Integer        | 无                    | 增加门禁卡数量                                               |
| UseAccessCard          | Function     | 无                    | bUsed：Boolean        | 如果门禁卡数量大于 0，则消耗 1 张                                   |
| RegisterLaserTriggered | Function     | 无                    | 无                    | 激光触发次数 +1，警戒值 +5                                      |
| RegisterCameraDetected | Function     | 无                    | 无                    | 摄像头发现次数 +1，警戒值 +10                                    |
| FinishMission          | Function     | bSuccess：Boolean     | 无                    | 结束任务，清除倒计时，计算评价，通知结算 UI                               |
| CalculateRating        | Function     | bSuccess：Boolean     | RatingText：Text      | 按评价优先级返回评价等级                                      |

---

## 八、BP_RandomSpawnManager 变量表

| 变量名                    | 类型                             |  默认值 | 用途               |
| ---------------------- | ------------------------------ | ---: | ---------------- |
| NormalSafeSpawnChance  | Float                          |  0.7 | 普通保险箱点位生成普通保险箱概率 |
| AccessCardSpawnChance  | Float                          |   可调 | 门禁卡点位生成门禁卡概率     |
| LaserActiveChance      | Float                          |   可调 | 激光随机启用概率         |
| CameraActiveChance     | Float                          |   可调 | 摄像头随机启用概率        |
| ExtractionOpenCount    | Integer                        |    4 | 初始开放撤离点数量        |
| NormalSafeSpawnPoints  | BP_SafeBoxSpawnPoint Ref Array |    空 | 普通区保险箱点位数组       |
| SpecialSafeSpawnPoints | BP_SafeBoxSpawnPoint Ref Array |    空 | 特殊区保险箱点位数组       |
| AccessCardSpawnPoints  | BP_PickupSpawnPoint Ref Array  |    空 | 门禁卡点位数组          |
| LaserTraps             | BP_LaserTrap Ref Array         |    空 | 激光机关数组           |
| CameraTraps            | BP_CameraTrap Ref Array        |    空 | 摄像头机关数组          |
| ExtractionPoints       | BP_ExtractionPoint Ref Array   |    空 | 全部撤离点数组          |
| OpenExtractionPoints   | BP_ExtractionPoint Ref Array   |    空 | 当前开放撤离点数组        |
| RunManagerRef          | BP_RunManager Ref              | None | 全局运行管理器引用        |

---

## 九、BP_RandomSpawnManager 事件 / 函数表

| 事件 / 函数名                  | 类型             | 输入              | 输出 | 主要逻辑                                   |
| ------------------------- | -------------- | --------------- | -- | -------------------------------------- |
| InitializeRun             | Function       | 无               | 无  | 获取 RunManager，依次执行收集点位、随机生成、随机激活、撤离点开放 |
| CollectSpawnPoints        | Function       | 无               | 无  | 获取所有保险箱点位、门禁卡点位、激光、摄像头、撤离点             |
| SpawnNormalSafes          | Function       | 无               | 无  | 普通保险箱点位 70% 生成普通保险箱                    |
| SpawnAccessCards          | Function       | 无               | 无  | 随机生成门禁卡，并保证至少 1 张                      |
| ActivateLaserTraps        | Function       | 无               | 无  | 随机启用部分激光机关                             |
| ActivateCameraTraps       | Function       | 无               | 无  | 随机启用部分摄像头                              |
| SetupExtractions          | Function       | 无               | 无  | 从 6 个撤离点中随机开启 4 个，其余关闭                 |
| SpawnSpecialAreaSafes     | Function       | 无               | 无  | 特殊区点位 20% 不生成、60% 加密保险箱、20% 高级保险箱      |
| CloseRandomOpenExtraction | Function       | Count：Integer   | 无  | 从当前开放撤离点中随机关闭 Count 个                  |

---

## 十、BP_NightCatCharacter 变量表

| 变量名                  | 类型                |  默认值 | 用途           |
| -------------------- | ----------------- | ---: | ------------ |
| CurrentInteractActor | Actor Ref         | None | 当前射线命中的可交互对象 |
| RunManagerRef        | BP_RunManager Ref | None | 全局运行管理器引用    |
| HUDRef               | WBP_HUD Ref       | None | 主界面引用        |
| InteractionDistance  | Float             |  350 | 交互射线距离       |
| WalkSpeed            | Float             |  500 | 普通移动速度       |
| SprintSpeed          | Float             |  800 | 奔跑速度         |

---

## 十一、BP_NightCatCharacter 事件 / 函数表

| 事件 / 函数名            | 类型                 | 输入            | 输出 | 主要逻辑                                         |
| ------------------- | ------------------ | ------------- | -- | -------------------------------------------- |
| Event BeginPlay     | Event              | 无             | 无  | 添加输入映射，获取 RunManager，创建 WBP_HUD              |
| Event Tick          | Event              | Delta Seconds | 无  | 从摄像机发射射线，检测是否命中实现 BPI_Interactable 的对象       |
| IA_Interact         | Input Action Event | F             | 无  | 对 CurrentInteractActor 调用 Interact (Message) |
| IA_Sprint Started   | Input Action Event | Shift         | 无  | 设置移动速度为 SprintSpeed                          |
| IA_Sprint Completed | Input Action Event | Shift 松开      | 无  | 恢复 WalkSpeed                                 |
| IA_Status           | Input Action Event | B             | 无  | 调用 HUD 的 ToggleStatusPanel                   |
| IA_Pause            | Input Action Event | Esc           | 无  | 打开暂停菜单                                       |

---

## 十二、保险箱蓝图变量与函数表

### BP_SafeBox_Base 变量

| 变量名            | 类型                    |    默认值 | 用途         |
| -------------- | --------------------- | -----: | ---------- |
| SafeBoxType    | E_SafeBoxType         | Normal | 当前保险箱类型    |
| bOpened        | Boolean               |  false | 是否已经开启     |
| RunManagerRef  | BP_RunManager Ref     |   None | 全局运行管理器引用  |
| ClosedMaterial | Material Interface    |   None | 未开启材质      |
| OpenedMaterial | Material Interface    |   None | 已开启材质      |
| SafeMesh       | Static Mesh Component |     组件 | 保险箱模型      |
| PointLight     | Point Light Component |   组件 | 开启后颜色变化/提示 |

### BP_SafeBox_Base 事件 / 函数

| 事件 / 函数名        | 类型                 | 输入         | 输出                  | 主要逻辑                                                  |
| --------------- | ------------------ | ---------- | ------------------- | ----------------------------------------------------- |
| Event BeginPlay | Event              | 无          | 无                   | 获取 BP_RunManager                                      |
| Event Interact  | Interface Event    | Interactor | 无                   | 若未开启，则 RollLoot，ApplySafeReward 成功后设置 bOpened、改变材质/颜色 |
| GetInteractText | Interface Function | Interactor | Text                | 返回“按 F 开启普通/加密/高级保险箱”或“已开启”                           |
| RollLoot        | Function           | 无          | LootType：E_LootType | 按保险箱类型和概率返回产出结果，并处理核心硬盘保底                             |
| 改变开启表现          | 节点               | 无          | 无                   | 简化开门动画为 Set Material / Set Light Color               |

---

## 十三、交互物蓝图变量与函数表

### BP_AccessCardPickup

| 类别              | 内容                                              |
| --------------- | ----------------------------------------------- |
| 主要变量            | 简化了玩法变量，后续加入 CardMesh、PointLight 等                  |
| Event Interact  | 调用 RunManager.AddAccessCard(1)，然后 Destroy Actor |
| GetInteractText | 返回“按 F 拾取门禁卡”                                   |

### BP_AccessDoor

| 变量名           | 类型                    |   默认值 | 用途        |
| ------------- | --------------------- | ----: | --------- |
| bOpened       | Boolean               | false | 门是否已经开启   |
| RunManagerRef | BP_RunManager Ref     |  None | 全局运行管理器引用 |
| DoorMesh      | Static Mesh Component |    组件 | 门禁门模型     |

| 事件 / 函数名        | 类型                 | 主要逻辑                                    |
| --------------- | ------------------ | --------------------------------------- |
| Event BeginPlay | Event              | 获取 RunManager                           |
| Event Interact  | Interface Event    | 如果有门禁卡，UseAccessCard 成功后开启门；否则提示“需要门禁卡” |
| GetInteractText | Interface Function | 有门禁卡返回“按 F 使用门禁卡开启特殊区”，没有则返回“需要门禁卡”     |

说明：这里开门可以考虑简化为隐藏门

### BP_ExtractionPoint

| 变量名            | 类型                      |   默认值 | 用途        |
| -------------- | ----------------------- | ----: | --------- |
| bIsOpen        | Boolean                 | false | 撤离点是否开放   |
| RunManagerRef  | BP_RunManager Ref       |  None | 全局运行管理器引用 |
| OpenMaterial   | Material Interface      |  None | 开放状态材质    |
| ClosedMaterial | Material Interface      |  None | 关闭状态材质    |
| PortalMesh     | Static Mesh Component   |    组件 | 撤离点模型     |
| PointLight     | Point Light Component   |    组件 | 开放时发红光    |
| ExtractionIcon | Widget Component / Mesh |    可选 | 撤离图标      |

| 事件 / 函数名        | 类型                 | 主要逻辑                                                   |
| --------------- | ------------------ | ------------------------------------------------------ |
| Event BeginPlay | Event              | 获取 RunManager                                          |
| Event Interact  | Interface Event    | bIsOpen 且 bHasCoreDrive 为 true 时 FinishMission(true)   |
| GetInteractText | Interface Function | 未开放返回“该撤离点已封锁”；未获得核心硬盘返回“尚未获得核心硬盘，无法撤离”；满足条件返回“按 F 撤离” |

---

## 十四、机关蓝图变量与函数表

### BP_LaserTrap

| 变量名             | 类型                              |   默认值 | 用途        |
| --------------- | ------------------------------- | ----: | --------- |
| bActive         | Boolean                         | false | 本局是否启用    |
| bCanTrigger     | Boolean                         |  true | 是否可再次触发   |
| CooldownSeconds | Float                           |   1.0 | 触发冷却      |
| RunManagerRef   | BP_RunManager Ref               |  None | 全局运行管理器引用 |
| LaserBeam       | Static Mesh / Niagara Component |    组件 | 激光表现      |
| TriggerBox      | Box Collision                   |    组件 | 玩家碰撞检测    |

| 事件 / 函数名                | 类型              | 主要逻辑                                  |
| ----------------------- | --------------- | ------------------------------------- |
| Event BeginPlay         | Event           | 获取 RunManager，按 bActive 设置表现          |
| SetActive    | Function | 设置 bActive，显示/隐藏激光，开启/关闭碰撞            |
| TriggerBox BeginOverlap | Component Event | 玩家碰到激光后 RegisterLaserTriggered，警戒值 +5 |

### BP_CameraTrap

| 变量名             | 类型                       |   默认值 | 用途          |
| --------------- | ------------------------ | ----: | ----------- |
| bActive         | Boolean                  | false | 本局是否启用      |
| bPlayerInZone   | Boolean                  | false | 玩家是否在扫描范围内  |
| bCooldown       | Boolean                  | false | 是否处于冷却      |
| DetectionDelay  | Float                    |   0.3 | 玩家停留检测时间    |
| CooldownSeconds | Float                    |   3.0 | 摄像头发现后的冷却时间 |
| DetectedPlayer  | BP_NightCatCharacter Ref |  None | 当前检测到的玩家    |
| RunManagerRef   | BP_RunManager Ref        |  None | 全局运行管理器引用   |
| ScanArea        | Box Collision            |    组件 | 摄像头扫描触发区域   |
| ScanConeMesh    | Static Mesh Component    |    组件 | 扫描范围可视化模型   |
| CameraMesh      | Static Mesh Component    |    组件 | 摄像头模型       |

| 事件 / 函数名              | 类型              | 主要逻辑                                                     |
| --------------------- | --------------- | -------------------------------------------------------- |
| Event BeginPlay       | Event           | 获取 RunManager，按 bActive 设置表现                             |
| SetActive  | Function | 设置 bActive，显示/隐藏扫描范围，开启/关闭碰撞                             |
| ScanArea BeginOverlap | Component Event | 玩家进入扫描范围后，0.3 秒后执行 ConfirmCameraDetect                   |
| ScanArea EndOverlap   | Component Event | 如果 Other Actor == DetectedPlayer，则 bPlayerInZone = false |
| ConfirmCameraDetect   | Custom Event    | 玩家仍在区域内且不在冷却时，RegisterCameraDetected，警戒值 +10             |
| ResetCameraCooldown   | Custom Event    | 3 秒后解除冷却，恢复扫描材质                                          |

### BP_AlertLight

| 变量名                    | 类型                |   默认值 | 用途     |
| ---------------------- | ----------------- | ----: | ------ |
| RunManagerRef          | BP_RunManager Ref |  None | 绑定警戒变化 |
| PointLight / RectLight | Light Component   |    组件 | 警戒灯    |
| bBlinking              | Boolean           | false | 是否闪烁   |

| 事件 / 函数名           | 类型           | 主要逻辑                            |
| ------------------ | ------------ | ------------------------------- |
| Event BeginPlay    | Event        | 获取 RunManager，绑定 OnAlertChanged |
| UpdateLightByAlert | Custom Event | 0～29 蓝紫色，30～59 黄色，60 以上红色闪烁     |

---

## 十五、SpawnPoint 点位蓝图表

| 蓝图名                  | 变量                     | 用途                               |
| -------------------- | ---------------------- | -------------------------------- |
| BP_SafeBoxSpawnPoint | bIsSpecialArea：Boolean | false 表示普通区保险箱点位；true 表示特殊区保险箱点位 |
| BP_PickupSpawnPoint  | 无核心变量                  | 门禁卡随机生成点                         |

---

## 十六、UI 蓝图变量与函数表

### WBP_HUD 变量

| 变量名            | 类型                  | 用途                          |
| -------------- | ------------------- | --------------------------- |
| RunManagerRef  | BP_RunManager Ref   | 读取收益、警戒值、门禁卡、核心硬盘、撤离点数量、倒计时 |
| StatusPanelRef | WBP_StatusPanel Ref | 控制 B 键状态面板打开/关闭             |

### WBP_HUD 控件

| 控件名                 | 类型          | 显示内容         |
| ------------------- | ----------- | ------------ |
| TXT_Task            | TextBlock   | 任务：寻找核心硬盘并撤离 |
| TXT_Value           | TextBlock   | 收益           |
| TXT_Alert           | TextBlock   | 警戒值          |
| PB_Alert            | ProgressBar | 警戒值进度，可选     |
| TXT_AccessCard      | TextBlock   | 门禁卡数量        |
| TXT_CoreDrive       | TextBlock   | 核心硬盘状态       |
| TXT_ExtractionCount | TextBlock   | 剩余撤离点数量      |
| TXT_Timer           | TextBlock   | 撤离倒计时        |
| TXT_InteractPrompt  | TextBlock   | 交互提示         |

### WBP_HUD 事件 / 函数

| 事件 / 函数名           | 类型                      | 输入                  | 主要逻辑                                                                                   |
| ------------------ | ----------------------- | ------------------- | -------------------------------------------------------------------------------------- |
| Event Construct    | Event                   | 无                   | 获取 RunManager，绑定 OnStatsChanged、OnAlertChanged、OnTimerChanged、OnMessage、OnMissionEnded |
| RefreshStats       | Function                | 无                   | 刷新任务、收益、门禁卡、核心硬盘、剩余撤离点                                                                 |
| RefreshAlert       | Function                | NewAlert、NewStage   | 刷新警戒值文字和进度条                                                                            |
| RefreshTimer       | Function                | RemainingSeconds    | 刷新撤离倒计时                                                                                |
| SetInteractionText | Function                | Text                | 设置交互提示文本                                                                               |
| ShowMessage        | Function                | Message             | 显示系统提示                                                                                 |
| ToggleStatusPanel  | Function                | 无                   | 打开或关闭 WBP_StatusPanel                                                                  |
| ShowSettlement     | Function / Custom Event | bSuccess、RatingText | 创建 WBP_Settlement 并显示结算界面                                                              |

### WBP_StatusPanel 控件

| 控件名                 | 类型        | 显示内容                             |
| ------------------- | --------- | -------------------------------- |
| TXT_CurrentValue    | TextBlock | 当前收益                             |
| TXT_Capacity        | TextBlock | 容量：CurrentCapacity / MaxCapacity |
| TXT_AccessCard      | TextBlock | 门禁卡数量                            |
| TXT_CoreDrive       | TextBlock | 核心硬盘状态                           |
| TXT_OpenedSafeCount | TextBlock | 已开启保险箱数量                         |
| TXT_CurrentAlert    | TextBlock | 当前警戒值                            |

### WBP_Settlement 控件

| 控件名                | 类型        | 显示内容        |
| ------------------ | --------- | ----------- |
| TXT_Result         | TextBlock | 任务成功 / 任务失败 |
| TXT_Rating         | TextBlock | 评价等级        |
| TXT_FinalValue     | TextBlock | 最终收益        |
| TXT_CoreDrive      | TextBlock | 是否获得核心硬盘    |
| TXT_FinalAlert     | TextBlock | 最终警戒值       |
| TXT_OpenedSafes    | TextBlock | 开启保险箱数量     |
| TXT_LaserCount     | TextBlock | 触发激光次数      |
| TXT_CameraCount    | TextBlock | 被摄像头发现次数    |
| TXT_ExtractionLeft | TextBlock | 剩余撤离点数量     |
| TXT_TimeUsed       | TextBlock | 用时          |
| BTN_Restart        | Button    | 重新开始        |
| BTN_Quit           | Button    | 退出          |

说明：主界面、状态界面、结算界面简化为只显示核心状态，不做复杂背包格和物品详情。

---

## 十七、Mesh 模型表

| Mesh 名称                  | 类型          | 对应蓝图 / 用途            | 说明             |
| ------------------------ | ----------- | -------------------- | -------------- |
| SM_Wall_Modular          | Static Mesh | 地图迷宫                 | 模块化墙体          |
| SM_Floor_Tile            | Static Mesh | 地图地板                 | 模块化地板          |
| SM_Ceiling_Tile          | Static Mesh | 地图天花                 | 模块化天花板    |
| SM_SafeBox_Normal        | Static Mesh | BP_SafeBox_Normal    | 普通保险箱          |
| SM_SafeBox_Encrypted     | Static Mesh | BP_SafeBox_Encrypted | 加密保险箱          |
| SM_SafeBox_Advanced      | Static Mesh | BP_SafeBox_Advanced  | 高级保险箱          |
| SM_AccessCard            | Static Mesh | BP_AccessCardPickup  | 门禁卡            |
| SM_AccessDoor            | Static Mesh | BP_AccessDoor        | 门禁特殊区入口门       |
| SM_LaserEmitter          | Static Mesh | BP_LaserTrap         | 激光发射器          |
| SM_LaserBeam_Cylinder    | Static Mesh | BP_LaserTrap         | 激光束，使用细长圆柱或长方体 |
| SM_SecurityCamera        | Static Mesh | BP_CameraTrap        | 摄像头            |
| SM_CameraScanCone        | Static Mesh | BP_CameraTrap        | 摄像头扫描范围可视化     |
| SM_ExtractionPortal      | Static Mesh | BP_ExtractionPoint   | 撤离点主体          |
| SM_ExtractionIconPlane   | Static Mesh | BP_ExtractionPoint   | 撤离图标平面，可选      |
| SM_AlertLight            | Static Mesh | BP_AlertLight        | 警戒灯外壳          |
| SM_BankProp_ServerRack   | Static Mesh | 目前暂时为地图氛围，选做  | 数据银行服务器柜       |
| SM_BankProp_DataTerminal | Static Mesh | 目前暂时为地图氛围，选做  | 数据终端           |
| SM_BankProp_WarningSign  | Static Mesh | 目前暂时为地图氛围，选做  | 警戒标识           |

---

## 十八、Materials 材质表

| 材质名                     | 用途           | 建议表现        |
| ----------------------- | ------------ | ----------- |
| M_Wall_DarkTech         | 迷宫墙体         | 深灰、暗蓝、科技线条  |
| M_Floor_DarkTile        | 地板           | 暗色金属或深色瓷砖   |
| M_Ceiling_DarkPanel     | 天花板          | 暗色科技板       |
| M_Safe_Normal_Closed    | 普通保险箱未开启     | 蓝紫暗色        |
| M_Safe_Normal_Opened    | 普通保险箱已开启     | 亮蓝或变暗，表示已开启 |
| M_Safe_Encrypted_Closed | 加密保险箱未开启     | 紫色加密感       |
| M_Safe_Encrypted_Opened | 加密保险箱已开启     | 紫色变亮或变灰     |
| M_Safe_Advanced_Closed  | 高级保险箱未开启     | 金红色或高价值感    |
| M_Safe_Advanced_Opened  | 高级保险箱已开启     | 金红变暗或亮边消失   |
| M_AccessCard            | 门禁卡          | 蓝紫发光卡片      |
| M_AccessDoor_Closed     | 门禁门关闭        | 暗色门体，带门禁标识  |
| M_AccessDoor_Opened     | 门禁门开启        | 变暗或绿/蓝提示    |
| M_LaserBeam             | 激光束          | 红色自发光       |
| M_CameraCone_Blue       | 摄像头扫描范围      | 蓝色半透明       |
| M_CameraCone_Red        | 摄像头发现 / 冷却提示 | 红色半透明       |
| M_Extraction_Open       | 开放撤离点        | 红色自发光       |
| M_Extraction_Closed     | 封锁撤离点        | 灰暗或暗红       |
| M_AlertLight_BluePurple | 安全阶段警戒灯      | 蓝紫色灯光       |
| M_AlertLight_Yellow     | 警觉阶段警戒灯      | 黄色灯光        |
| M_AlertLight_Red        | 警报阶段警戒灯      | 红色闪烁        |
| M_UI_DarkPanel          | UI 背板        | 黑色半透明       |
| M_UI_AlertRed           | UI 警报提示      | 红色警报色       |
| M_UI_NeonBlue           | UI 普通科技线条    | 蓝紫色         |

---

## 十九、Textures 纹理表

| 纹理名                      | 类型              | 用途             |
| ------------------------ | --------------- | -------------- |
| T_Icon_AccessCard        | UI Icon         | 门禁卡图标          |
| T_Icon_CoreDrive         | UI Icon         | 核心数据硬盘图标       |
| T_Icon_Extraction        | UI Icon         | 撤离点图标          |
| T_Icon_Extraction_Locked | UI Icon         | 撤离点封锁图标        |
| T_Icon_Alert             | UI Icon         | 警戒值图标          |
| T_Icon_Value             | UI Icon         | 收益图标           |
| T_Icon_SafeBox           | UI Icon         | 保险箱图标          |
| T_Icon_Timer             | UI Icon         | 撤离倒计时图标        |
| T_Decal_CatPawBank       | Decal           | 猫爪银行标识         |
| T_Decal_WarningLine      | Decal           | 警戒线 / 危险区域地贴   |
| T_Decal_AccessArea       | Decal           | 门禁特殊区标识        |
| T_Decal_ExtractionMark   | Decal           | 撤离点地面标识        |
| T_Decal_DataCircuit      | Decal / Texture | 数据银行科技线路纹理     |
| T_UI_PanelGrid           | UI Texture      | HUD 背景网格       |
| T_UI_DarkGradient        | UI Texture      | UI 暗色渐变底       |
| T_UI_RedFlash            | UI Texture      | 被激光/摄像头触发时红色闪屏 |

---

## 二十、VFX 特效表

| VFX 名称                   | 类型                               | 对应蓝图 / 用途           | 说明                         |
| ------------------------ | -------------------------------- | ------------------- | -------------------------- |
| NS_LaserBeam             | Niagara System / 可选              | BP_LaserTrap        | 激光束效果；也可用静态 Mesh + 自发光材质替代 |
| NS_LaserTriggerFlash     | Niagara System / UI Flash        | BP_LaserTrap        | 玩家触发激光时红色闪烁                |
| NS_CameraScan            | Niagara System / 可选              | BP_CameraTrap       | 摄像头扫描感；也可用 ScanConeMesh 替代 |
| NS_CameraDetectFlash     | Niagara System / UI Flash        | BP_CameraTrap       | 摄像头发现玩家时提示                 |
| NS_AccessCardGlow        | Niagara System                   | BP_AccessCardPickup | 门禁卡悬浮发光                    |
| NS_SafeOpenSpark         | Niagara System / 可选              | BP_SafeBox_Base     | 保险箱开启瞬间火花或光效               |
| NS_ExtractionAura        | Niagara System                   | BP_ExtractionPoint  | 开放撤离点光圈                    |
| NS_ExtractionClosedSmoke | Niagara System / 可选              | BP_ExtractionPoint  | 撤离点关闭时短暂封锁效果               |
| NS_AlertRedBlink         | Niagara System / Light Animation | BP_AlertLight       | 警戒值 60 以上红色闪烁              |
| NS_CoreDriveObtain       | Niagara System / 可选              | BP_SafeBox_Base     | 获得核心硬盘时的特殊提示效果             |

优先做的 VFX：

```text
NS_ExtractionAura
NS_LaserTriggerFlash
NS_CameraDetectFlash
NS_AlertRedBlink
```

其他 VFX 后期（或本学期课程项目结束后）补。

---

## 二十一、课程 Demo 资产清单

本课程阶段计划 Demo 跑通的资产如下（按照课程进度可能做不完，尽量把优先级高的做了）：

| 类别          | 必需资产                                                                       |
| ----------- | -------------------------------------------------------------------------- |
| Data        | E_SafeBoxType、E_LootType、E_AlertStage、ST_LootInfo                          |
| Interface   | BPI_Interactable                                                           |
| Core        | BP_RunManager、BP_RandomSpawnManager                                        |
| Character   | BP_NightCatCharacter                                                       |
| SafeBoxes   | BP_SafeBox_Base、BP_SafeBox_Normal、BP_SafeBox_Encrypted、BP_SafeBox_Advanced |
| Pickups     | BP_AccessCardPickup                                                        |
| Doors       | BP_AccessDoor                                                              |
| Extractions | BP_ExtractionPoint                                                         |
| Traps       | BP_LaserTrap、BP_CameraTrap、BP_AlertLight                                   |
| SpawnPoints | BP_SafeBoxSpawnPoint、BP_PickupSpawnPoint                                   |
| UI          | WBP_HUD、WBP_StatusPanel、WBP_Settlement、WBP_PauseMenu                       |
| Mesh        | 墙、地板、三种保险箱、门禁卡、门、激光、摄像头、撤离点、警戒灯                                            |
| Material    | 三种保险箱开启/未开启材质、门禁卡材质、激光材质、摄像头扫描材质、撤离点开放/关闭材质、警戒灯材质                          |
| Texture     | 门禁卡、核心硬盘、撤离、警戒、收益、保险箱、倒计时 UI 图标                                            |
| VFX         | 撤离点光圈、激光触发闪屏、摄像头发现提示、警戒灯红色闪烁                                               |
