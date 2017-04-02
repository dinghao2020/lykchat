# lykchat信息发送系统
lykchat信息发送系统是Python3开发的，通过模拟微信网页端，基于个人微信号，为系统管理人员提供信息发送工具。
实现的功能有用户登录管理、微信登陆管理和微信信息发送功能。


## 特点 ## 

	1、简单高效
		基于个人微信号，模拟微信web端，部署和维护简单
		web管理页面实现可视化管理微信登陆
		接口采用URL，简化调用复杂度，返回结果均为json格式
	2、信息共享
		通过共享用户session和微信登陆信息，保证系统长期稳定运行
	3、7*24不间断服务
		计划任务定时检查微信登陆状态，微信保持登陆超过20天
	4、用户管理
		通过用户隔离微信个人号，不同用户管理不同微信号
		用户密码分为管理密码和接口密码，保证用户信息安全性
	5、微信信息安全
		不会监控和存储微信聊天信息
		不会增加和删除好友


## 截图 ##

管理页面--等待扫码
![等待扫码 截图](https://raw.githubusercontent.com/lykops/lykchat/master/doc/web管理--登陆.jpg)


管理页面--功能展示
![等待扫码 截图](https://raw.githubusercontent.com/lykops/lykchat/master/doc/web页面--功能说明.jpg)

管理页面--微信登陆时长
![等待扫码 截图](https://raw.githubusercontent.com/lykops/lykchat/master/doc/微信登陆时间超过1天.jpg)


接口-发送信息成功

![等待扫码 截图](https://raw.githubusercontent.com/lykops/lykchat/master/doc/接口-发送信息成功.jpg)


## 模块 ##

	1、管理web页面：可视化管理微信个人号
		用户登录和认证
		微信号登陆管理
		发送信息给好友
	2、发送信息接口：
		通过接口方式为其他业务系统发送信息给指定好友
	3、计划任务检测微信登陆状态：
		获取所有登录微信成功的用户，通过调用检测微信登陆接口
	4、会话保持模块：
		存储微信登陆信息和会话信息，同用户在任何地方登陆，保证微信登陆状态一致
	5、模拟微信web端模块：
		通过微信登陆信息，访问微信web端接口，实现管理登陆、发送信息等功能。
		
	
## V2.0.0版本说明 ##
	
	1、修复bug：
		微信登陆时间超过12小时自动退出，测试过程中测得最大登陆时长20天
	2、完善功能：
		1）、微信会话保持机制：
			保存位置：之前保存在数据库中，修改为数据库只记录用户名，所有信息保持到文件中，减少数据库的查询、写入、加解密压力
			动态更新微信登陆信息
			调整会话信息内容
		2）、优化微信检测登陆流程，大大缩短各个页面执行时间
		3）、完善获取好友流程
	3、新增功能：
		1）、增加用户管理机制
		2）、好友信息缓存机制
	4、取消功能：
		接受和处理新信息


## 发送信息接口 ##
	URL地址：http://IP（或者域名）/sendmsg
	支持post和get方法
	请求参数说明：
		'username' : 管理用户，同管理web页面，通过用户确认微信发送者
        'pwd' : 接口密码，注意不等于登陆密码,
        'friendfield'：接受信息的好友字段代号，0昵称，1微信号，2备注名，可以为空，默认为0
        'friend': 接受信息的好友的昵称、微信号、备注名的其中之一，不能为空
        'content': 发送内容，不能为空
		注意：
			friend一定是该用户下的登陆微信好友列表中的
			friendfield最好是微信号（Alias），也可以使用昵称（NickName）或者备注名（RemarkName）（但不能重复出现）
			由于好友列表使用缓存机制，新增好友可能发送信息不成功
	返回信息：
		json格式，{'Msg': 执行结果, 'Code':返回代码, 'ErrMsg':如果-1005返回参数列表，其他发送微信返回信息}
			常见code：0成功；-1101参数错误；-1102无法找到好友；1101微信号退出登录，其他为微信返回错误
	例子：http://192.168.100.104/sendmsg?username=zabbix&pwd=123456&friendfield=1&friend=lyk-ops&content=test

 
## 说明 ##

	1、作者尽可能通过严谨测试来验证系统功能，但由于专业水平有限，无法避免出现bug。
	2、该项目是基于微信web端进行开发的
		由于微信web端参数经常变动，可能会导致系统异常。
		如作者发现该问题，将会更新和修复。
	3、该项目开发的目的：为监控系统提供一个通过微信发送告警信息。
		所以该项目只实现了微信的登陆、接受和发送信息这三个功能，其他功能暂不考虑。
		建议使用一个独立的微信号，避免在登陆过程中在微信web端、PC客户端登陆，也不要在手机端退出web登陆。
	4：该项目为个人开源项目，免费开源。
		请勿使用该系统发送非法、不良信息。
		在使用过程中，如有任何问题，作者不承担任何责任。
	5、联系方式：		
		微信：lyk-ops
		邮箱：liyingke112@126.com	


![WIKI](https://github.com/lykops/lykchat/wiki/)


