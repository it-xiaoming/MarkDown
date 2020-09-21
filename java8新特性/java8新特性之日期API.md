# java8新特性之日期API

### 一、LocalDate

### 二、LocalTime

### 三、LocalDateTime

```java
//获取系统当前时间
LocalDateTime now = LocalDateTime.now();

//获取系统当前时间的年、月、日、时、分、秒
int year = now.getYear();
int monthValue = now.getMonthValue();
int dayOfMonth = now.getDayOfMonth();
int hour = now.getHour();
int minute = now.getMinute();
int second = now.getSecond();

//加年、月、日、时、分、秒
LocalDateTime localDateTime 			     =now.plusYears(2).plusMonths(2).plusDays(2).plusHours(2).plusMinutes(2).plusSeconds(2);

//减年、月、日、时、分、秒
LocalDateTime localDateTime1 = now.minusYears(2).minusMonths(2).minusDays(2).minusHours(2).minusMinutes(2).minusSeconds(2);
```

### 四、Instant--[时间戳]

```java
//默认获取UTC时区
Instant instant = Instant.now()
//获取毫秒值
long l = instant.toEpochMilli();
```

#### Duration

> Duration：计算两个“时间”之间的间隔

```java
//默认获取UTC时区
Instant instant = Instant.now();
try {
    Thread.sleep(1000);
} catch (InterruptedException e) {
}
Instant instant2 = Instant.now();
Duration between = Duration.between(instant, instant2);
System.out.println(between.toMillis());
```

```java
LocalDateTime now = LocalDateTime.now();
try {
    Thread.sleep(1000);
} catch (InterruptedException e) {
}
LocalDateTime now1 = LocalDateTime.now();
Duration between = Duration.between(now, now1);
System.out.println(between.toMillis());
```

#### Period

> Period：计算两个“日期”之间的间隔

```java
LocalDate now = LocalDate.of(2015,1,2);
LocalDate now1 = LocalDate.now();
Period between = Period.between(now, now1);
System.out.println(between.getYears()+"年"+between.getMonths()+"月"+ between.getDays()+"日");
```

#### TemporalAdjuster

> 时间矫正器

```java
LocalDateTime ldt = LocalDateTime.now();
LocalDateTime with = ldt.with(TemporalAdjusters.lastDayOfMonth());
```

#### DateTimeFormatter

> 格式化时间/日期

```java
DateTimeFormatter dtf = DateTimeFormatter.ISO_DATE;
LocalDateTime ldt = LocalDateTime.now();
//日期格式化字符串
string date = dtf.format(ldt)
    
//自定义日期转换格式
DateTimeFormatter dtf = DateTimeFormatter.ofPartten
```

### 五、ZoneDate、ZoneTime、ZoneDateTime

```java