## IOS-MESH-SDK

### 概述

MTC SDK开发包（MeshSDK）使用苹果的蓝牙协议，提供了蓝牙数据扫描、APP唤醒、广播蓝牙数据，并支持配置指定设备参数等API。你可以访问MTC官网（http://www.mtc.io）了解更多信息，加入MTC社群交流我们软硬件相关问题。

MTC SDK开发包需要手持设备硬件支持蓝牙4.0及其以上，并要求系统版本至少IOS7及其以上。 附：支持的IOS设备列表 iphone4s及以上、 itouch5及以上、 iPad3及以上、 iPad mini均可以 其他详情见：http://en.wikipedia.org/wiki/List_of_iOS_devices

### 集成指南
- 打开Info.plist添加key：NSLocationAlwaysUsageDescription或者NSLocationWhenInUseUsageDescription，（填写描述如：用于Mesh唤醒）
- 打开Info.plist添加key：UIBackgroundModes(bluetooth-central、bluetooth-peripheral)，添加后台使用蓝牙传输权限

### 常用API

- 设置监听唤醒处理回调

```
WakeUpManager
- (void)monitorMeshWakeUp:(CLBeaconRegion *)region;
- (void)stopMonitor:(CLBeaconRegion *)region;
* WakeUpManager处理类，需软件启动即初始化，如appDelegate;
```
```
* 请放置以下回调到 唤醒处理类，此类必须是随APP启动，否则无法处理后台唤醒事件
* 唤醒失败回调
- (void)wakeUpManager:(WakeUpManager *)manager monitoringDidFailForRegion:(CLBeaconRegion *)region withError:(NSError *)error;
* 进入唤醒区域回调
* - (void)wakeUpManager:(WakeUpManager *)manager didEnterRegion:(CLBeaconRegion *)region;
* 离开唤醒区域回调
- (void)wakeUpManager:(WakeUpManager *)manager didExitRegion:(CLBeaconRegion *)region;
* 锁屏唤醒区域检测
- (void)wakeUpManager:(WakeUpManager *)manager didDetermineState:(CLRegionState)state forRegion:(CLBeaconRegion *)region;
```

- 广播唤醒数据

```
BLEBroadcast
设置唤醒区域
- (void)meshWakeUp:(CLBeaconRegion *)region;
- (void)stopMeshWakeUp:(CLBeaconRegion *)region;
开启
- (BOOL) start;
```

- 唤醒Region设置

```
//唤醒接入示例
     CLBeaconRegion* region = [[CLBeaconRegion alloc] initWithProximityUUID:@"用于唤醒此设备的UUID" identifier:@"区域标识符，用于覆盖、或停止已有区域"];
    region.notifyOnEntry = YES;//监听进入区域
    region.notifyOnExit = YES;//离开区域时回调
    region.notifyEntryStateOnDisplay = YES;//锁屏唤醒时，是否立即扫描区域
    [WakeUpManager monitorMeshWakeUp:region];

```

- 广播普通数据

```
BLEBroadcast
设置广播数据
- (BOOL)setMeshCast:(CBUUID *)uuid data:(NSData *)data;
- (void)removeMeshCast:(CBUUID *)uuid;
开启
- (BOOL) start;
```

- 接收普通数据

```
BLEScanner
开启接收数据
- (BOOL) start;
- 数据接收返回
- (void)bleScanner:(BLEScanner*)scanner  didDiscoverUUID:(CBUUID *)uuid advertisementData:(NSData *)advertisementData RSSI:(NSNumber *)RSSI;
```

