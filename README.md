一个在windows系统上用于给AI虚拟主播在bilibili上开播的工具。

**注意：本项目只能实现各个平台的对接与结果呈现，相关语言模型，筛选器模型，TTS模型需要自行准备以对接**

本项目处于早期阶段，欢迎任何issue。

## （并非快速的）开始

（请先确保你安装了pywin32等库并且运行了postinstall脚本，我懒得写具体步骤了）

### 一：开放平台的申请

[哔哩哔哩直播开放平台](https://open-live.bilibili.com/)

在上述平台中申请权限，等待一周左右，你会在对应邮箱中获得access_key_id，access_key_secret。

在[幻星-互动玩法](https://play-live.bilibili.com/)中，你可以获得你的主播身份码。

此外，你需要找一个GPT-so-vits的api服务（或者直接部署本地）以及语言模型（可以是你自己的模型，也可以是有prompt的网络模型），准备至少一个语言模型的api_key。

### 二：配置文件

有了足够的配置，现在可以开始进行相关环境配置了。

打开`config.json`文件，填入相关信息。（文件里的描述很完整了，这里不再赘述）

**注意：我不建议你使用beta功能，有bug且高度不可控**

### 三：直播场景的搭建

**本项目不可以帮你搭建直播场景，相关直播场景的搭建，live2d的准备需要个人完成**

建议使用的直播场景搭建工具：哔哩哔哩直播姬，OBS Studio

建议使用的live2d播放工具：VTube Studio（如果你有能力，可以直接使用pylive2d，后续版本可能会补上接口）

### 四：直播程序的启动

直接启动主程序`todolist.py`可以自动开始监听直播间消息，并进行相关输出。

## 详细说明

1. **项目能够实现的功能**
   - 监听直播间消息（具体类型：进入直播间，给直播间点赞，直播间弹幕，直播间礼物，直播间大航海）
   - 对弹幕的筛选以及指令反馈：去除**不易于AI主播处理**的弹幕，在程序内处理有关命令的弹幕。
   - 对正常的聊天类型弹幕，交给AI主播处理，进行TTS，并进行输出和相应反馈。

2. **关于输出**

   所有输出文件储存在`logs`文件夹中，其中：

   - `command.json` `text.json` `todo_raw.json`为中间文件（日志），分别存储直播命令+经过筛选的弹幕，语言模型的输出，初始消息列表。

   - `livetext.txt`存储除弹幕外之前提到的所有监听到的消息，可以直接导入OBS。

   - `output.txt`存储语言模型的输出，可以直接导入OBS。

3. **关于指令**

	现阶段不支持自定义指令，支持的指令有：

	- `remtext`**操作权限仅限于主播（需在配置文件中设置），房管**，可以清除模型的记忆。（prompt，预设对话除外）
	- `翻唱 歌曲名/编号`可以进行相关角色翻唱（配置文件中可以设置条件），仅支持在`AI`目录中保存的歌曲。
	- `点歌 歌曲名`可以进行点歌，前提是你自己开一个点歌机。（这好像不是我们的指令，但我们可以保证相关弹幕不会被语言模型处理）
	- 以`#`开头的弹幕不会进行处理。

好像没什么可说的了（？），想到什么再来补充吧。
