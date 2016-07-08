##安装
```bash
composer require "anruence/yii2-wechat-sdk": "dev-master"
```
您可以使用composer来安装, 添加下列代码在您的``composer.json``文件中并执行``composer update``操作

```json
{
    "require": {
       "anruence/yii2-wechat-sdk": "dev-master"
    }
}
```

##使用示例

在使用前,请先参考微信公众平台的[开发文档](http://mp.weixin.qq.com/wiki/index.php?title=%E9%A6%96%E9%A1%B5)

- Wechat定义方式

```php
//在config/web.php配置文件中定义component配置信息
'components' => [
  .....
  'wechat' => [
    'class' => 'anruence\wechat\sdk\Wechat',
    'appId' => '微信公众平台中的appid',
    'appSecret' => '微信公众平台中的secret',
    'token' => '微信服务器对接您的服务器验证token'
  ]
  ....
]
// 全局公众号sdk使用
$wechat = Yii::$app->wechat; 


//多公众号使用方式
$wechat = Yii::createObject([
    'class' => 'anruence\wechat\sdk\Wechat',
    'appId' => '微信公众平台中的appid',
    'appSecret' => '微信公众平台中的secret',
    'token' => '微信服务器对接您的服务器验证token'
]);
```

- Wechat方法使用(部分示例)

```php

$wechat = Yii::$app->wechat;

//获取access_token
var_dump($wechat->accessToken);

//获取二维码ticket
$qrcode = $wechat->createQrCode(123);
var_dump($qrcode);

//获取二维码
$imgRawData = $wechat->getQrCodeUrl($qrcode['ticket']);

//获取群组列表
var_dump($wechat->getGroups());


//创建分组
$group = $wechat->createGroup('测试分组');
echo $group ? '测试分组创建成功' : '测试分组创建失败';

//修改分组
echo $wechat->updateGroupName($group['id'], '修改测试分组') ? '修改测试分组成功' : '测试分组创建失败';


//根据关注者ID获取关注者所在分组ID
$openID = 'oiNHQjh-8k4DrQgY5H7xofx_ayfQ'; //此处应填写公众号关注者的唯一openId

//修改关注者所在分组
echo $wechat->updateMemberGroup($openID, 1) ? '修改关注者分组成功' : '修改关注者分组失败';

//获取关注者所在分组
echo $wechat->getGroupId($openID);

//修改关注者备注
echo $wechat->updateMemberRemark($openID, '测试更改备注') ? '关注者备注修改成功' : '关注者备注修改失败';

//获取关注者基本信息
var_dump($wechat->getMemberInfo($openID));

//获取关注者列表
var_dump($wechat->getMemberList());

//获取关注者的客服聊天记录, 
var_dump($wechat->getCustomerServiceRecords($openID, mktime(0, 0, 0, 1, 1, date('Y')), time())); //获取今年的聊天数据(可能获取不到数据)

//上传媒体文件
$filePath = '图片绝对路径'; //目前微信只开发jpg上传
var_dump($media = $wechat->uploadMedia(realpath($filePath), 'image'));

//下载媒体文件
echo $wechat->getMedia($media['media_id']) ? 'media下载成功' : 'media下载失败';

```
