영상 : https://www.youtube.com/watch?v=JEkZi6h8LSU


![[Pasted image 20250530140427.png]]

우리는 RUM 위주로 궁금
Real User Monitoring (RUM)

https://rum-webinar.datadoghq.net/menu
- 가상 사이트로 테스트 함

이벤트에 대한 그룹핑을 아래와 같이 가능해보임
시간대별로 모니터링이 가능함

![[Pasted image 20250530141207.png]]


End to End 모니터링

## 시뮬레이션
- 세션 리플레이 (재현 시뮬레이션이 가능)
	- 사용자의 액션 흐름 확인 가능
	- 에러 부분에 대해 포인트가 찍혀있음
	- 사용자 행위 영상으로 확인 가능
	- 에러의 원인을 확인 가능
	- 프론트 - 서버 - DB 까지 전체 확인 가능
![[Pasted image 20250530141603.png]]
프론트 - 서버 - DB 까지의 전체 흐름을 확인가능
![[Pasted image 20250530141641.png]]

브라우저의 브라우저 첫 렌더링 퍼포먼스 LCP 를 확인할 수 있음
![[Pasted image 20250530141912.png]]

RUM 대시보드
- 로딩 확인
- Lagest Contentful Paint 
- FCP (그려지는 시간)
- CLS (Cumulative Paint)
- ITNP (Interaction To Next Paint)
![[Pasted image 20250530142027.png]]

여기서는
Sessions, Actions, Error 모니터링 가능
version 별, 브라우저, 접속 위치 확인 가능


## 트러블 슈팅
![[Pasted image 20250530142322.png]]

에러 로그 확인 가능
어느정도의 세션이 영향을 받았는지
바로 장애 선언하고, 담당자 배정해서 리뷰 요청 가능
브라우저(디바이스)별 확인 가능
다운로드 지연 시간 등등 확인 가능
![[Pasted image 20250530142400.png]]



## Product Analytics (데이터 모니터링)
구글 애널리틱스 같이 사용자별 사용성 로그 확인 가능
복잡한 설정 없이 모니터링 가능
- 어느 나라에서 많이 들어오는지
- 어느 뷰가 가장 사용자 이용이 많은지
- 어떤 이벤트 클릭이 좋은지
- 어떤 제품을 카트에 많이 넣는지
- 사용자별 방문 횟수
![[Pasted image 20250530143023.png]]

/cart 에 어떤 /product 가 많이 담기는지
![[Pasted image 20250530143118.png]]

사용자가 어디까지 스크롤 내리는지
사용자가 어디를 클릭을 많이 하는지
사용자가 마우스 어느 영역을 많이 돌아가는지
![[Pasted image 20250530143338.png]]

![[Pasted image 20250530143510.png]]

시각화
![[Pasted image 20250530144212.png]]
- 방문 페이지
- 이탈률
	- 이탈 원인도 볼 수 있다
- 회원가입률
- 로그인 수
- 결제 매출액
- 결제 실패 매출
![[Pasted image 20250530143704.png]]


## 비용 최적화
샘플링 퍼센트를 줄이면 된다
![[Pasted image 20250530144506.png]]

코드단에서는
- 로그인 한 경우만 수집
- 중요한 페이지 진입시에만
- 크롤러 봇 차단!!
	- https://docs.datadoghq.com/ko/real_user_monitoring/guide/identify-bots-in-the-ui/
![[Pasted image 20250530144530.png]]


필요한 세션만 선택적으로 15개월 저장하게끔 해서 비용 절감을 도움
- 로그인한 유저만 저장하겠다 조건
- 중요한 고객 유저만 저장하겠다 조건
- CEO 유저! ID 특정해서 저장하겠다 조건
![[Pasted image 20250530144650.png]]

RUM 이 아니라 로그만 수집할 수 있음
![[Pasted image 20250530144916.png]]

서비스 별, 배포시기별 을 조절할 수 있음



가격 정책
https://www.datadoghq.com/pricing/?product=real-user-monitoring#products

비용은 크게 두가지로 나누는데
- 정보를 쏘는 비용 (세션별 비용이 발생)
- 저장하는 비용 (그런데, 저장하는 비용에서 최적화 할 수 있다)
	- RUM은 별도의 비용이 발생한다




