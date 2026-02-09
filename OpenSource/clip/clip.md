*2026-02-09*

## 【开源】《clip》一个不到4M的、跨平台的、支持分组、搜索、自定义条数、局域网共享的、剪贴板历史工具

### 开源仓库

[github 仓库](https://github.com/DoYoungDo/clip)

[gitee 仓库](https://gitee.com/DoyoungDo/clip)

### 安装

下载地址

从 [github 仓库](https://github.com/DoYoungDo/clip/releases) 或 [gitee 仓库](https://gitee.com/DoyoungDo/clip/releases) 下载指定平台指定版本的 clip。 

本工具为免安装工具，下载解压后直接运行即可

### 功能

#### 剪贴板历史记录

- clip 启动后会显示一个系统托盘图标

  ![BQACAgUAAyEGAASHRsPbAAEQz9BpibuGp8YpRgJFru9mmB-qcnyUxAACQB8AAplwUFTV4SAl4syhwzoE.png](https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAEQz9BpibuGp8YpRgJFru9mmB-qcnyUxAACQB8AAplwUFTV4SAl4syhwzoE.png)

- 左键点击图标会显示一个菜单，菜单分为上下两部分，上半部分为剪贴板的最近历史记录，下半部分为分组列表。

  ![BQACAgUAAyEGAASHRsPbAAEQz9Npibyffo7BRRULi630AAFwpZEj_1UAAk0fAAKZcFBUN8doIOMWhEU6BA.png](https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAEQz9Npibyffo7BRRULi630AAFwpZEj_1UAAk0fAAKZcFBUN8doIOMWhEU6BA.png)

  - 点击任意一条记录会将该记录复制到剪贴板（同时也会出现在记录的最上面一条，也就是说最上面一条永远是系统剪贴板中的内容），此时可以在任意位置粘贴。分组内的记录同上

  - 记录支持文本和图片

- 右键点击图标，除了显示历史记录，还会显示一些进行额外操作的选项菜单，例如：清空历史记录、创建分组等。

  ![BQACAgUAAyEGAASHRsPbAAEQz-hpib4NFys9Z99u9c9F5g_YpsEYpAACbR8AAplwUFR7VRLlg4XZuToE.png](https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAEQz-hpib4NFys9Z99u9c9F5g_YpsEYpAACbR8AAplwUFR7VRLlg4XZuToE.png)

#### 分组

- 操作：`右键` -> `创建分组`，会以剪贴板历史记录的最近一条（最上面一条）为分组名，创建一个分组
- 如果最近一条为图片，则会创建失败
- 自定义分组名，需要在其它位置将名称文本复制到剪切板
- 分组重命名也会以最近一条记录为分组名,操作：`右键` -> `分组` -> `重命名`
- 分组状态为激活和非激活，默认为非激活状态，激活状态下，复制的内容会自动同步到该分组中
- 本工具支持本地持久化存储，退出工具后再次启动，会加载上次关闭前的历史记录和分组，包括激活状态，由此，可以将分组当作TODO LIST使用
- 删除分组操作：`右键` -> `分组` -> `删除分组`

#### 搜索

本工具为纯菜单操作，没有输入框，搜索时，将需要搜索的内容复制到剪贴板（记录最上面一条），然后操作 `右键` -> `搜索`，再次点击或右键点击托盘图标时，会显示过滤后的结果

#### 自动识别颜色

- 设置状态 ：`右键` -> `配置` -> `自动识别颜色`
- 开启后，本工具会识别`#fff`、`#ffffff`十六进制和`(255,255,255)`、`255,255,255`rgb格式的颜色文本
- 在该颜色文本记录上可以直接复制指定格式，做到快速转换格式
  ![BQACAgUAAyEGAASHRsPbAAEQ0AxpicUAAVpC_cELk04GgItxgbD2S0YAArAfAAKZcFBUmOshBCmDGPQ6BA.png](https://img.remit.ee/api/file/BQACAgUAAyEGAASHRsPbAAEQ0AxpicUAAVpC_cELk04GgItxgbD2S0YAArAfAAKZcFBUmOshBCmDGPQ6BA.png)

#### 自定义条数

- 本工具可以设置最小1条，最大300条历史记录，默认为50条
- 同搜索，先将要设置的条数数字复制到剪贴板（记录最上面一条），然后操作 `右键` -> `配置` -> `设置最大历史记录条数`
- 注意，如果上次的历史记录条数超过本次设置的条数，并且历史记录真实条数也超过本次设置的条数，那么只会从上到下保留最新的记录，超出部分会被删除，本地持久化时亦不会保留

#### 局域网共享

  局域网内的机器可以通过本工具实现剪贴板共享（包括图片）

  - 共享机器A
    - 操作：`右键` -> `配置` -> `局域网共享` -> `局域网共享`
    - 开启会会将监听本机地址以`192.168.1.100:8080`的形式复制到剪贴板中
  - 被共享机器B
    - 将A共享的地址复制到剪贴板（记录最上面一条）
    - 操作：`右键` -> `配置` -> `局域网共享` -> `连接到`
    - 再次操作，断开连接
  - A之后的所有复制操作，都会同步到B的剪贴板中，之前的不会


