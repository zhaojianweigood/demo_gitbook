
### 时间类型和字符串类型转换
```angular2html
import datetime as dt

dt.datetime.strptime('2017-01-12T14:12:06.000-0500','%Y-%m-%dT%H:%M:%S.%f%z')
dt.datetime.strptime('2017-01-12T14:12:06.010', '%Y-%m-%dT%H:%M:%S.%f')
dt.datetime.strptime('2017-08-27 17:45:03', '%Y-%m-%d %H:%M:%S')

dt.datetime.now().strftime('%Y-%m-%d %H:%M:%S')

```

### 时区时间转换
```angular2html
from datetime import timezone
from datetime import datetime, timedelta
print(datetime.now())  # 本时区时间
utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)  # utc时区时间
print(utc_dt)
cn_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))  # 东8区时间
print(cn_dt)
_dt = utc_dt.astimezone(timezone(timedelta(hours=-8)))  # 西8区时间
print(_dt)
```

### 时间戳转换
```angular2html
import datetime as dt

timestamp = dt.datetime.timestamp(dt.datetime.now())
# 结合time模块， 获取时间戳
#now_time = dt.datetime.now()
#now_time_mk = time.mktime(now_time.timetuple())

print(timestamp)
# datetime.fromtimestamp
print(dt.datetime.fromtimestamp(timestamp))
# date.fromtimestamp
print(dt.date.fromtimestamp(timestamp))
```

### 当前周一、周五
```angular2html
import datetime as dt

dayweek = dt.datetime.now().weekday()
begin_date = (dt.datetime.now()-dt.timedelta(days=(dayweek+7))).strftime('%Y%m%d ')
end_date = (dt.datetime.now()-dt.timedelta(days=(dayweek+1))).strftime('%Y%m%d ')
print(begin_date, end_date)
```

### 
