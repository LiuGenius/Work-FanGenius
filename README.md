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
	- 创建测评普查时需要的字段：
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
