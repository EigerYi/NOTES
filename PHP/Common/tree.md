# 无限级分类 #

两种分组方式
* [子集分类](#makeTree)
* [同级分类](#getTree)

### 数组

```php
$list = [
    ['id' => 1, 'name' => 'A', 'parent' => 0],
    ['id' => 2, 'name' => 'B', 'parent' => 0],
    ['id' => 3, 'name' => 'A.1', 'parent' => 1],
    ['id' => 4, 'name' => 'A.1.1', 'parent' => 3],
    ['id' => 5, 'name' => 'A.1.2', 'parent' => 3],
    ['id' => 6, 'name' => 'B.1', 'parent' => 2],
];
```


### <a name="makeTree">子集分类</a> ###
```php
/**
 * 无线级分类 子级
 * @param $list         数据
 * @param string $pk    主键字段
 * @param string $pid   关联字段
 * @param string $child 子集名称
 * @param int $init     初始id
 * @return array
 */
function makeTree($list, $pk = 'id', $pid = 'parent', $_child = '_child', $init = 0)
{
    $tree = array();
    foreach ($list as $key => $val) {
        if ($val[$pid] == $init) {
            //获取当前$pid所有子类
            unset($list[$key]);
            if (!empty($list)) {
                $child = makeTree($list, $pk, $pid, $_child, $val[$pk]);
                if (!empty($child)) {
                    $val[$_child] = $child;
                }
            }
            $tree[] = $val;
        }
    }
    return $tree;
}
```
>调用后生成数据
>```js
>JSON格式
>[
>	{
>		"id": "1",
>		"name": "A",
>		"parent": "0",
>		"_child": [
>			{
>				"id": "3",
>				"name": "A.1",
>				"parent": "1",
>				"_child": [
>					{"id": "4", "name": "A.1.1", "parent": "3"},
>					{"id": "5", "name": "A.1.2", "parent": "3"}
>				]
>			}
>		]
>	},
>	{
>		"id": "2",
>		"name": "B",
>		"parent": "0",
>		"_child": [
>			{"id": "6", "name": "B.1", "parent": "2"}
>		]
>	}
>]
>```

### <a name="getTree">同级分类</a> ###
```php
/**
 * 无线级分类 同级
 * @param $list         数据
 * @param string $pk    主键字段
 * @param string $pid   关联字段
 * @param string $rank  等级字段
 * @param int $init     初始id
 * @param int $initRank 初始等级
 * @return array
 */
function getTree($list, $pk = 'id', $pid = 'parent', $rank = 'rank', $init = 0, $initRank = 1)
{
    static $tree;
    foreach ($list as $key => $val) {
        if ($val[$pid] == $init) {
            $val[$rank] = $initRank;
            $tree[] = $val;
            unset($list[$key]);
            getTree($list, $pk, $pid, $rank, $val[$pk], $initRank + 1);
        }
    }
    return $tree;
}
```
>调用后生成数据
>```js
>JSON格式
>[
>	{"id": "1","name": "A","parent": "0","rank": "1"},
>	{"id": "3","name": "A.1","parent": "1","rank": "2"},	
>	{"id": "4","name": "A.1.1","parent": "3","rank": "3"},
>	{"id": "5","name": "A.1.2","parent": "3","rank": "3"},
>	{"id": "2","name": "B","parent": "0","rank": "1"},
>	{"id": "6","name": "B.1","parent": "2","rank": "2"}
>]
>```

