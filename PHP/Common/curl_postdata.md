# 发送GET/POST请求 #

```
/**
 * curl 获取数据
 * @param type $url
 * @param type $data
 * @param type $method
 * @param type $author
 * @return type
 */
function curl_postdata($url, $data = NULL, $method = "GET", $author = NULL)
{
    $timeout = 60 * 5;
    ini_set('max_execution_time', $timeout);
    $curl = curl_init();
    curl_setopt_array($curl, array(
        CURLOPT_URL => $url,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_ENCODING => "",
        CURLOPT_MAXREDIRS => 10,
        CURLOPT_TIMEOUT => $timeout, //设置超时时间为5分钟
        CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
        CURLOPT_CUSTOMREQUEST => $method,
        CURLOPT_POSTFIELDS => http_build_query($data),
        CURLOPT_HTTPHEADER => empty($author) ? array() : $author,
        CURLOPT_USERAGENT => '', //代理
    ));
    $result = curl_exec($curl);
    $error = curl_error($curl);
    $curlinfo = curl_getinfo($curl);
    curl_close($curl);
    return array("result" => $result, "error" => $error, "code" => $curlinfo['http_code'], "curlinfo" => $curlinfo);
}
```
