*2026-03-24*

## 【开源】《PhotoAlbum》一个极简的、单文件运行的、适合内网和家庭部署的相册应用

一个面向局域网使用的照片管理工具，支持多用户登录、时间线浏览、相册管理、回收站恢复、分享链接和批量下载等功能。

### 特性

- 单二进制运行，部署方式简单
- 局域网访问，适合家庭和小团队使用
- 多用户登录与隔离
- 时间线视图，按日期浏览照片
- 自定义相册管理
- 回收站与恢复
- 分享链接与匿名访问
- 单张下载、批量打包下载、整相册下载
- 支持桌面端与移动端

### 仓库

- GitHub：`https://github.com/DoYoungDo/PhotoAlbum`

### 下载地址

- [https://github.com/DoYoungDo/PhotoAlbum/releases](https://github.com/DoYoungDo/PhotoAlbum/releases)

### 使用

#### 部署和启动

这个项目采用单二进制形式，部署时不需要额外准备复杂环境。将程序放到一台局域网内可访问的设备上，启动后即可通过浏览器访问。

首次运行时，程序会进入初始化流程，按提示完成以下配置：

- 服务端口
- 图片存储路径
- 默认用户名
- 默认密码

初始化完成后，程序会输出访问地址。同一局域网内的电脑或手机，使用浏览器打开该地址即可进入相册页面。

整体流程可以理解为：

1. 准备可执行文件
2. 在局域网设备上启动程序
3. 完成首次初始化配置
4. 记录程序输出的访问地址
5. 在浏览器中打开相册页面

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGLNpwg4PxvtCgaBlRlgxDjA-ZHVLvgACpSEAApbxEFbYxYMXraIqeDoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGLNpwg4PxvtCgaBlRlgxDjA-ZHVLvgACpSEAApbxEFbYxYMXraIqeDoE.png" />


如果后续需要增加新的登录用户，也可以通过程序提供的命令行子命令来完成：

```bash
./photoalbum adduser
```

执行后按提示输入用户名和密码即可。新用户会被写入配置文件中

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGNJpwhgBHLudMWbypzlVV0Abql2MVwAC-CEAApbxEFY5YySFT8tG6ToE.png" alt="BQACAgUAAyEGAASHRsPbAAESGNJpwhgBHLudMWbypzlVV0Abql2MVwAC-CEAApbxEFY5YySFT8tG6ToE.png" />

**提示：** *没有提供注册用户的界面，也不打算提供，为了尽量极简*

#### 界面使用

登录后即可开始使用。

照片上传完成后会进入时间线视图，并按日期展示。用户可以继续浏览，也可以把照片加入相册、批量下载、删除，或者生成分享链接发给其他人。

常见使用流程如下：

1. 登录相册页面
2. 上传照片
3. 在时间线中浏览或多选照片
4. 将照片加入相册，或执行下载、删除等操作
5. 需要共享时生成分享链接

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGLRpwg51ajlf6W5xetZRiu96n7JncAACqCEAApbxEFb3-JHXT2BJlToE.png" alt="BQACAgUAAyEGAASHRsPbAAESGLRpwg51ajlf6W5xetZRiu96n7JncAACqCEAApbxEFb3-JHXT2BJlToE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGLVpwg_IrybZeG5W0kdcO-VKolxOBwACsCEAApbxEFazQjP1E4DWmDoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGLVpwg_IrybZeG5W0kdcO-VKolxOBwACsCEAApbxEFazQjP1E4DWmDoE.png" />

### 功能介绍

#### 时间线视图

时间线是这个相册最适合日常使用的浏览方式。

照片会按日期分组展示，浏览时不需要依赖文件夹或文件名来查找内容，而是可以直接按时间回看。对于已经积累了较多历史照片的用户来说，这种方式会更自然。

在时间线中，还可以按日期批量选择、批量删除、批量下载，或者批量加入相册。

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGLZpwhBGFeU3V3JXXbx7gJiqLwIOJwACtSEAApbxEFbwuP0UZZxw3zoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGLZpwhBGFeU3V3JXXbx7gJiqLwIOJwACtSEAApbxEFbwuP0UZZxw3zoE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGLlpwhFRQYswgLQezDBEBD4pgYOGBgACuSEAApbxEFaJipWqMYvPWDoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGLlpwhFRQYswgLQezDBEBD4pgYOGBgACuSEAApbxEFaJipWqMYvPWDoE.png" />

#### 上传照片

上传支持点击选择、拖拽上传和多文件上传。

上传过程中会显示进度；如果个别文件失败，也可以单独重传，或者一键重传失败项，不需要重新处理整批文件。对于需要频繁整理照片的场景，这种方式更省事。

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGLppwhGawV5sCZZhFLFn8mXVGVDEiQACuiEAApbxEFbUqWcvc5Y2djoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGLppwhGawV5sCZZhFLFn8mXVGVDEiQACuiEAApbxEFbUqWcvc5Y2djoE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGLtpwhGdu5m60U3yFn0634b1Ad4akAACuyEAApbxEFb-7QERIOUVqjoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGLtpwhGdu5m60U3yFn0634b1Ad4akAACuyEAApbxEFb-7QERIOUVqjoE.png" />

#### 相册管理

如果需要按主题长期整理照片，可以使用相册功能。

例如家庭聚会、旅行记录、孩子成长、团队活动、产品素材等，都可以分别建立相册进行管理。进入相册后仍然可以继续浏览其中的照片内容，也支持整相册下载。

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGL5pwhIxyukrBYKoQw_UJtJj_4kvWgACviEAApbxEFZtpYOj_713ajoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGL5pwhIxyukrBYKoQw_UJtJj_4kvWgACviEAApbxEFZtpYOj_713ajoE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGL9pwhJlrOxCs7o_GDw1HLX1qeR2JwACvyEAApbxEFa2MEGsx7t2VToE.png" alt="BQACAgUAAyEGAASHRsPbAAESGL9pwhJlrOxCs7o_GDw1HLX1qeR2JwACvyEAApbxEFa2MEGsx7t2VToE.png" />

#### 回收站

删除的照片不会立即永久消失，而是先进入回收站。

在回收站中可以预览、恢复、批量恢复；确认不再需要时，再执行永久删除或清空回收站。这样的设计更适合日常整理照片时使用，可以降低误删带来的风险。

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMBpwhLa3n5erOAG2mIr49FRlLeZZwACwCEAApbxEFa_N4o46lHAZjoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGMBpwhLa3n5erOAG2mIr49FRlLeZZwACwCEAApbxEFa_N4o46lHAZjoE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMFpwhMj5qTi0CuPi2kaGBEf9_70CwACwyEAApbxEFYGNsB3xA0Q1joE.png" alt="BQACAgUAAyEGAASHRsPbAAESGMFpwhMj5qTi0CuPi2kaGBEf9_70CwACwyEAApbxEFYGNsB3xA0Q1joE.png" />

#### 分享链接

图片支持创建分享链接，并可设置过期时间。

拿到链接的人无需登录，就可以直接查看和下载被分享的内容。这种方式适合家庭成员之间共享照片，也适合小团队临时分发活动照片、记录照片或素材图。

系统内部也可以统一查看、复制和删除已创建的分享链接，便于后续管理。

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMRpwhQc6D8Sy70zBRweE0S4sYTpdwACyCEAApbxEFa3MHLGxPhYhDoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGMRpwhQc6D8Sy70zBRweE0S4sYTpdwACyCEAApbxEFa3MHLGxPhYhDoE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMNpwhQYLnTDASvDEnwUoK8G8dFGrAACxyEAApbxEFZY2IPvtzOWXzoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGMNpwhQYLnTDASvDEnwUoK8G8dFGrAACxyEAApbxEFZY2IPvtzOWXzoE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMJpwhQVbY0OkSASkcPLzrqpKNQ2nAACxiEAApbxEFY_S1JywYYkYToE.png" alt="BQACAgUAAyEGAASHRsPbAAESGMJpwhQVbY0OkSASkcPLzrqpKNQ2nAACxiEAApbxEFY_S1JywYYkYToE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMVpwhQfuByPc7BdfyMz0YtF0MdvKwACySEAApbxEFaAUrpD5VnIXzoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGMVpwhQfuByPc7BdfyMz0YtF0MdvKwACySEAApbxEFaAUrpD5VnIXzoE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMdpwhUIJt6Y6gGEclAZoQYYXQ-E6wAC0iEAApbxEFbrPOssdo8AAX06BA.png" alt="BQACAgUAAyEGAASHRsPbAAESGMdpwhUIJt6Y6gGEclAZoQYYXQ-E6wAC0iEAApbxEFbrPOssdo8AAX06BA.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMZpwhUFs1z5uC4Y50Aa9jvTQREF4AAC0SEAApbxEFaOz22_9CXyFToE.png" alt="BQACAgUAAyEGAASHRsPbAAESGMZpwhUFs1z5uC4Y50Aa9jvTQREF4AAC0SEAApbxEFaOz22_9CXyFToE.png" />

#### 下载

下载方式覆盖了比较常见的需求。

支持单张下载、多选打包下载，以及整相册打包下载。无论是临时取回一张原图，还是一次性拿走一组照片，都不需要逐张处理。

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMtpwhWZ66Ju7DZwpl6BAAFha9xWplQAAt4hAAKW8RBWcMKoVO2CVwM6BA.png" alt="BQACAgUAAyEGAASHRsPbAAESGMtpwhWZ66Ju7DZwpl6BAAFha9xWplQAAt4hAAKW8RBWcMKoVO2CVwM6BA.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMxpwhWc1H4W_jZ0wUibcTUi4k2eRwAC3yEAApbxEFYa5pPA55nEEzoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGMxpwhWc1H4W_jZ0wUibcTUi4k2eRwAC3yEAApbxEFYa5pPA55nEEzoE.png" />
<br>
<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGMppwhWWt_AYpuStmjAdSZ-XiAmsFQAC3SEAApbxEFb2f0JkD45jAjoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGMppwhWWt_AYpuStmjAdSZ-XiAmsFQAC3SEAApbxEFb2f0JkD45jAjoE.png" />

#### 桌面端和移动端

这个相册同时考虑了桌面端和移动端的使用体验。

桌面端适合浏览和批量整理，移动端适合日常查看。对于家庭共享和小团队协作来说，这种适配方式会更实用。

<img width="500" src="https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAESGM5pwhZqjO0R-KcrSMSkQykoT-VaagAC6iEAApbxEFbBGs8ZsFuPyDoE.png" alt="BQACAgUAAyEGAASHRsPbAAESGM5pwhZqjO0R-KcrSMSkQykoT-VaagAC6iEAApbxEFbBGs8ZsFuPyDoE.png" />

### 适合的场景

- 家庭照片集中存放与日常回看
- 在局域网内给家人共享照片
- 小团队整理活动照片、记录照片、素材图
- 希望自己掌控照片存储与访问方式的用户
- 需要一个上手简单、功能完整的本地相册工具的场景

### 总结

《PhotoAlbum》更像一个偏日常使用的局域网相册工具，而不是一个需要复杂配置和重型流程的平台。

它的重点在于把照片管理中最常见的动作做完整：上传、浏览、整理、恢复、分享和下载。对于家庭和小团队来说，这样的能力组合已经能够覆盖大多数实际使用场景。

### 最后

- 这是一个仅花了半天时间，全程使用AI完成的项目（包括此Blog），可能有一些问题，欢迎在GitHub上提交Issue。🤣
- 后面我准备再添加一个上传视频和在线播放的功能。