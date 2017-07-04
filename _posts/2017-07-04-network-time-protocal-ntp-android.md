---
layout: post
title: "Network Time Protocal (ntp) Android"
description: ""
category: ntp
tags: [iOS, android]
---

# Source Code

<https://github.com/AndroidDevLog/NetworkTimeProtocol.git>

# How to Use

<http://mvnrepository.com/search?q=+Apache+Commons+Net>

Android Studio *Cmd + ;* add Library Dependency, Search *commons-net*, You can find *commons-net:commons-net:20030805.205232*

![commons-net](/assets/images/Android/ntp/commons-net.png)

Open the follow link to Find the last version, *3.6* not *20030805.205232*.


<http://mvnrepository.com/artifact/commons-net/commons-net>

打开 build.gradle (Module:app)

```
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:26.+'
    compile 'com.android.support.constraint:constraint-layout:1.0.2'
    testCompile 'junit:junit:4.12'
```

添加这行

```
    compile 'commons-net:commons-net:3.6'
```

结束行

```
}
```

# Network Time Protocol (ntp)
---

Android

```java
public class Util {
    //NTP server list: http://tf.nist.gov/tf-cgi/servers.cgi
    public static final String TIME_SERVER = "pool.ntp.org";

    public static long getCurrentNetworkTime() {
        NTPUDPClient lNTPUDPClient = new NTPUDPClient();
        lNTPUDPClient.setDefaultTimeout(3000);
        long returnTime = 0;
        try {
            lNTPUDPClient.open();
            InetAddress lInetAddress = InetAddress.getByName(TIME_SERVER);
            TimeInfo lTimeInfo = lNTPUDPClient.getTime(lInetAddress);
            // returnTime =  lTimeInfo.getReturnTime(); // local time
            returnTime = lTimeInfo.getMessage().getTransmitTimeStamp().getTime();   //server time
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lNTPUDPClient.close();
        }

        return returnTime;
    }
}
```

# Unit Test

```java
public class NtpUnitTest {
    @Test
    public void ntp_isCorrect() throws Exception {
        long longTime = Util.getCurrentNetworkTime();
        Date date = new Date(longTime);
        // You can specify styles if you want
        DateFormat format = DateFormat.getDateInstance();
        // Set time zone information if you want.
        String text = format.format(date);

        System.out.print(text);

        Calendar calendar = new GregorianCalendar();
        calendar.setTime(date);
        int year = calendar.get(Calendar.YEAR);
        //Add one to month {0 - 11}
        int month = calendar.get(Calendar.MONTH) + 1;
        int day = calendar.get(Calendar.DAY_OF_MONTH);
        int hour = calendar.get(Calendar.HOUR_OF_DAY);
        int minute = calendar.get(Calendar.MINUTE);
        int second = calendar.get(Calendar.SECOND);
        int millis = calendar.get(Calendar.MILLISECOND);

        assertEquals(2017, year);
        assertEquals(7, month);
        assertEquals(4, day);
        assertEquals(12, hour);
//        assertEquals(6, minute);
    }
}
```
