- 日期&时间

```
date
#若是不以加号作为开头，则表示要设定时间
date +"%Y-%m-%d %H:%M:%S"
#日期前一天
date -d "1 day ago" +"%Y-%m-%d"

date -d "+1 day" +"%Y-%m-%d"

#2秒钟后
date -d "2 second" +"%Y-%m-%d %H:%M:%S"

date -d "1970-01-01 1234567890 seconds" +"%Y-%m-%d %H:%M:%S"

#时间格式转换
date -d "2009-12-12" +"%Y/%m/%d %H:%M.%S"

#输出时间戳
date -d "2022-10-01" +"%s"
```