마스킹 관련 기능
안녕하세요, Datadog에서는 RUM에서 수집되는 데이터에 대해 마스킹을 하거나, Datadog에 저장되기 전에 민감데이터를 처리하여 저장할 수 있는 Sensitive Data Scanner 기능을 제공하고 있습니다.

아래 URL을 참고 부탁드립니다.

[RUM Privacy Options]
https://docs.datadoghq.com/real_user_monitoring/session_replay/browser/privacy_options/

[Sensitive Data Scanner]
https://docs.datadoghq.com/security/sensitive_data_scanner/






# Q&A
datadog agent에서 cgroup v2는 언제쯤 지원예정 일까요?

Datadog_정영석      오후 2:26
안녕하세요 박종규님, cgroup v2의 어떤 부분이 필요하실까요? K8S 환경에서는 request limit 등으로 agent의 리소스 사용량을 제어하시는데, cgroup2 의 필요하신 부분을 말씀해주시면 좀 더 확인해 보겠습니다.

재윤 우      오후 2:06
세션은 몇분동안 진행되나요?

Datadog_Jade Cho      오후 2:06
금일 웨비나는 본 세션 약 45~50분, Q&A 10분으로 1시간 예정되어 있습니다. :)

Seulgi Yoo      오후 2:16
모든 액션에 대해 레코딩 되는건가요? 레코딩 rate 제한이 있을까요?

Datadog_DeukHee Kim      오후 2:19
안녕하세요, 수행했던 내용에 대해서 레코딩이 진행됩니다. 이를 위해서는 Datadog SDK 구성이 필요하여, 사용자의 환경에 맞게 Sample Rate를 설정하실 수 있습니다.

창원 주      오후 2:16
현재 업계 동향 및 앞으로의 발전 방향에 대해서 문의드립니다

Datadog_Kihyun Lee      오후 2:23
현재 여러 Observability 솔루션이 프론트엔드 성능 모니터링을 지원하려고 하고 있으며, 그 중 Datadog에서는 이미 성능 모니터링 기능을 넘어 Replay, Product Analysis, BI 데이터, 보안 영역 등 새로운 기능을 통해 발전하고 있습니다. :)

익명 참석자      오후 2:20
이런 모니터링을 구현하기 방법도 이번 세션에서 보여주실까요..?

Datadog_DeukHee Kim      오후 2:24
세션 시간이 한정되어 있기 때문에, 구성에 대한 자세한 내용은 다루지 않습니다. 다만, 세션 후반부에 적용 스크립트에 대해 짧게 소개가 될 예정입니다.

설정 가이드 링크를 전달드리오니 참고 부탁드려요~
https://docs.datadoghq.com/real_user_monitoring/browser/setup/client?tab=rum

준성 김      오후 2:20
바로바로 따라해보고 있는데 rum/replay/sessions 에서 LCP가 측정될 때가 있고 안 될 때가 있나요?

저 같은 경우 Measures에 Cumulative Layout Shift 값만 있네요

Datadog_Jade Cho      오후 2:26
웹페이지의 특성상 지표가 계산되는 타이밍에 차이가 있을 수 있겠습니다. 혹시 현상이 지속되거나, 부정확한 것으로 보일 경우, Datadog Support로 문의해주시면 보다 정확하게 답변드릴 수 있도록 하겠습니다. :)

익명 참석자      오후 2:22
summary가 저는 뷰가 다른데, 혹시 다르게 설정할 수 있나요?

Datadog_Taeyeong Im      오후 2:25
RUM Summary 탭에서 JS, Flutter, android , ios등 구성 환경에 따라서, 위젯의 종류는 다를 수 있습니다.

익명 참석자      오후 2:28
Shopify와 연동시에도 RUM 구현이 가능할까요?

Datadog_Wudu      오후 2:31
안녕하세요, Shopfiy Store에서 RUM 연동과 관련하여 지원을 드리고 있습니다. 

페이지에서 Edit code를 통해 RUM SDK를 삽입해주시면 설정이 가능합니다.
관련하여 하단의 링크를 참고하실 수 있습니다.
https://docs.datadoghq.com/real_user_monitoring/guide/enable-rum-shopify-store/

