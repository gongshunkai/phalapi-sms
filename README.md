# 短信扩展


## 安装和配置
修改项目下的composer.json文件，并添加：  
```
    "phalapi/sms":"dev-master"
```
在/path/to/phalapi/config/app.php文件中，配置： 
```
    'apiCommonRules' => array(//'sign' => array('name' => 'sign', 'require' => true),
    ),

    "SMSService" => array(
        "accountSid"   => "",  //主帐号
        "accountToken" => "",  //主帐号Token
        "appId"        => "",  //应用Id
        "serverPort"   => "",  //请求端口 默认:8883
        "serverIP"     => ""   //请求地址不需要写https:// 默认:sandboxapp.cloopen.com
    )
```
然后执行```composer update```。  

## 注册
在/path/to/phalapi/config/di.php文件中，注册：  
```php

//初始化SMS\Lite传入配置文件地址,第二个参数为是否开启debug模式开启debug模式(默认false)会把返回结果打印出来(生产环境请不要进行设置)
$di->sms = function() {
	return new \PhalApi\SMS\Lite(\PhalApi\DI()->config->get('app.SMSService'));
};
```

## 使用
1. 发送模板短信
```php
\PhalApi\DI()->sms->sendTemplateSMS("手机号码", "内容数据", "模板Id");
```
2. 短信模板查询
```php
\PhalApi\DI()->sms->QuerySMSTemplate("模板ID");
```
3. 语音验证码
```php
\PhalApi\DI()->sms->voiceVerify("验证码内容", "循环播放次数", "接收号码", "显示的主叫号码", "营销外呼状态通知回调地址", '语言类型', '第三方私有数据');
```
4. 语音文件上传
```php
\PhalApi\DI()->sms->MediaFileUpload("文件名", "文件二进制数据");
```
5. 话单下载 前一天的数据（从00:00 – 23:59）
```php
\PhalApi\DI()->sms->billRecords("话单规则", "客户的查询条件");
```
6. IVR外呼
```php
\PhalApi\DI()->sms->ivrDial("待呼叫号码", "用户数据", "是否录音");
```
7. 外呼通知
```php
\PhalApi\DI()->sms->landingCall("被叫号码", "语音文件名称", "文本内容", "显示的主叫号码", "循环播放次数", "外呼通知状态通知回调地址", '用户私有数据', '最大通话时长', '发音速度', '音量', '音调', '背景音编号');
```
8. 主帐号信息查询
```php
\PhalApi\DI()->sms->queryAccountInfo();
```
9. 呼叫状态查询
```php
\PhalApi\DI()->sms->QueryCallState("callid", "查询结果通知的回调url地址");
```