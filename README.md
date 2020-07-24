#重点在于数据库做成，UI太难看，不想花时间做。
#针对车机USB歌曲视频播放机能，设计一个扫描APP替代MediaProvider.apk，MediaProvider对于新数据插入速度很慢，我的车机MIPS是10000，新的USB，一万首歌和一万个视频全部扫描需要20分钟，而我的需求是10秒内完成，所以我从设计方面和数据库结构做了改善，扫描分为两次，第一次扫描不添加专辑等信息（这样专辑列表可能很慢才更新好），用最快的速度把所有歌曲视频文件夹路径名称id，parentid等信息插入数据库，这样用户就可以在所有歌曲列表、所有视频列表或者文件夹里面找到自己想要播放的歌曲视频，为了方便找，也可以根据拼音排序，英文排序，或者文件修改时间排序。第一遍扫描完后，就开始第二遍扫描，第二遍扫描会把有些数据更新到原来的表里面，然后做专辑风格等列表，这个时候用户可以访问专辑列表，但是可能还没完全更新完，只能看到一部分数据。

开发进度：
现在第一遍扫描已经完成80%，有些细节需要改正，测试一万首歌一千个视频需要5秒，车机性能差，大概是手机性能的1/5.手机的话可能一秒就完成了。UI方面视频播放还没做。代码有点乱，过几天把注释都写好。

![主界面.png](https://github.com/Tecinno/MediaScanner/blob/tamago/%E4%B8%BB%E7%95%8C%E9%9D%A2.png)

针对Android的MediaProvider 和 MediaScanner对车机USB播放的几点问题：
1、扫描的时候是按照深度扫描，并且顺序扫描，如果视频太多，音乐就一直扫描不到，而且一般歌曲视频不会放到那么深的目录；
2、如果USB删除了一部分媒体文件，Android虽然在预扫描的时候会删除不存在的数据，但是预扫描的时候是可以播放歌曲视频的，这个时候就会有脏数据；如果先预扫描再播放就很慢。
3、插入新USB，扫描新数据的时候会读取音频title等信息，耗时特别严重。
4、相对于车机的需求，Android创建的表比较多，多出很多冗余信息。

