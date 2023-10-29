# Stick Session 이 나오게 된 배경

- 대용량 트래픽을 처리하기 위해 Scale-out 을 한 후에는 [[Load Balancing|로드밸런싱]]을 해야 한다.
- 문제는 한 요청을 [[Load Balancing|로드밸런싱]]을 한 결과가 매번 같을 수는 없다는 것이다.
	- 아래 그림처럼 처음에는 1번 서버로 갔다가, 두번째는 3번 서버로 요청이 갈 수도 있다.
	- 이러면 1번 서버에서 로그인을 한 후 세션 정보가 저장됐는데, 다음 요청은 3번 서버로 가버려서 로그인을 또 해야하는 상황이 벌어질 수 있다.

![[Stick Session - Problem.gif|500]]

# Stick Sesison 이란

> [!info] Stick Session 이란
> 특정 세션의 요청을 처음 처리한 서버로만 전송하는 것

- 첫 요청 이후 모든 요청을 특정 서버로 고정하는 방법으로 세션을 관리한다.
- Sticky Session 을 유지하기 위해 Cookie를 사용하거나 클라이언트의 IP tracking하는 방식이 있다.

# Stick Session 단점

- 특정 서버만 과부하가 걸릴 수 있다.
- 특정 서버에서 문제가 발생하면 서버에 붙어있는 세션 전체에 문제가 생긴다.

# Stick Session 해결 방안

## Session 을 하나로 묶어 클러스터로 관리

![[Stick Session - Session Cluster.gif|500]]

- 각 WAS들은 세션을 각각 가지고 있지만, 이를 하나로 묶어 하나의 클러스터로 관리하는 방법이 있다.
- 이 상태에서 하나의 WAS가 fail이 발생하면 해당 WAS가 들고 있던 세션은 다른 WAS로 이동되어 관리된다.

- 다만, 각 서버마다 세션 클러스터링 방식이 다르고 지원하는 방식이 다르기 때문에 현재 사용하고 있는 WAS의 session clusturing 부분을 보고 확인해야 한다.

- 그리고 이 방식은 새로운 서버가 하나 추가될 때마다 기존에 존재하던 WAS에 새로운 서버의 IP/PORT를 입력해서 클러스터링 해줘야 하는 단점이 있다.

## Session Server 분리

![[Stick Session - Session Distribution.png|500]]

- Session 을 서버에서 분리하는 방식이다.
- 요청이 올때마다 서버에서는 Session 을 따로 저장하고 이는 DB 로 가서 세션 정보를 가져오게 된다.

- 새로운 서버를 생성하더라도 해당 서버에만 세션 서버의 정보를 적어주고 연결해주면 되기 때문에 기존 서버의 수정이 발생하지 않는다.

- 다만 세션 서버의 중요성이 올라가고, 세션 서버가 죽는 순간 모든 세션이 사라지기 때문에 세션 서버의 다중화를 고려해야 한다.

# Reference

- https://help-kakaoicloud.kakaoenterprise.com/hc/ko/articles/13528936793753-Sticky-Session은-어떤-기능인가요-
- https://kchanguk.tistory.com/146