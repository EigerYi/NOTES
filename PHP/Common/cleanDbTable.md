# 清空数据库表数据方法 #

```php
/**
 * 清除数据库
 */
public function cleanDbTable()
{
    $sql = "select table_name from information_schema.TABLES where TABLE_SCHEMA='DB_name'";
    $query = $this->db->query($sql);
    $result = $query->result_array();
    //不需要清除的数据库
    $notTable = ['am_nodes', 'am_nodes_cate', 'bmm_employees', 'um_company', 'nm_sms_template', 'xtm_bank'];
    foreach ($result as $v) {
        if (!in_array($v['table_name'], $notTable)) {
            $sql = "truncate {$v['table_name']}";
            $this->db->query($sql);
        }
    }
}
```
