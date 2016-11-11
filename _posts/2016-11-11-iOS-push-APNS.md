---
layout: post
title: "iOS 推送"
description: "iOSDevLog"
category: 
tags: [iOSDevLog]
---
{% include JB/setup %}

## keyword

* Development SSL Certificate： 连接 XCode 调试的证书
* Production SSL Certificate：  Ad-Hoc/App Store等ipa安装的证书
* Debug:                        包含调试信息
* Release:                      不包含调试信息


## iOS 推送

<https://developer.apple.com/account/ios/certificate/>

XCode 8 自己管理证书

![1](/assets/images/iOS/push/1.png)

![2](/assets/images/iOS/push/2.png)

![3](/assets/images/iOS/push/3.png)

![4](/assets/images/iOS/push/4.png)

![5](/assets/images/iOS/push/5.png)

![6](/assets/images/iOS/push/6.png)

![7](/assets/images/iOS/push/7.png)

![8](/assets/images/iOS/push/8.png)

![9](/assets/images/iOS/push/9.png)

![10](/assets/images/iOS/push/10.png)

![11](/assets/images/iOS/push/11.png)

![12](/assets/images/iOS/push/12.png)

![13](/assets/images/iOS/push/13.png)

![14](/assets/images/iOS/push/14.png)

![15](/assets/images/iOS/push/15.png)

![16](/assets/images/iOS/push/16.png)

![17](/assets/images/iOS/push/17.png)

```bash
$ ls
$ openssl pkcs12 -clcerts -nokeys -out apns-dev-cert.pem -in apns-dev-cert.p12
$ openssl pkcs12 -nocerts -out apns-dev-key.pem -in apns-dev-key.p12
$ cat apns-dev-cert.pem apns-dev-key.pem > apns-dev.pem
$ openssl s_client -connect gateway.sandbox.push.apple.com:2195 -cert apns-dev-cert.pem -key apns-dev-key.pem
$ openssl pkcs12 -clcerts -nokeys -out apns-dis-cert.pem -in apns-dis-cert.p12
$ openssl pkcs12 -nocerts -out apns-dis-key.pem -in apns-dis-key.p12
$ cat apns-dis-cert.pem apns-dis-key.pem > apns-dis.pem
$ openssl s_client -connect gateway.push.apple.com:2195 -cert apns-dis-cert.pem -key apns-dis-key.pem
$ ls
```

## 测试

```php
<?php

$deviceToken = '';//这里填写设备的Token码
$passphrase = ''; //证书密码
//推送的消息 ;
$message = ''; //要发送的标题
////////////////////////////////////////////////////////////////////////////////
//如果在Windows的服务器上，寻找pem路径会有问题，路径修改成这样的方法：
//$pem = dirname(__FILE__) . '/' . 'apns-dev.pem';
//linux 的服务器直接写pem的路径即可
$ctx = stream_context_create();
stream_context_set_option($ctx, 'ssl', 'local_cert', 'ck.pem');//ck文件
stream_context_set_option($ctx, 'ssl', 'passphrase', $passphrase);

// Open a connection to the APNS server
/*
/此处有两个服务器需要选择，如果是开发测试用，选择第二名sandbox的服务器并使用Dev的pem证书，
如果是正是发布，使用Product的pem并选用正式的服务器	
*/

//正式环境的地址
$fp = stream_socket_client(
    'ssl://gateway.push.apple.com:2195', $err,
    $errstr, 60, STREAM_CLIENT_CONNECT|STREAM_CLIENT_PERSISTENT, $ctx);

//测试环境
// $fp = stream_socket_client(
//     'ssl://gateway.sandbox.push.apple.com:2195', $err,
//     $errstr, 60, STREAM_CLIENT_CONNECT|STREAM_CLIENT_PERSISTENT, $ctx);

if (!$fp)
    exit("Failed to connect: $err $errstr" . PHP_EOL);

echo 'Connected to APNS' . PHP_EOL;

// Create the payload body
$body['aps'] = array(
    'alert' => $message,
    'sound' => 'default',
    'badge' => '12'
    );

// Encode the payload as JSON
$payload = json_encode($body);

// Build the binary notification
$msg = chr(0) . pack('n', 32) . pack('H*', $deviceToken) . pack('n', strlen($payload)) . $payload;

// Send it to the server
$result = fwrite($fp, $msg, strlen($msg));

if (!$result)
    echo 'Message not delivered' . PHP_EOL;
else
    echo 'Message successfully delivered' . PHP_EOL;

// Close the connection to the server
fclose($fp);
    
?>
```
