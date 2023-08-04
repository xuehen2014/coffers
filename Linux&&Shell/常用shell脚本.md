- 输入开始日期与结束日期，遍历所有日期并执行脚本
  ```bash
  #!/bin/bash
  if [ -n "$1" ] ;then
    echo "开始日期为 $1"
    start_date=`date -d "$1" +%s`
  else
    echo "warn: 请输入一个开始时间,格式YYYY-MM-dd"  
    exit 1
  fi
  
  if [ -n "$2" ] ;then
    echo "结束日期为 $2"
    end_date=`date -d "$2" +%s`
  else
    echo "warn: 请输入一个结束日期,格式YYYY-MM-dd"
    exit 1
  fi
  
  #start_date=`date -d "2022-04-01" +%s`
  #end_date=`date -d "2022-04-15" +%s`
  
  echo "开始执行......"
  
  while [ $start_date -le $end_date ]
  do
    date_str=`date -d @$start_date "+%Y-%m-%d"`
    echo $date_str
    echo `date "+%Y-%m-%d %H:%M:%S"`
    sleep 5s
    su - sync360 -c "sh xxx.sh --inputDate=${date_str}"
    echo `date "+%Y-%m-%d %H:%M:%S"`
    let start_date=$start_date+24*60*60
  done
```
