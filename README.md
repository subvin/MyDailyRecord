# MyDailyRecord

### 仓库说明

#### 2016-1-12

1.前几天在替换喜马拉雅SDK的时候，AFNetworking中的一个类AFURLConnectionOperation或者（AFHttpClient）中有一个Block，第一个参数是，id类型的一个形参，它调了一个 isFinished 的方法 Multible methods miss match isFinished 我选中isFinished方法，点击右键，发现有两个类方法冲突，一个是XMAlbum,一个就是当前类本身，显然这个AF和喜马拉雅的网络请求产生了冲突。找遍了百度、stackOverFlow，也没找到问题在哪，问了下永何，它的AF比较新，我的是比较老的。他的没报这个问题，于是我试着替换一下第三方库。等换好了以后发现，新的第三方库是没有这个AFHttpClient这个类的，而我的所有的网络请求类都是直接或者间接的继承于这个类，这意味着我要把这些所有的网络请求都废除掉，取用最新的AF 第三方框架，可惜了没用cocoaPods,要不然就不用这个麻烦了，好不容易把所有相关的网络请求类都给移除了，但是在AFURLConnectionOperation这个类中既然还会报这样的错误，这下我真的受伤了，被逼无奈啊，难道非得让我把整个AF给弄通透了吗？好累，替换个SDk还能出这样的问题，想把出错的这个方法看了一下，最终，把isFinished的方法，对象换成了该类对象，暂时不报错了，运行起来也没问题，暂时OK，但不知道以后会不会有问题。。有待完善。

#### 2016-1-2

1.上传APP store 证书不匹配问题   WARNING ITMS-90076: "Potential Loss of Keychain Access. The previous version of software has an application-identifier value of ['GR3FK25RUR.com.chipsguide.app.carmp3.sagehuman.general'] and the new version of software being submitted has an application-identifier of ['AXTESR7SA4.com.chipsguide.app.carmp3.sagehuman.general']. This will result in a loss of keychain access."报这样的错误,一定要复查自己的所选择的证书和齿轮文件是否选对，一般都是选不对而导致的问题，

2.Xcode中开发者、配置文件都选对了依然报错的BUG     在解决上面所犯的错误的条件下，证书齿轮文件都选对了，团队也选对了，但是Xcode 还是会让我我们fix isues 一下，修复就修复吧，谁知，修复了以后还是没法用，什么都是对的，但就是不能用，这时候我们需要把本地的齿轮文件，通通删掉（至少得把这个对的齿轮文件给删掉吧，猜的 ，没试验过），重新选择开发者，重新下载配置文件（齿轮文件），然后就不报错了。


#### 2015-12-31

1.重复上传应用问题   今天在上传时捷E酷行得时候，等了好久，后台那边一直没有更新iPa到达的信息，等了将近一个小时吧，终于忍不住想重新上传一次，等到我重新上传到额一次的时候，报了这样的一个错误ERROR ITMS-4238: "Redundant Binary Upload. There already exists a binary upload with build version '1.20' for train '1.20'" atSoftwareAssets/PreReleaseSoftwareAsset，看来是我多心了，已经存在这个版本了，没必要再上传一次了。可是等了好久还是没上传过去。.....

2.mac 地址问题忘了添加的问题  等了将近另个小时多了，应用还没传上去，于是打开了应用，尽然发现，应用居然可以连着其它的设备，比如iLight，尼玛，这下麻烦大了，刚才把mac 去掉了一位，现在可以连很多设备了，居然忘了这一点，欲哭无泪，只是发现爱车听没这个问题，千算万算居然忘了这一点，只能撤销或者改变版本号， 再上传了，。

#### 2015-12-30

#### 2015-12-29

#### 2015-12-28

#### 2015-12-27

#### 2015-12-26

1.蓝牙模式自动切换的坑   模式车载项目中，切换模式到TF卡模式时，如果先暂停本地播放器播放，会发一个通知出来重新设置模式为A2DP，导致模式切了TF卡模式之后又重新跳转到A2DP模式。

2.蓝牙设备容易死机的问题  车载项目或者其他蓝牙项目中，如果从TF卡列表切换到播放器界面（这两个界面设置的监听（或者代理）一致），如果前一个界面已经从蓝牙设备的TF卡中获取了歌曲，则在播放器列表界面就不该再次获取（触发回调函数回调来时，判断即可）。如果获取，哪怕只有一次，在切换这两个界面的过程中，很容易导致设备死机，整体失灵。


