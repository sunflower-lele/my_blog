### curl知识

#### 主要是自己在平常的开发和前辈的一些指点中，遇到的一些问题，怕记不住，就用这种形式记录下来，等忘记的时候，过来翻看一下。

#### curl基础例子

只要php开启了curl扩展，就可以使用curl函数了。使用curl函数的基本思想是先使用curl_init()初始化curl会话，
然后可以通过curl_setopt()设置需要的全部选项，然后再使用curl_exec()来执行会话，当执行完会话后使用curl_close()
关闭会话。这是一个完整的curl使用过程。

```
<?php
    $ch = curl_init("http://www.example.com/");
    $fp = fopen("example_homepage.txt", "w");
    
    curl_setopt($ch, CURLOPT_FILE, $fp);
    curl_setopt($ch, CURLOPT_HEADER, 0);
    
    curl_exec($ch);
    curl_close($ch);
    fclose($fp);
    ?>
```

下面来说一说我今天遇到的一种情况：接入方发现回调失败，经过日志排查，发现问题是：SSL read: error:00000000:lib(0):func(0):reason(0), errno 104，
百度以后发现是SSL认证的问题。

https服务器post数据

```
function curlPost($url, $data, $timeout = 15)
{
    $ssl = substr($url, 0, 8) == "https://" ? TRUE : FALSE;
    $ch = curl_init();
    $opt = array(
            CURLOPT_URL     => $url,
            CURLOPT_POST    => 1,
            CURLOPT_HEADER  => 0,
            CURLOPT_POSTFIELDS      => (array)$data,
            CURLOPT_RETURNTRANSFER  => 1,
            CURLOPT_TIMEOUT         => $timeout,
            );
    if ($ssl)
    {
        $opt[CURLOPT_SSL_VERIFYHOST] = 1;
        $opt[CURLOPT_SSL_VERIFYPEER] = FALSE;
    }
    curl_setopt_array($ch, $opt);
    $data = curl_exec($ch);
    curl_close($ch);
    return $data;
}
$data = curlPost('https://www.php230.com', array('p'=>'hello'));
echo ($data);
```

以上代码是说服务器不进行SSL认证，并不是真的走HTTPS
如果要真的使用https，需要提供CA证书
关于SSL部分设置如下：

```
01.CURLOPT_SSL_VERIFYPEER 设置为 true ，说明进行SSL证书认证  
02.CURLOPT_SSL_VERIFYHOST 设置为 2， 说明进行严格认证  
03.CURLOPT_CAINFO 设置为证书的路径
```

重新封装的函数如下：

```
/** 
 * curl POST 
 * @param   string  url 
 * @param   array   数据 
 * @param   int     请求超时时间 
 * @param   bool    HTTPS是否进行严格认证 
 * @return  string 
 */  
function curlPost($url, $data = array(), $timeout = 15, $CA = true){    
  
    $cacert = getcwd() . '/cacert.pem'; //CA根证书  
    $SSL = substr($url, 0, 8) == "https://" ? true : false;  
      
    $ch = curl_init();  
    curl_setopt($ch, CURLOPT_URL, $url);  
    curl_setopt($ch, CURLOPT_TIMEOUT, $timeout);  
    curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, $timeout-2);  
    if ($SSL && $CA) {  
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, true);   // 只信任CA颁布的证书  
        curl_setopt($ch, CURLOPT_CAINFO, $cacert); // CA根证书（用来验证的网站证书是否是CA颁布）  
        curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 2); // 检查证书中是否设置域名，并且是否与提供的主机名匹配  
    } else if ($SSL && !$CA) {  
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false); // 信任任何证书  
        curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 1); // 检查证书中是否设置域名  
    }  
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);  
    curl_setopt($ch, CURLOPT_HTTPHEADER, array('Expect:')); //避免data数据过长问题  
    curl_setopt($ch, CURLOPT_POST, true);  
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data);  
    //curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data)); //data with URLEncode  
  
    $ret = curl_exec($ch);  
    //var_dump(curl_error($ch));  //查看报错信息  
  
    curl_close($ch);  
    return $ret;    
} 
```

如果url是https的话，就走ssl，否则就走普通的http协议。

#### 更为安全的验证

公用名(CN:common name)：就是申请ssl证书所需要的域名和子域名。
如果ssl证书买的是CA的，那么访问时是可以使用比较严格的认证。举例如下：

```
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, true);   // 只信任CA颁布的证书  
curl_setopt($ch, CURLOPT_CAINFO, $cacert); // CA根证书（用来验证的网站证书是否是CA颁布）  
curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 2); // 检查证书中是否设置域名，并且是否与提供的主机名匹配 
```

如果证书是自己生成的，或者是小机构申请的，那么严格认证是无法通过的，会返回false（此时可以打印curl_error($ch)查看具体的错误信息）
那么我们可以降低验证程度来保持正常访问。距离如下：

```
curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 1);//检验证书中是否设置域名(0 表示域名不论存在与否都不进行验证)
```

平时我们使用浏览器访问网站时，会有证书不受信任的提示，就是因为这些网站的证书不是正规的CA颁布的。




