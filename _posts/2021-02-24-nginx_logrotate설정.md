---
layout: post
title:  "logrotate 설정"
date:   2021-02-24 01:40
category: category1
icon: www
keywords: tag1, tag2
image: /1.kafkaSetting/kafka_setting.png
preview: 1
---

## Logroate
logrotat는 시스템로그를 자동으로 rotate, 압축, 이메일 전송을 가능하게함. 설정에 따라 cron 기능을 이용하여 매일, 매주, 매달 로그 관리가 가능!
-> /etc/cron.daily를 이용하여 일마다 로그 관리가 가능...

우선,logrotate를 설치하자,
`sudo apt-get install logrotate`


그 다음에, `sudo vi /etc/logrotate.d/nginx`명령어를 통하여 nginx 파일을 해당 경로에 만들고. 아래 코드를 추가.

~~~
[로그가 위치하는 경로]/*.log {
    daily
    missingok
    copytruncate
    rotate 30
    dateext
    notifempty
    sharedscripts
    postrotate
        [ ! -f [로그위치경로]/nginx.pid ] || kill -USR1 `cat [로그위치경로]nginx.pid`
    endscript
}
~~~

마지막으로 아래 명령어를 이용하여 변경된 값을 적용해줌.
`sudo logrotate -f /etc/logrotate.d/nginx` 
