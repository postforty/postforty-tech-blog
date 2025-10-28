인증서 발급을 위해 chat gpt와 구글링을 통해 발급 방법을 알아보았지만 유튜브의 무료 강의가 더 도움이 되었다. 참고한 영상은 본문에 링크로 표시해 두었다.

## 1\. Let's Encrypt로 TLS(Transport Layer Security) 인증서 발급

우분투에서 진행하였고 certbot이 이미 설치된 상태에서 진행하였다. 그리고 nginx 서버를 사용하고 있다. TLS 인증서 발급 시 리룩스 명령어는 다음과 같은 순서로 사용하였다.

※ Let's Encrypt 설치는 [https://postforty.tistory.com/437](https://postforty.tistory.com/437) 참조!

1.  리눅스 정보 확인(이 부분은 생략해도 됨) :  hostnamectl
2.  기 발급된 인증서 확인(최초 발급이라면 생략) : certbot certificates
3.  인증서 발급 전 웹서버 중지 : service nginx stop
    1.  포트 80 및 443을 사용하는 프로세스 확인
        1.  sudo netstat -tulpn | grep :80
        2.  sudo netstat -tulpn | grep :443
    2.  강제 중지(중지 안될 시 사용) : sudo pkill -f nginx
4.  인증서 생성(-d로 하나 이상의 도메인 추가 가능) : letsencrypt certonly --standalone -d 도메인1 -d 도메인2 -d 도메인3
    - 이메일 인증 요구 시 이메일 입력하고, 모든 질문에 긍정 답변한다.
    - \[그림 1\]과 같이 "Certbot failed to authenticate some domains (authenticator: standalone). The Certificate Authority reported these problems:"가 발생 경우는 DNS가 인증서 발급을 시도하는 IP와 연결되어 있는지 확인해봐야 한다.
5.  발급 후 웹서버 다시 시작 : service nginx start

위 과정을 마친 후에 /etc/nginx/site-available의  default에 발급받은 pem 키를 적용해 주었다.

[##_Image|kage@cSJ53k/btsFE0Ur7NF/AAAAAAAAAAAAAAAAAAAAACKX6r2UkRVA2vgm5owh1g0kz29_5i259t1GL4jV9P8-/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1761922799&amp;allow_ip=&amp;allow_referer=&amp;signature=jNXUNsmUb3%2BoA5rKXmWbvmQktZY%3D|CDM|1.3|{"originWidth":833,"originHeight":375,"style":"alignCenter","caption":"[그림 1] 인증서 발급 실패한 경우"}_##]

> 인증서 발급 : [https://youtu.be/Xah-BtVfHac?feature=shared](https://youtu.be/Xah-BtVfHac?feature=shared)

## 2\. 인증서 갱신

1.  갱신 테스트 : certbot renew --dry-run     📢 nginx를 끄고 테스트해야 한다(명령어 : service nginx stop).
2.  갱신 : certbot renew

## 3\. 인증서 자동 갱신

Let's Encrypt는 만료 30일 전부터 갱신이 가능하다. 갱신 가능 횟수는 무제한이다.

### 1\. 자주 사용하는 명령어

crontab을 이용하여 인증서를 자동 갱신할 수 있다. 자주 사용하는 명령어는 다음과 같다.

- 목록 조회 : crontab -l
- 생성, 편집 : crontab -e
- 삭제 : crontab -r
- 실행 로그 보기 : view /var/log/syslog

### 2\. crontab 작성 규칙

crontab 작성 규칙은 \[그림 2\]와 같다. 시간은 서버의 시간을 고려하여 설정해야 한다.

- 서버 시간 확인 : date

갱신 갱신 시간은 사용자의 서 요청이 가장 적은 시간대로 예상되는 월요일 1~3시 사이로 설정하는 것이 좋을 것이다.

[##_Image|kage@bX6mQV/btsDxjawm4l/AAAAAAAAAAAAAAAAAAAAAGvYv_RvkYm6XvhSWN3mr0FpbyHSYEo7KOnKYc9j_xFR/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1761922799&amp;allow_ip=&amp;allow_referer=&amp;signature=YI4rtfDDO4%2FK0M3lfAFannb8ysM%3D|CDM|1.3|{"originWidth":652,"originHeight":185,"style":"alignCenter","caption":"[그림 2] crontab 규칙"}_##]

### 3\. 적용해 본 설정

나의 경우 아래와 같이 crontab을 작성했다. 매주 월요일 오전 3시에 실행된다.

> 0 3 \* \* 1 /usr/bin/certbot renew --quiet --pre-hook "/usr/sbin/service nginx stop" --post-hook "/usr/sbin/service nginx start"

- 분 (0): cron 작업이 실행될 분을 나타냄. 여기서는 0으로 설정되어 있으므로 정확한 시간의 0분에 실행
- 시 (3): cron 작업이 실행될 시간을 나타냄. 여기서는 3으로 설정되어 있으므로 오전 3시에 실행
- 일 (\*): cron 작업이 실행될 일(day)을 나타냄
- 월 (\*): cron 작업이 실행될 달(month)을 나타냄
- 요일 (1): cron 작업이 실행될 요일을 나타냄. 여기서는 1로 설정되어 있어 월요일에 실행(요일은 0부터 일요일)
- /usr/bin/certbot renew --quiet: certbot을 사용하여 SSL/TLS 인증서를 갱신하고, --quiet 플래그를 통해 출력을 최소화
- \--pre-hook "/usr/sbin/service nginx stop": 인증서 갱신을 시작하기 전에 실행되는 명령, Nginx를 중지
- \--post-hook "/usr/sbin/service nginx start": 인증서 갱신이 완료된 후에 실행되는 명령, Nginx를 다시 시작

📢 certbot renew --dry-run 명령어로 테스트해 보니 에러(unexpected error: Problem binding to port 80: Could not bind to IPv4 or IPv6.. Skipping.)가 발생했다. 그래서 --pre-hook, --post-hook를 추가했다.

crontab 작성, 저장 후 재시작한다.

- cron 재시작 : service cron restart

> 인증서 자동 갱신 : [https://www.youtube.com/watch?v=CcLL8hx3n0M](https://www.youtube.com/watch?v=CcLL8hx3n0M)

---

2025-06-23 추가

Let's Encrypt 인증서는 도메인 소유권을 확인하는 방식으로 동작한다. DNS 챌린지 또는 HTTP 챌린지를 통해 해당 도메인에 대한 제어 권한이 있음을 증명해야 인증서 발급이 가능하다. IP 주소는 이러한 소유권 증명 방식에 적합하지 않기 때문이다. 따라서 IP 주소만으로는 Let's Encrypt 인증서를 발급받을 수 없다.

만약 IP 주소에 직접 SSL/TLS를 적용하고 싶다면, Let's Encrypt가 아닌 다른 방법을 고려해야 할 것이다. 예를 들어, 자체 서명(Self-signed) 인증서를 사용하거나, IP 주소 기반의 SSL/TLS를 지원하는 상용 인증서를 구매하는 방법이 있다. 하지만 이 경우에도 브라우저 경고 없이 신뢰성을 확보하기는 어려울 수 있다.