정은 이      오후 2:28
1. 권장 SessionReplaySampleRate가 있으신지
2. SessionReplaySampleRate가 높아지면 비용에 어느 정도로 영향을 주는지

궁금합니다~!

Datadog_DeukHee Kim      오후 2:32
안녕하세요, SampleRate는 고객의 환경과 예산 등을 고려하여 측정합니다. 고객사마다 활용범위와 환경이 다르기 때문에, 이는 사용 패턴을 분석하여 결정하시는 것이 일반적입니다.

비용에 대해서는 아래 Price 페이지를 참고해주시면 도움이 되실 것 같습니다.

https://www.datadoghq.com/pricing/?produ

ct=real-user-monitoring#products

태인 김      오후 2:29
에러에 대한 해결책을 AI가 분석해주는 기능의 경우는 현재는 사용할 수 없고 추후 반영 예정인건가요?
아니면 특정 케이스에 대해서만 제시되어서 보이는 이슈가 있고 안보이는 이슈가 있는걸까요?

Datadog_Kihyun Lee      오후 2:31
에러 트래킹 시 해결책이나 원인을 분석하는 AI 기능은 현재 개발중에 있으며 이번달에 열리는 Dash 행사 때 추가적인 정보가 공개될 예정입니다. 많은 기대 부탁드립니다 :)

정은 이      오후 2:30
소스맵 업로드를 완료해서 어느정도 기간동안엔 에러 시에 코드가 보였는데 시간이 지나면 코드가 안보이는 경우가 있던데 소스맵 업로드를 주기적으로 해줘야하나요?

Datadog_Taeyeong Im      오후 2:33
소스맵의 경우 Version tag, Service tag, file path가 일치하는 경우 코드가 보여집니다. 새로운 버전이 배포되면서 소스맵의 버전과 일치하지 않아서 보이지 않을 수 있습니다. 해당 문제는 데이터독 콘솔 좌측 하단 support ticket으로 문의해주시면 빠르게 도움드리도록 하겠습니다

준성 김      오후 2:32
product analytics에서 수집되는 session 값들은 독립 유저라고 볼 수 있나요? 아니면 단순 수집되는 세션 기준일까요?

만약 독립 유저라면 어떤 값 기준으로 판단하는지 궁금합니다.

Datadog_Wudu      오후 2:35
Product Analytics에서 Overview 페이지에서 보이는 Session은 수집되는 세션 수 기준으로 이해해주시면 되겠습니다. 이외의 유저별 세션으로 구분하기 위해서는 Usr.id 정보를 삽입해주시면 되겠습니다.

참고하실 수 있는 링크를 전달드립니다.
https://docs.datadoghq.com/real_user_monitoring/browser/advanced_configuration/?tab=npm#user-session

지수 이      오후 2:32
product analytics 기능은 6월 1일에 RUM에서 떨어져나온다고 들었는데, 이 때 기존에 rum에서 제공하던 product analytics preview 기능은 바로 사라지게 될까요?

Datadog_Jade Cho      오후 2:35
안녕하세요. 기능이 바로 사라지지는 않게 되겠고, 구매하신 Datadog 계약 형태에 따라 사용하시는 기능에 대해 부과되는 비용기준이 달라지겠습니다.
과금에 대한 보다 정확한 안내를 드릴 수 있기 위해, 담당 CSM이나 영업대표, 혹은 Datadog Support에 문의를 주시면 감사하겠습니다

혜림 천      오후 2:33
1. 데모 때 보여주신 오류 분석 창에서 custom attributes 도 수집 가능하다고 하셨는데 어떤 값이 수집되는 걸까요?

2. 사용자가 접속한 브라우저의 session, local Storage 정보도 자동으로 수집할 수 있을까요?

Datadog_Kihyun Lee      오후 2:37
1. Custom Attribute 의 경우 사용자 정보나 현재 페이지 관련 정보 등 원하시는 값을 넣어주실 수 있습니다.
2. local storage나 세션 정보는 자동으로는 수집되지 않으며 , 코드 단에서 따로 해당 값에 접근하여 추가는 가능하십니다

