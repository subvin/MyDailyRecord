# MyDailyRecord

### 仓库说明

#### 2015-12-26

1.车载项目中，切换模式到TF卡模式时，如果先暂停本地播放器播放，会发一个通知出来重新设置模式为A2DP，导致模式切了TF卡模式之后又重新跳转到A2DP模式。
2.车载项目或者其他蓝牙项目中，如果从TF卡列表切换到播放器界面（这两个界面设置的监听（或者代理）一致），如果前一个界面已经从蓝牙设备的TF卡中获取了歌曲，则在播放器列表界面就不该再次获取（触发回调函数回调来时，判断即可）。如果获取，哪怕只有一次，在切换这两个界面的过程中，很容易导致设备死机，整体失灵。
