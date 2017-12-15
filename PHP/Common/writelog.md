## 写日志 ##

```php
/**
 * 写日志
 * 调用方式   writelog(['msg'=>'说明文字','order_info'=>[]]);
 * @param type $data
 */
function writelog($data = [])
{
    $dir = APPPATH . "logs/";
    if (!is_dir($dir)) {
        @mkdir($dir);
    }
    $logname = $dir . date("Ymd") . ".log";
    $date = date("Y-m-d H:i:s") . "=>";
    $debug_backtrace = debug_backtrace();
    $res = [
        'file' => $debug_backtrace[0]['file'],
        'line' => $debug_backtrace[0]['line'],
        'class' => isset($debug_backtrace[1]['class']) ? $debug_backtrace[1]['class'] : '',
        'func' => isset($debug_backtrace[1]['function']) ? $debug_backtrace[1]['function'] : '',
        //'args' => isset($debug_backtrace[0]['args']) ? $debug_backtrace[0]['args'] : '',
    ];
    $res = array_merge($res, $data);
    file_put_contents($logname, $date . var_export($res, TRUE) . "\r\n", FILE_APPEND);
}
```