요한 곽      오후 2:34
Observability 플랫폼의 큰 특징중 다양한 데이터를 수집하여 상관관계 분석을 함으로 과거 직접 하나씩 데이터를 찾아가는것 대비 인사이트를 도출하는데 도움이 됩니다.
많은 데이터를 수집하는것이 중요하지만 비용과의 trade-off가 있을텐데요, 이 문제를 해결하는 방법중 sampling이 있을겁니다. 지금 시연중인 BI 기능의 경우 sampling되지 못한(저장되지 못한) 데이터는 어떻게 분석이 가능할까요?

Datadog_Taeyeong Im      오후 2:42
Sampling되지 못한 세션은 Datadog RUM BI에서 분석에 포함되지 않지만, 상대적 비교나 추이를 분석하는데 활용 하시거나 결제 정보와 같은 데이터는 로그로 따로 남기시는 방법이 있습니다. 혹은 이따가 소개드릴 Rum without limit 의 기능을 활용하여, 특정 attribute가 있다면, 해당 세션은 sampling하지 않고 수집하게하거나 코드 상의 로직으로 세션 sample rate을 다르게 하는 방법이 있을 수 있을 것 같습니다. 자세한 내용은 데이터독 도입을 검토하실 때 담당 영업 대표와 이야기해보시면 좋을 것 같습니다.

익명 참석자      오후 2:34
히트맵이 Heatmaps are not available for this application
라는 에러가 나오는데 필요한 설정이 있나요?

Datadog_Jade Cho      오후 2:36
혹시 Session Replay가 수집되고 있는지 확인 부탁드리며, 수집되는 경우에 에러가 발생하는 경우 Datadog Support에 문의해주시면 보다 정확하게 기술적 안내를 드릴 수 있겠습니다.

태인 김      오후 2:35
Product Analytics 보여주신 화면과 제가 지금 보고 있는 화면이 다른데 이것도 곧 새롭게 제공될 예정인걸까요?

Datadog_Jade Cho      오후 2:37
네, Product Analytics 기능은 현재 Preview로 베타 제공 중이라 고객 환경과 소폭 차이가 있을 수 있는데요. 이번 6월에 있을 Datadog의 DASH 컨퍼런스에서 정식 출시가 될 예정이기 때문에 많은 관심 부탁드립니다!

정은 이      오후 2:36
에러 트래킹에서 나오는 에러와 세션 익스플로러에서 나오는 에러가 달라보이는데 에러 트래킹에서는 모든 에러가 보이는게 아니라 에러가 특정되는 걸까요?

Datadog_Wudu      오후 2:40
Error Tracking에서 표시되는 에러의 경우, error.type/ error.stack/error.message 정보가 필요하며 이미 핸들링 처리된 에러에 대해서는 Error Tracking 페이지에 보이지 않으실 수 있습니다.

모든 에러에 대해 표시되도록 설정이 필요하실 경우, 참고하실 수 있는 링크를 전달드립니다.

https://docs.datadoghq.com/error_tracking/frontend/collecting_browser_errors/?tab=npm#collect-errors-manually

정현 김      오후 2:36
혹시 addAction이나 addError에 값을 넘길 때
민감정보 등을 자동으로 마스킹한다던지, 하는 옵션이 있을까요?

Datadog_DeukHee Kim      오후 2:39
안녕하세요, Datadog에서는 RUM에서 수집되는 데이터에 대해 마스킹을 하거나, Datadog에 저장되기 전에 민감데이터를 처리하여 저장할 수 있는 Sensitive Data Scanner 기능을 제공하고 있습니다.

아래 URL을 참고 부탁드립니다.

[RUM Privacy Options]
https://docs.datadoghq.com/real_user_monitoring/session_replay/browser/privacy_options/

[Sensitive Data Scanner]
https://docs.datadoghq.com/security/sensitive_data_scanner/

익명 참석자      오후 2:37
이탈률의 기준은 뭔가요? 탈퇴인지? 로그아웃인지? 브라우저 종료한건지?

Datadog_Wudu      오후 2:42
이탈률의 경우, 고객마다 기준이 달라질 수 있지만 데이터독 샘플에서 보여드렸던 샘플에 대해서는 10초 이내에 브라우저를 종료한 경우라고 이해해주시면 되겠습니다!

