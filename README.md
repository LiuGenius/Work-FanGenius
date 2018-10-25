## 灵之心管理员端接口需求（2018-07-11） ##
### 用户相关 ###

- 获取管理员所有设备下的所有用户（筛选条件： 按设备/按性别/按年龄区间）
	- 用户信息必须字段：用户id，用户名，年龄，性别，单位名称。
- 根据用户ID获取一个用户的详细信息。
	- 需要用户的所有信息。
	
	
### 设备相关 ###
- 获取一个管理员名下的所有设备信息(可以使用用户相关接口获取用户数据)
	- 设备信息必须字段：设备id，设备名称，设备类别，管理员对设备的备注名
- 根据用户查询智能多维自信训练系统个人报告数据
	- 智能多维自信训练系统个人报告数据必须字段：见界面
- 获取智能多维自信训练系统数据（按设备）/ 具体首页展示的数据
	- 字段：
		- (总用户数 int)
		- (总报告数 int) 每个用户使用一次会生成一个报告
		- (今日新增用户 Int)
		- (今日产生报告 int)
		- (今日使用人数 int)
		- (本月产生报告 int)
		- (当前设备状态  使用中/待机中) 具体使用什么数据类型你那边定
- 获取心理健康教育管理平台（按设备）/具体首页展示的数据
	- 字段：
		- (总用户数 int)
		- (总报告数 int) 每个用户参与评测一次会生成一个报告
		- (今日新增用户 Int)
		- (今日产生报告 int)
		- (今日使用人数 int)
		- (本月产生报告 int)
		- (当前设备状态  使用中/待机中) 具体使用什么数据类型你那边定
- 管理员绑定一台设备，并且会生成一个推送ID，建议可以把推送ID作为这个设备的ID。
	- 管理员绑定设备时才添加这台设备到服务器
	- 一台设备只能被一个管理员绑定
	- 推送功能 使用百度云推送

### 管理员个性接口 ###
- 管理员用户登录，登录后获取管理员所有信息。
- 管理员删除用户：从设备中删除一个用户。
- 管理员对某一个设备设置备注名称
- 管理员推送信息(控制设备)：从百度云推送发送一条推送到对应设备，推送信息如:<br/> `{"play":"http://xxxx.mp3"}`
- 管理员创建组：创建一个用户组，用户组依赖于设备下，只能填写该设备下的用户，心理健康教育管理平台无法创建组，只有智能多维自信训练系统可以创建组
- 获取所有的评测表信息列表：<font color="#660000">这个是固定的，从那100多个表中选10-15个，固定创建之后录入对应题目和得分即可，确定是哪一些之后会把表名列在这里</font>
	- 需要的字段
		- 评测表ID
		- 评测表名称	 
- 管理员创建测评普查：在管理员选择普查信息，选择服务器中的一个普查表，推送给管理选择的用户组相应的信息
	- 创建测评普查时提交到服务器的字段（）：
		- （普查名称 String）
		- （开始时间 long）
		- （结束时间 long）
		- （最小年龄 int）
		- （最大年龄 int）
		- （组信息（多个字段）array）
		- （普查说明 String）
		- （评测表id String）
		- （用户测评完成之后是否将测评结果展示给用户看 boolean）


### 心理健康教育管理平台  评测表（10） ###
具体分为5个大类，每个大类下分为不同的评测表，但是录入不用分组，每个表加一个字段为 （评测类型） 记录大类就可以

- 能力测评
	- 1.思维能力测试
	- 2.自信心心理测量
- 情绪评定
	- 3.艾森科情绪稳定性测验
	- 4.焦虑自评量表
	- 5.社会适应性自评问卷
	- 6.心理健康临床症状自评量表SCL-90
- 学习方面
	- 7.学习动机测验
	- 8.学习风格调查表
- 职业倾向
	- 9.霍兰德职业倾向测验
- 睡眠评定
	- 10.匹兹堡睡眠质量指数  

## 灵之心管理员端接口需求  2018-07-12 更新  ##


### 管理员个性接口 ###

- 获取评测的数据： 管理员发布测评后，可以查看本次测评的数据
	- 字段
		- 本次测试总人数 Int
		- 本次测试群组数量 Int
		- 每个用户测试平均用时 long
		- 未完成评测用户数 Int
		- 已完成评测用户数 Int
		- 完成情况百分比 int（100为最大值，直接返回50表示百分之五十）
		- 每个用户组完成的情况 array 
			- 组名 String
			- 组人数 int
			- 完成的人数 int
		- 用户展示 array
			- 用户名
			- 单位名称
