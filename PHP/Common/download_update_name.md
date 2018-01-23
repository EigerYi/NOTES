# 修改下载文件名

```php
    $filename =  dirname(__FILE__) . '/oldfilename.jpg';
    $out_filename = 'newName.jpg';
    if (!file_exists($filename)) {
        echo 'Not Found' . $filename;
        exit;
    } else {
        // We'll be outputting a file
        header('Accept-Ranges: bytes');
        header('Accept-Length: ' . filesize($filename));
        // It will be called
        header('Content-Transfer-Encoding: binary');
        header('Content-type: application/octet-stream');
        header('Content-Disposition: attachment; filename=' . $out_filename);
        header('Content-Type: application/octet-stream; name=' . $out_filename);
        // The source is in filename
        if (is_file($filename) && is_readable($filename)) {
            $file = fopen($filename, "r");
            echo fread($file, filesize($filename));
            fclose($file);
        }
        exit;
    }
```