익명 참석자      오후 2:38
정확히는 에러는 아니고 특정 히트맵 상세를 확인할 경우 아래와 같은 문구가 뜹니다.

Heatmaps are not available for this application
Heatmaps are currently only supported for browser applications.

Session Replay 수집은 50%로 설정 되어있는 상태입니다.

Datadog_Jade Cho      오후 2:42
네 확인 감사드립니다. 문구로 보았을 때는 모바일 앱에 대한 내용으로 보입니다. 현재 Heatmap 분석은 모바일 앱에 대해서는 공식적으로 제공되지 않고 있으며 개발중에 있습니다. 상세한 Preview 정보와 로드맵 등은 Support에 문의해주시면 제품 팀에서 정해진 내용이 있다면 알려드릴 수 있을 것 같습니다. :)

영재 김      오후 2:41
해당 대시보드 공유 받을 수 있을까요?

Datadog_Wudu      오후 2:45
대시보드에 대해서는 담당하고 있는 영업대표분께 문의주시면 Import 하실 수 있는 파일에 대해 전달드릴 수 있도록 하겠습니다.

익명 참석자      오후 2:42
이벤트명은 다 커스텀인가요?

Datadog_Taeyeong Im      오후 2:44
Datadog RUM(Product Analytics)에서 이벤트 이름(event name)은 기본값도 있고, 커스텀도 가능합니다.

익명 참석자      오후 2:43
세션 리플레이 기능을 활성화 해야 애널리틱스를 볼 수 있는 건가요?

Datadog_DeukHee Kim      오후 2:46
Product Analytics는 RUM이 활성화되어야 사용할 수 있는 기능입니다.
그리고 Product Analytics 기능 중에서, Session Replay가 활성화되어 있어야 확인 가능한 기능들도 있습니다.
(예를 들어 HeatMap)

익명 참석자      오후 2:44
iOS/Android 앱에서도 제약 없이 애널리틱스 보여주신것과 동일하게 사용 가능한지 궁금해요~

Datadog_Wudu      오후 2:48
모바일 애플리케이션의 경우, Heatmap 기능을 제외한 기능들에 대해서는 동일하게 지원을 드리고 있습니다. 

모바일 애플리케이션에 대한 Heatmap 기능은 개발 로드맵에 있으나 공식적인 일정은 나와있지 않아 참고해주시면 감사하겠습니다.

요한 곽      오후 2:48
앞서 질문에서 Rum without limit를 이용하면 수집된 데이터의 특정 attribute를 이용해 sampling 대상을 결정할 수 있다고 알려주셨습니다. 이어서 두가지 질문이 있습니다.

1. 이러한 sampling 방식은 rum 뿐만 아니라 apm 데이터의 sampling에서도 유효한 방식인가요?
2. latency나 page load 시간과 같은 정보로도 sampling이 가능한가요?

Datadog_Kihyun Lee      오후 2:52
1. RUM without limit 의 경우 RUM 의 이벤트들에만 적용됩니다.
2. 레이턴시 페이지로드 시간 정보까지 샘플링 가능한지는 아직 정보가 없습니다만 데이터독 Support 쪽으로 문의주시면 추가 정보 드릴 수 있도록 하겠습니다.

상권 서      오후 2:49
에러가 발생되었을때만 세션리플레이를 수집해서 확인할 수 있나요??

Datadog_Wudu      오후 2:51
안녕하세요.

RUM Without Limits 기능을 통해 에러가 발생한 케이스만 Session 및 Session Replay를 저장하여 확인하실 수 있습니다.

상권 서      오후 2:51
세션 저장을 셈플링이라던지, 아니면 특정 경로로 잡는건 알려주셨는데 에러발생되었을대로 조건을 잡을수 있는건지 궁금해서요.

Datadog_Wudu      오후 2:52
안녕하세요.

RUM Without Limits 기능을 통해 에러가 발생한 케이스만 Session 및 Session Replay를 저장하여 확인하실 수 있습니다.

예를 들어, 필터링 조건에 @session.error.count:>0과 같이 설정하여 수집이 가능합니다.


iframe 구조는 트래킹 어렵다