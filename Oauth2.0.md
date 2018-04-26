1. 获取code

`https://open.weixin.qq.com/connect/qrconnect?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect`

用户允许授权, 重定向到redirect_url?code=CODE&state=STATE;
用户禁止授权, 重定向到redirect_url?state=STATE

2. 通过code获取access_token和openid

`https://api.weixin.qq.com/sns/oauth2/access_token?appid=APPID&secret=SECRET&code=CODE&grant_type=authorization_code`

返回结果:

```
{
	"access_token":"84位字符串",
	"expires_in":7200,
	"refresh_token":"",
	"openid":"28位字符串",
	"scope":"snsapi_login",
	"unionid":"28位字符串"
}
```

3. 通过access_token和openid获取userinfo

`https://api.weixin.qq.com/sns/userinfo?access_token=ACCESS_TOKEN&openid=OPENID`

返回结果:

```
{	
	"openid":"28位字符串",
	"nickname":"tony",
	"sex":1,
	"language":"en",
	"city":"Guangzhou",
	"province":"Guangdong",
	"country":"CN",
	"headimgurl":"img url",
	"privilege":[],
	"unionid":"28位字符串"
}
```


html 页面显示二维码

```
<!doctype html>
<html>
<head>
	<title>wx login</title>
	<meta charset="utf-8" />
	<script src="http://res.wx.qq.com/connect/zh_CN/htmledition/js/wxLogin.js"></script>
</head>
<body>
	<div id="qr-container"></div>
	<script>
	var obj = new WxLogin({
		id:"qr-container",
		appid:"your appid",
		scope:"snsapi_login",
		redirect_uri:"https://domain.com/xxx/fuck",
		state:Date.parse(new Date()),
		style:"",
		href:"https://domain.com/static/css/qrcode.css"
	});
	</script>
</body>
</html>
```

php 页面显示二维码

```
$redirect_uri = "https://domain.com/xxx/fuck";
$redirect_uri = urlencode($redirect_uri);

$appID = "your appid";
$scope = "snsapi_login";
$state = uniqid();

$url = "https://open.weixin.qq.com/connect/qrconnect?appid={$appID}&redirect_uri={$redirect_uri
}&response_type=code&scope={$scope}&state={$state}#wechat_redirect";

$result = file_get_contents($url);
$result = str_replace("/connect/qrcode/", "https://open.weixin.qq.com/connect/qrcode/", $result);
echo $result;

function get_userinfo()
{
    $code = $_GET["code"];
    $appid = "your appid";
    $secret = "your secret";
    if ( ! empty($code) ) {
		$url = "https://api.weixin.qq.com/sns/oauth2/access_token?appid={$appid}&secret={$secret}&code={$code}&grant_type=authorization_code";
        $jsonResult = file_get_contents($url);
        $resultArray = json_decode($jsonResult, true);
        $access_token = $resultArray["access_token"];
        $openid = $resultArray["openid"];
        $infoUrl = "https://api.weixin.qq.com/sns/userinfo?access_token={$access_token}&openid={$openid}";
        $infoResult = file_get_contents($infoUrl);
        $infoArray = json_decode($infoResult, true);
	}
}
```
