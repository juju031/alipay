## 生成支付宝

##安装

```
composer require "juju/alipay:1.0"

"juju/alipay": "~1.0.0"
```

##控制器顶部
```
use use \juju\alipay\AlipayConfig;
use \juju\alipay\AlipaySubmit;
```

##控制器调用(生成支付)
```
$alipayConfig = (new AlipayConfig())->getAlipayConfig();
$alipaySubmit = new AlipaySubmit($alipayConfig);

//服务器异步通知页面路径
$notify_url = Yii::$app->urlManager->createAbsoluteUrl(['pay/alipay/notify']);
$return_url = Yii::$app->urlManager->createAbsoluteUrl(['pay/alipay/return']);
$show_url   = Yii::$app->urlManager->createAbsoluteUrl(['goods/id/111']);
//需http://格式的完整路径，不允许加?id=123这类自定义参数

$out_trade_no = time();
$subject = '测试支付宝';
$body = '支付描述';
$total_fee = 0.01;     

$parameter = array(
    "service"           => "create_direct_pay_by_user",
    "partner"           => trim($alipayConfig['partner']),
    "payment_type"      => 1,
    "notify_url"        => $notify_url,
    "return_url"        => $return_url,
    "seller_email"      => trim($alipayConfig['seller_email']),
    "out_trade_no"      => $out_trade_no,
    "subject"           => $subject,
    "total_fee"         => $total_fee,
    "body"              => $body,
    "show_url"          => $show_url,
    "anti_phishing_key" => '',
    "exter_invoke_ip"   => Yii::$app->request->userIP,
    "_input_charset"    => trim(strtolower($alipayConfig['input_charset']))
);

$html_text = $alipaySubmit->buildRequestForm($parameter,"post", "确认");
echo $html_text;
exit();
```

## 验证支付

##控制器顶部
```
未完成
$alipayConfig = (new AlipayConfig())->getAlipayConfig();
$notify = new AlipayNotify($alipayConfig);
if ($notify->verifyNotify()) {
	return "success";
}else{
	return "false";
}
```