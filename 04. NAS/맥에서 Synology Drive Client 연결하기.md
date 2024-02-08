---
tags:
  - NAS
  - Synology
  - 문제해결
---
맥에 Synology Drive Client 를 설치한다음, 로그인 화면에서 로그인하려 하니 연결 실패가 되면서 안될 수 있다.

이건 시놀로지 나스에서 Synology Drive Client 의 포트(6690)에 관한 작업이 수행되지 않아서 그렇다.

### 1. Firewall Rule 에서 포트 6690을 허용한다.

포트를 직접 허용하던, 애플리케이션 선택으로 허용하던 허용한다.

![[맥에서 Synology Drive Client 연결하기 - 그림 1.png]]

### 2. 공유기에서 port-forwarding 을 한다.

나는 DDNS 으로 Synology Drive Client 를 연결할것이기 때문에 아래처럼 6690 을 허용하는 과정이 필요하다.

![[맥에서 Synology Drive Client 연결하기 - 그림 2.png]]

### 3. 맥 Synology Drive Client 앱에서 아래와 같이 연결한다.

앱 비밀번호 같은건 없다. 그냥 비밀번호 입력하면 2단계 인증으로 들어간다.

![[맥에서 Synology Drive Client 연결하기 - 그림 3.png]]