![sss](https://github.com/LiuGenius/lingzhixin/blob/master/%E6%99%AE%E6%9F%A5%E8%AF%A6%E6%83%85.png)	

##  2018-08-03 更新  ##
- 管理员修改用户信息：管理员修改用户信息（）
	- 发送到服务器的数据,为null就不修改，手机号不修改 例：
	```
		{
		"id":"1e66e0fe11c444eeaf77eab0faea2592",
		"name":"LiuGenius",
		"birthday":"1999-09-09",
		"mobile":"15274973573",
		"passwd":"123456",
		"sex":null,
		"address":null,
		"idnumber":null,
		"email":null,
		}
	```	
	- 需要服务器返回的字段
		- 修改成功或者失败的标识	
- 获取每个发现所有的评论信息传入发现的id，按分页查询该id下的评论信息。
	- 发送到服务器的字段 (key-value)
		- 发现id
		- 要获取的条数 例：20
		- 获取的页数 例：5
	- 需要服务返回的字段：(array)
		- 评论id
		- 评论内容
		- 评论人头像（小）
		- 评论人昵称
		
		
##  2018-08-09 更新 	. 
### ------------------- 分割（先看这里）-------------------
### 智能多维自信训练系统描述:主要分为五类

#### 1.外显训练 - 播放视频，用户根据视频做出相应动作。（其实就是播放视频），
#### 2.请跟我读 - 播放音频，用户跟着读
#### 3.故事动画 - 纯粹播放视频
#### 4.训练互动 - 玩游戏。内置的8个小游戏。
#### 5.评测报告 - 评测

####（每次用户完成以上任意一个模块中的内容我会发送数据到服务器，生成一段数据，这条数据不管是什么模块生成的统一叫做报告）

### ------------------- 分割结束 -------------------


- 发送报告到服务器（完成任意模块的任意内容将数据存储到服务器）

！！！ 现在一共有两种设备一种是这个智能多维自信训练系统，一种是专门用来做测评报告的，但是这个智能多维自信训练系统也可以做测评，另一个是专门用来做测评没有其他的功能，我们老板的这个设计很迷。建议可以在每个报告里面加一个 equiType的字段用来表示到底是哪种设备产生的报告。equiType为2时报告都是测评报告特有的字段！！！ 

	- 发送到服务器的数据
		- 1.用户id
		- 2.设备id
		- 3.在哪个模块完成的内容 int（1-外显训练，2-请跟我读，3....，5-评测报告），方便每次对该模块使用次数+1
		- 4.完成描述 对模块的描述 如在训练互动中玩了 ‘走出沙漠’ 这个游戏，字段就为 “走出沙漠”
		- 5.报告建议（根据用户完成情况生成的建议，字符串，长度无法确定，可能很长，模块值为 5 时就是测评结果详情）
		- 6.测评表名称 （模块值为5时才会有值，测评的表） 
		- 7.本次使用时长 （建议以秒作为单位）
		- 8.本次使用时间 （开始使用时的时间long值，取当前时间也无所谓其实）
		- 9.测评结果 （模块值为 5 时才会有值，测评结果名称 例 ‘焦虑型人格’）
		- 10.最高分贝 int（模块值为 4 时才会有值，因为都是声音游戏，玩游戏时用户产生的最高分贝值）
		- 11.平均分贝  （模块值为 4 时才会有值 平均分贝=总分贝/分钟）ps: /是除的意思不是或者啊啊啊啊啊
		- 12.总分贝 （模块值为 4 时才会有值 总共产生的分贝）
		
![sss](https://github.com/LiuGenius/lingzhixin/blob/master/ae0a45db0d046fcb8216e91c05b03f6.png)

			
##  2018-08-24 更新 	.

- 1.修改用户头像
	- 发送到服务器的数据
		- 用户id
		- file文件
	- 需要服务器返回的数据
		- 修改是否成功
- 2.获取应用版本号
	- 发送到服务器的信息
		- 要获取应用tag （例 ： 手机端：lzx_phone 平板端： lzx_table 电视端：lzx_tv）
	- 需要服务器返回的数据
		- 最新的版本信息
- 3.上传最新的版本到服务器
	- 发送到服务器的数据
		- apk文件
		- 版本tag
		- 当前版本名称 (例 ：1.0.0.1)
	- 需要服务器返回的数据
		- 上传结果
		- 文件保存的路径（下载的url）
- 4.添加一条专家咨询信息
	- 发送到服务器的数据
		- 用户姓名 （用户真实姓名）
		- 联系方式 （手机号）
		- 咨询的内容（长文本）
	- 需要服务器返回的数据
		- 成功或者失败
	
##  2018-10-11 更新 
- /immediatelyPour/queryDetail 获取倾诉详情缺少回复数字段，缺少用户头像字段，缺少用户名字段，可以把评论参数从这个接口中拆除
- 添加一个新接口获取倾诉的评论 请求字段和返回字段都可以和 /discover/pageQueryDiscovers（分页查询发现下的评论信息） 一样

##  2018-10-24 更新 
- 添加一个接口修改设备的当前状态，联网状态，连接服务器状态
	- 发送到服务器的数据
		- 设备id
		- 设备的当前状态 1：使用中 2：游戏中 3：评测中 4：关机
		- 联网状态 true/false
		- 连接服务器状态 true/false
	- 需要服务器返回的数据	

##  2018-10-25 更新 
- /adminequip/initEquiGroup(查询一个设备下的所有用户组) 接口需要修改
	- Group需要的字段
		- 组id
		- 组名
		- 组人数
		- 组备注信息
		- 组推送 tag （创建这个组的同时在极光推送创建这个tag 注意应用名为lzx_phone,和设备的lzx_tv不是同一个，需要创建两个推送源）
		- 男性用户数
		- 女性用户数
		- 其他字段按照你之前的设定
- 用户信息需要添加三个字段
	- 绑定设备数
	- 产生报告数
	- 用户登陆的手机号的推送id
- 推送数据到组
	- 发送到服务器的字段(字段名不是固定的，你那边随便改)
		- GroupId 推送组id
		- GroupTag 推送组tag
		- pushTitle 推送的标题
		- pushMsg 推送的内容
		- adminId 发送推送的管理员id
		

- 推送数据到用户
	- 发送到服务器的字段
		- userId 用户id
		- userPushId 用户推送id
		- pushTitle 推送的标题
		- pushMsg 推送的内容
		- adminId 发送推送的管理员id
		
- 推送数据到设备
	- 发送到服务器的字段
		- equId 设备id
		- pushId 推送id（和设备id相同）
		- pushTitle 推送的标题
		- pushMsg 推送的内容
		- adminId 发送推送的管理员id
	
![sss](https://github.com/LiuGenius/lingzhixin/blob/master/%E6%8C%89tag%E6%8E%A8%E9%80%81.jpg)
api: https://docs.jiguang.cn/jpush/server/sdk/java_sdk/
