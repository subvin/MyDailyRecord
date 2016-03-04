# MyDailyRecord

### 仓库说明

#### 2016-03-04

1、图片目录结构不对导致有时候图片不显示的问题

问题分析，朗琴项目灯效页面的五个效果图片，第五张图片的普通状态一直不显示，高亮状态没有问题，英文状态下没有这个问题，正常，可是到了在中文状态下，这个问题就凸现出来得了，看了代码，写的没有任何问题，就是不出现图片，很奇怪。

解决方法：用其它图片替换了一下 ，显示出来了，再找了下那张图片的路径和不显示的图片的路径，惊奇的发现这个图片不是在同一个相同的文件夹下，把不显示的图片拖到项目工程图片主目录，OK!问题得到解决。

2、类型安全转换的BUG

问题描述：宝宝点读已读书籍列表界面，需要对书籍的阅读次数对书籍进行从大到小的排序，由于对iOS里面的枚举器排序用得不好，怎么用的不管用，最后选择了用最原始最简单的排序---选择排序进行。原先代码是这样的
//选择排序
            NSMutableArray *array = [[NSMutableArray arrayWithArray:object] mutableCopy];
            int index=0;
            for (int i = 0 ; i < array.count - 1; i ++) {
                index = i;
                for (int j = i + 1; j < array.count; j ++) {
                    TRPNoteMainModel *obj1 = array[index];
                    TRPNoteMainModel *obj2 = array[j];
                    if ([obj1.readcount intValue] < [obj2.readcount intValue]) {
                        index = j;
                    }
                }
                if (index != i) {
                    [array exchangeObjectAtIndex:index withObjectAtIndex:i];
                }
            }
新注册了一个新的账号，由于还没有阅读任何书籍，所得到的array的长度为0，加载图标停住了，然后应用再也走不了了。

问题解决：经过断点调试，发现第二层（里面那层）怎么都走不到那里去，外面的循环怎么都出不来；最后我是这么解决的
NSMutableArray *array = [[NSMutableArray arrayWithArray:object] mutableCopy];
            //选择排序
            int index = 0;
            int count = (int)array.count;
            for (int i = 0 ; i < count - 1; i ++) {
                index = i;
                for (int j = i + 1; j < count; j ++) {
                    TRPNoteMainModel *obj1 = array[index];
                    TRPNoteMainModel *obj2 = array[j];
                    if ([obj1.readcount intValue] < [obj2.readcount intValue]) {
                        index = j;
                    }
                }
                if (index != i) {
                    [array exchangeObjectAtIndex:index withObjectAtIndex:i];
                }
            }
            
解释：array.count 是一个无符号的long类型 array.count = 0,在内存中四个字节全部都是0，其中第一位不是符号位，减掉 1 就是加上（-1），而（-1）在内存中的各个比特都是1，当执行 array.count - 1 的时候，这个结果相减的结果会变成一个最大的无符号数，然后这个循环就一直的进行,知道执行完这个最大的数的次数后才终止。
鉴于此，在往后的编码规范中应该多注意点类型安全的转换，


#### 2016-01-19

1.宝宝点读中 3D 书籍切换到2D界面时，2D界面概率性横屏的问题。

问题分析：  由于有时候在跳转界面的时候，都用了两种弹出方式即:（导航和模态）,所有从导航推过去的页面再从3D界面返回来时，都不会出现屏幕方向出错的问题，而从模态弹出的界面进到3D界面再返回来时，必然会导致转屏出错，原因是因为集成的Unity在返回2D界面时，只是针对Unity的界面和2D界面上的根界面进行切换和旋转屏幕的处理，并未处理其他之外的根界面，而模态弹出切换了新的根视图,所以从3D界面返回到2D界面的时候会出现旋转屏幕方向不对的问题。

解决方案：综上所述，得出了两套解决方案  一、到Unity跳转到2D这块的逻辑再做处理（个人自己认为不大好完成）；
二、是把之前的模态弹出都变成导航，这样就不会有这样的问题了，后来发现安卓的也是这样的效果，于是便这样处理了。等有时间一定好好看看Unity相关的东西。

2.优化点读笔切换点读3D 书籍到2D书籍的流畅性。

问题分析：切换3D界面到2D界面时，直接切换了根视图控制器，之前从3D界面切换到2D界面时，需要点两下第一下，跳到其它的根视图，第二下跳到了2D书籍界面，或是点击2D书籍的时候按久一点（其实本质是点了多次），之前用了Block等方法都没实现直接点击了以后，是由于没找到一种好的方式使得界面切换到2D界面后，再自动切换到其它界面的方法，所以之前的版本都是点久一点。

解决方案：定义可一个全局布尔变量，以及一个2D数据数据模型，让所有的地方都可以访问，当要从3D书籍界面切换到2D书籍模块的时候，把这个模型赋值，在把这个布尔变量的值变为YES;当界面跳转到2D界面的时候，无论是在哪一个界面，因为这些界面都继承了同一个BaseViewController,在这个类的ViewDidAppear这个方法内部判断，如果该变量的值为YES，就改为NO，再跳转到2D界面，同时把模型也一并赋值过去，这样当跳转到2D界面的时候，如果需要跳转到2D书籍界面，就会自动跳转到相应的2D书籍界面，从而问题得到了解决，并且不会有后遗症，问题得到了很大的改善，流畅性也提高了不少。


#### 2016 -01-16

1.上传APP，到APPStore,没有证书的情况，报了这样的错误，

codesign /Users/Gaby/Library/Developer/Xcode/DerivedData/RoyalAppInspection-dthvtpxadkslqmhkwdpaqkyujscg/Build/Products/Debug-iphoneos/RoyalAppInspection.app
cd /Users/Gaby/Desktop/RoyalAppInspection
export CODESIGN_ALLOCATE=/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/codesign_allocate
export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin

/usr/bin/codesign --force --sign 79640A11C8D22589BD337496ABB8443581513846 --entitlements /Users/Gaby/Library/Developer/Xcode/DerivedData/RoyalAppInspection-dthvtpxadkslqmhkwdpaqkyujscg/Build/Intermediates/RoyalAppInspection.build/Debug-iphoneos/RoyalAppInspection.build/RoyalAppInspection.app.xcent /Users/Gaby/Library/Developer/Xcode/DerivedData/RoyalAppInspection-dthvtpxadkslqmhkwdpaqkyujscg/Build/Products/Debug-iphoneos/RoyalAppInspection.app

79640A11C8D22589BD337496ABB8443581513846: no identity found Command /usr/bin/codesign failed with exit code 1

问了百度。StackoverFlow ,好多人说，证书冲突了，如果该系统帐号下也有两个identifier为证书则删除过期的一个，再重启Xcode即可。这是一种方法。打开钥匙串，并没有发现证书，在开发者中心重新下载了一个用钥匙串打开。打包--------OK     问题得到了解决。



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


