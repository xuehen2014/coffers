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



- 检测两台服务器指定目录下的文件一致性   
   ```bash
   #!/bin/bash  
   ######################################  
   检测两台服务器指定目录下的文件一致性  
   #####################################  
   #通过对比两台服务器上文件的md5值，达到检测一致性的目的  
   dir=/data/web  
   b_ip=192.168.88.10  
   #将指定目录下的文件全部遍历出来并作为md5sum命令的参数，进而得到所有文件的md5值，并写入到指定文件中  
   find $dir -type f|xargs md5sum > /tmp/md5_a.txt  
   ssh $b_ip "find $dir -type f|xargs md5sum > /tmp/md5_b.txt"  
   scp $b_ip:/tmp/md5_b.txt /tmp  
   #将文件名作为遍历对象进行一一比对  
   for f in `awk '{print 2} /tmp/md5_a.txt'`do  
   #以a机器为标准，当b机器不存在遍历对象中的文件时直接输出不存在的结果  
   if grep -qw "$f" /tmp/md5_b.txt  
   then  
   md5_a=`grep -w "$f" /tmp/md5_a.txt|awk '{print 1}'`  
   md5_b=`grep -w "$f" /tmp/md5_b.txt|awk '{print 1}'`  
   #当文件存在时，如果md5值不一致则输出文件改变的结果  
   if [ $md5_a != $md5_b ]then  
   echo "$f changed."  
   fi  
   else  
   echo "$f deleted."  
   fi  
   done  
   
   ```
