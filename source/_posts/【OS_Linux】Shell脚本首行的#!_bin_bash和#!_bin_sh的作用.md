---
title: '【OS_Linux】Shell脚本首行的#!/bin/bash和#!/bin/sh的作用'
date: 2020-09-09 07:26:00
categories:
  - OS_Linux
------

“#!” 是一个约定的标记，它告诉系统用什么解释器来执行这个脚本，即使用哪一种Shell。

#!/bin/sh是指此脚本使用/bin/sh来解释执行，其实第一句的#!是对脚本的解释器程序路径，脚本的内容是由解释器解释的，我们可以用各种各样的解释器来写对应的脚本。

比如说/bin/csh脚本，/bin/perl脚本，/bin/awk脚本，/bin/sed脚本，甚至/bin/echo等等。

#!/bin/bash同理。

区别：#!/bin/sh是#!/bin/bash的缩减版。

第一行写成 #!/bin/bash --posix，那么脚本执行效果跟#!/bin/sh是一样的

参考博文：

[Shell脚本中：#!/bin/bash和#!/bin/sh是什么意思以及区别？](https://blog.csdn.net/wzk646795873/article/details/91045172)

[写好第一个Shell脚本#!/bin/bash作用](https://blog.csdn.net/h330531987/article/details/78172898)
