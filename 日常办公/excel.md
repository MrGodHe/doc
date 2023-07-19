## VLOOKUP函数

语法：VLOOKUP( lookup_value,  table_array,  col_index_num,  [range_lookup])

| **参数**      | **简单说明**                 | **输入数据类型**                                             |
| ------------- | ---------------------------- | ------------------------------------------------------------ |
| lookup_value  | 要查找的值                   | 数值、引用或文本字符串                                       |
| table_array   | 要查找的区域                 | 数据表区域                                                   |
| col_index_num | 返回数据在查找区域的第几列数 | 正整数                                                       |
| range_lookup  | 精确匹配/近似匹配            | FALSE（0、空格或不填（但是要有','占位)）/TRUE（1或不填(无逗号占位)） |

例子：VLOOKUP(D1, Sheet2!$A$1:$B$518 ,2,  FALSE)

