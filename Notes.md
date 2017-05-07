##MySQL必知必会（6）：常用文本字符串处理函数##

编程语言中经常使用函数来处理一些字符串，数字或者其他内容。MySQL也是一样，可以使用函数来快速达到一系列的目的。

函数
MySQL支持以下常用函数：

文本处理函数
数值数据处理函数
日期时间处理函数
MySQL一些特殊信息，比如版本信息函数
本篇文章主要介绍：文本处理函数，一般用于处理字符串，比如删除、填充、转换大小写等等操作

提示： SQL语句具有较强的可移植性。比如在Mysql中使用的SQL语句，搬到postgresql中使用起来问题也不大。但是，在SQL语句中使用的函数可移植性较差，实现相同功能的函数在不同DBMS中可能完全不一样。所以如果需要移植SQL要慎重使用函数。

准备
首先我们先建立一个数据表，同时填入一些数据，下面我们将使用此表


-- ----------------------------
-- Table structure for fun_text
-- ----------------------------
DROP TABLE IF EXISTS `fun_text`;
CREATE TABLE `fun_text` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `content` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of fun_text
-- ----------------------------
INSERT INTO `fun_text` VALUES ('1', '1234567890');
INSERT INTO `fun_text` VALUES ('2', 'abc');
INSERT INTO `fun_text` VALUES ('3', 'ABC');
INSERT INTO `fun_text` VALUES ('4', '  abc  ');
INSERT INTO `fun_text` VALUES ('5', 'lee');
下面我们直接列举一些常用MySQL字符串处理函数，及其用法实例

LEFT()
用于返回字符串左侧的字符。

该函数有两个参数，第一个参数为字符串，第二个参数为字符串长度。字符串长度为2，就从字符串的左侧截取两个字符串返回。

SELECT id,LEFT(content,3) AS left_content FROM fun_text WHERE id='1'


我们从上表中返回了id=1的记录的content字段的左侧3个字符

TIP: 该函数还有相似函数RIGHT()用于返回字符串右侧的字符，用法类似。

LENGTH()
这个函数很简单，返回字符串的长度。

SELECT id,LENGTH(content) AS content_length FROM fun_text WHERE id='1'


我们返回了id=1的记录的content长度为10

LOCATE()
返回一个字符串在另一个字符串中第一次出现的位置，如果没有发现就返回0.

SELECT id,LOCATE('234', content) AS locate_content FROM fun_text WHERE id='1';


之前id=1的content字段内容为1234567890，234的起始位置为2. 所以，如果在开始位置查找到，那么返回是1而不是0

LOWER()
将字符串全部转换为小写字母

我们使用id=3的记录来操作。

SELECT id,LOWER(content) AS lower_content FROM fun_text WHERE id='3';


可以看到，之前的大写ABC被转换成了小写的字母abc

TIP: 该函数还有一个相反函数UPPER()可以将小写字母均转换为大写字母

TRIM()
去除字符串两边的空格。

这次使用id=4的记录作为演示，content字段两边包含了空格。

SELECT id,TRIM(content) AS trim_content FROM fun_text WHERE id='4';


可以看到，字符串两边的空格都被去除了。

TIP: 还有另外两个相似函数：1、LTRIM()：去除字符串左侧空格；2、RTRIM()去除字符串右侧空格

SUBSTRING()
字符串截取函数。该函数与LEFT()和RIGHT()函数有点儿类似，都可以截取字符串，只不过功能更加强大。

1、从第N个字符串开始截取

SELECT id,SUBSTRING(content,5) AS sub_content FROM fun_text WHERE id='1';


我们使用id=1记录，截取content字段，并且是从第5个字符串开始截取，所以结果为567890

2、从第N个字符串开始截取，截取M个字符

SELECT id,SUBSTRING(content,5,2) AS sub_content FROM fun_text WHERE id='1';


这个方法去截取字符串是不是有更多的自由

SOUNDEX()
最后说一个不同于之前的字符处理函数SOUNDEX()，这个函数可以将一个单词的读音转换成与它相似的读音的单词。简单的来说，就是讲方言转换为普通话的意思。这个在搜索中十分的好使。

我们拿上面的id=5的记录来举个例子，记录的content值为lee，但是有个方言用户输入了lie，无论是模糊查询还是精确查询都是无法匹配到lee结果，但是使用了soundex函数则可以改变这些。

SELECT * FROM fun_text WHERE SOUNDEX(content) = SOUNDEX('lie');


观察上面的例子，lie是不等于lee的，但是SOUNDEX(content) = SOUNDEX('lie')，因为lie和lee的读音十分相似。
