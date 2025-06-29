# 1. 모바일 IP
***
### Moblie IP
- 휴대용 장치와 네트워크 간의 연결은 일반적으로 무선
- IP 주소가 인터넷 연결 지점을 지정하고 런타임에 IP 주소를 변경하면 모든 연결과 세션이 종료된다는 문제
- 장치가 IP 주소와 통신을 유지하면서 연결 지점을 변경할 수 있는 간단한 모바일 IP 솔루션을 살펴볼 것
### Routing in Mobile IP
- 통신 세션을 유지하면서 모바일 호스트라는 휴대용 장치를 한 영역에서 다른 영역으로 이동할 수 있게 해주는 모바일 IP 라우팅
- 모바일 호스트가 다른 영역으로 로밍할 때 영구 IP 주소를 계속 사용해야 한다
- 그렇지 않으면 기존 세션이 작동을 멈추고 모바일 호스트가 다른 지역으로 이동할 때 새 세션을 시작해야 함
- 호스트 에이전트는 네트워크 내 각 모바일 호스트의 위치를 추적함
- 홈 에이전트는 주기적으로 존재를 알림
- 동일한 주소 접두사를 사용하는 홈 네트워크의 모든 모바일 호스트를 관리함
- 모바일 호스트가 홈 네트워크에 있는 경우 홈 에이전트는 패킷을 모바일 호스트로 직접 전달함
- 모바일 호스트가 외부 네트워크로 이동하면 모바일 호스트는 외부 에이전트로부터 관리 주소를 얻어 홈 에이전트에 새 주소로 등록함
- 따라서 모바일 IP 라우팅은 다음과 같이 정렬됨 (ppt 참고)
  - Correspondent Host(CH) sends packets as usual (1)
  - Packets are intercepted by HA(home agent) which then forwards to Foreign Agent (FA) (2)
  - FA forwards packets to the MH
  - MH sends packet to CH as usual (3)
  - How does HA send packets to MH in foreign network?
### IP-to-IP Encapsulation
- 각 IP 패킷을 홈 에이전트가 소스 IP 주소로, 관리 주소를 대상 IP 주소로 포함하는 외부 IP 주소로 캡슐화하여 구현함
- 외부 에이전트가 패킷을 수신하면 외부 에이전트는 원본 호스트 주소를 원본 IP 주소로 하고 대상 주소의 모바일 호스트 주소를 사용하여 원래 IP 패킷이 생성된 패킷의 캡슐화를 해제함
- 외부 에이전트가 패킷을 모바일 호스트에 전달할 수 있음
- 모바일 호스트가 해당 호스트로 전송하는 패킷은 일반적으로 모바일 호스트 주소를 원본 IP 주소로 하고 해당 호스트 주소를 대상 IP 주소로 하는 일반 IP 패킷 형식을 사용함
- 이러한 패킷은 기본 경로를 따름
### Route Optimization
- 패킷이 해당 호스트에서 모바일 호스트로 이동하는 시간이 일반적으로 모바일 호스트에서 해당 호스트로 이동하는 시간보다 김
- 라우팅 메커니즘을 개선하기 위한 몇 가지 방안
- 해당 호스트는 후속 교환에서 Care-of address endpoint로 직접 패킷을 보낼 수 있음
- 홈 에이전트가 해당 호스트로부터 패킷을 수신하면 1 단계에서 보이는대로 모바일 호스트로 전송함
- 2A 단계에서 이전과 같이 패킷을 현재 관리 주소로 터널링함 
- 2B 단계처럼 현재 관리 주소가 포함된 바인딩 메시지를 해당 호스트에 다시 전송하기도 함
- 해당 호스트는 이 정보를 저장할 수 있음
- 4 단계에서 향후 모바일 호스트로 전달되는 패킷을 관리 주소로 직접 터널링할 수 있음
***
# 2. 멀티캐스트 라우팅
***
### Multicasting
- 이전에는 특정 소스가 단일 대상으로 패킷을 전송한다고 가정하는 라우팅 메커니즘 설명
- 원격 회의와 같은 상황에서는 소스가 여러 대상으로 동시에 패킷을 보내려고 할 수 있음
- 이에 멀티캐스트 라우팅이 필요함
- 소스 S는 멀티캐스트 그룹 G1으로 패킷을 전송하려고 함
- 일반적인 유니캐스트 라우팅을 사용하여 패킷의 각 사본을 대상에게 개별적으로 보낼 수 있지만, 복사본 수를 최소화하는 것이 더 효율적임
### Reverse-Path Broadcasting (RPB)
- 노드에 대한 최단 경로 집합이 네트워크를 가로지르는 최단 경로 트리를 형성한다
- 각 라우터가 지정된 목적지까지의 현재 최단 경로를 이미 알고 있다고 가정하면 역방향 경로 브로드캐스팅은 매우 간단함
- 멀티캐스트 패킷을 수신하면 라우터는 패킷의 소스 주소와 패킷이 도착하는 포트를 추가로 기록함
- 라우터는 하나의 패킷이 도착할 때를 제외하고 다른 모든 포트에 패킷을 전달함
- 그렇지 않으면 해당 라우터는 패킷을 삭제함
- 이 접근 방식의 장점은 각 패킷이 라우터에 정확히 한 번 전달된다는 것
- 알고리즘의 기본 가정은 소스로부터 지정된 라우터까지의 최단 경로가 라우터에서 소스로 돌아가는 최단 경로와 같아야 한다는 것
### Truncated RPB (TRPB)
- 연결된 호스트가 멀티캐스트 그룹에 속하지 않는 경우, 라우터가 로컬 네트워크로의 전송을 truncate(잘라내도록) 하면 문제 해결
### Internet Group Management Protocol
- 인터넷 그룹 관리 프로토콜인 IGMP를 사용하면 호스트가 멀티캐스트 그룹 구성원 자격을 연결된 라우터에 알릴 수 있음
- 멀티캐스트 라우터는 정기적으로 IGMP 쿼리 메시지를 발행하여 멀티캐스트 그룹에 속하는 호스트가 있는지 확인함
- 라우터는 특정 포트와 연결된 멀티캐스트 그룹을 결정함
- 라우터는 멀티캐스트 그룹에 속하는 호스트가 있는 포트에서만 패킷을 전달함
### Reverse-Path Multicasting
- 역방향 경로 멀티캐스팅은 잘린 역방향 경로 브로드캐스팅을 개선한 것
- 그러나 TRPB와 달리 RMP는 멀티캐스트 패킷을 라우텅에만 전달하여 그룹 구성원이 있는 리프 라우터로 연결함
- 이를 위해 IGMP를 사용하여 멀티캐스트 그룹 구성원을 식별함
***
# 3. OpenFlow, SDN, NFV
***
### OpenFlow
- 프로덕션 네트워크에서 실험적이고 혁신적인 프로토콜을 실행할 수 있게 해주는 개방형 표준
- 상용 이더넷 스위치, 라우터 및 무선 액세스 포인트에 기능으로 추가됨
- 벤더가 네트워크 장치의 내부 작동 방식을 공개할 필요 없이 실험을 실행할 수 있는 도구를 제공하고 표준화함
- 소프트웨어 정의 네트워크(Software-Defined-Network, SDN) 아키텍쳐의 제어 계층과 포워딩 계층 사이에 정의된 표준 통신 인터페이스
- 2012년 말에 Network Function Visualization, NFV 네트워크 기능 가상화라는 개념 정의
- SDN과 NFV가 서로를 보완하는 개념
### OpenFlow Architecture
- 기존 라우터나 스위치에서는 데이터 경로와 제어 경로가 동일한 디바이스에서 발생함
- Openflow 스위치는 이 두 기능을 분리함
- 데이터 경로 부분은 여전히 스위치에 있으며, 상위 수준의 라우팅 결정은 일반적으로 표준 서버인 별도의 컨트롤러로 이동함
- Openflow 스위치와 컨트롤러는 메시지를 정의하는 Openflow 프로토콜을 통해 통신함
- Openflow 프로토콜은 라우터와 스위치로 구성된 인프라 계층과 제어 계층 간의 통신 경로로 사용됨
- 제어 계층은 중앙 집중식 인텔리전스를 처리하여 프로비져닝을 간소화하고 성능을 최적화하며 정책 관리를 세분화함
### General Misconceptions
- 몇 가지 일반적인 오해들
- Openflow는 SDN이 아니며 그 반대도 마찬가지
- SDN이 표준 southbound API 인가? 컨트롤 플레인의 중앙 집중화?, 제어 및 데이터 플레인의 분리? 등
- 모든 오해는 소프트웨어 정의 네트워크가 메커니즘인지 아닌지에 대한 의문
- SDN은 메커니즘이 아니라 다양한 솔루션을 제시하는 일련의 문제를 해결하기 위한 프레임워크
- Openflow는 데이터 플레인 스위치 프로그래밍을 위한 표준 인터페이스를 제공하는 개방형 API
### What is SDN?
- SDN은 프로덕션 네트워크를 보다 민첩하고 유연하게 만들기 위해 설계된 새로운 기술임
- 프로덕션 네트워크는 대개 매우 정적이고 변경 속도가 느리며 단일 서비스에만 집중됨
- 소프트웨어 정의 네트워킹을 사용하면 다양한 서비스를 동적으로 처리하는 네트워크를 만들어 여러 서비스를 하나의 공통 인프라로 통합할 수 있음
- 중앙 집중식 네트워크 제어를 통해 이루어지므로 제어 로직을 분리하여 클라우드, 관리 및 비즈니스 애플리케이션을 포함하는 개방형 프로그램이 인터페이스를 통해 네트워크 서비스를 자동화하고 조정할 수 있음
### SDN Benefits
- Efficiency
  - 기존 애플리케이션, 서비스, 인프라 최적화
- Scalability
  - 기존 애플리케이션, 서비스의 빠른 성장
- Innovation
  - 개발자가 새로운 유형의 애플리케이션, 서비스, 새로운 비즈니스 모델을 만들고 제공할 수 있음
### Why do we need SDN?
- 시각화 : 네트워크 리소스가 물리적으로 어디에 있는지 알 필요가 없음
- Orchestration : 명령 하나로 수천대의 장치를 제어하고 관리
- Programmable : 심각한 업그레이드 없이 네트워크 동작을 쉽게 변경할 수 있음
- Dynamic scaling : 규모와 수량을 변경가능
- Automation : 문제 해결 및 리소스 공급과 같은 수동 작업을 최소화
- Visibility : 네트워크 연결에 대한 리소스 모니터링 가능
- Performance : 트래픽 엔지니어링, 용량 최적화, 로드 밸런싱 및 높은 사용률
- Multi-tenancy : 관리자가 주소, 토폴로지, 라우팅, 보안 및 밸런서 부하, 침입 탐지 시스템, 방화벽의 서비스 통합 액세스
### Network Function Virtualization (NFV)
- 네트워크 기능마다 다른 하드웨어를 배포하는 대신 가상 서버 내에 여러 네트워크 기능을 통합하는 것
- 방화벽이나 암호화와 같은 기능을 전용 하드웨어에서 분리하고 기능을 가상 서버로 이동함
- NFV라는 용어는 별도의 하드웨어에서 표준 하드웨어를 사용하여 가상 서버에서 실행되는 소프트웨어로 전환하는 네트워크 기능을 가상화하는 전략을 의미함
### NFV Innovations
- NFV에는 모듈 간 표준 API를 포함하는 4가지 주요 혁신이 있음
- 네트워크 기능은 가상 머신에 구현됨
- 손쉬운 프로그래밍 기능과 네트워크의 소프트웨어 구현을 향상시키는 네트워크 기능 모듈
### NFV and SDN Relationship
- NFV의 컨셉은 SDN으로부터 옴
- NFV와 SDN은 상호보완적임
- 둘다 비슷한 목적을 가지지만 접근방식이 다름
- 가상화는 자체로 NFV와 SDN 둘 모두에게 필요한 기능을 제공함
***
# 4. Network Security Threats
***
### Network Security
- 저렴한 비용의 강력한 컴퓨팅과 고성능 네트워크의 조합은 양날의 검과 같지만 새로운 서비스와 애플리케이션을 구현 가능
- 컴퓨터 시스템과 네트워크는 다양한 보안 위협에 매우 취약함
- 특히 인터넷 및 TCP/IP 프로토콜은 개방성과 침입성을 염두에 두고 설계되었지만 보안 문제도 드러남
- 네트워크 보안에는 컴퓨터 시스템을 침입자로부터 보호하기 위한 대응책이 포함됨
- 일반적인 조치에는 방화벽, 보안 프로토콜, 보안 관행 등이 포함
### Eavesdropping
- 인터넷과 같은 공용 패킷 스위칭 네트워크는 전송되는 정보에 대해 높은 수준의 보안을 제공한다는 점에서 전통적으로 안전
- 이러한 네트워크는 상업 거래에 많이 사용되고 있음, 보안 제공이 중요
- Eavesdropping (도청) : 네트워크를 통해 전송되는 정보는 안전하지 않음
- 도청자가 이를 관찰하고 기록할 수 있음
- 예를 들어, 패킷 스니퍼를 사용하면 서버 액세스를 시도할 때 정보를 재생할 수 있음
### Client Imposter
- 클라이언트 사기꾼 : 서버에 무단으로 액세스하려고 시도함
- 은행 계좌 또는 개인 기록 데이터베이스에 액세스하는 것도 마찬가지
- 예를 들어 IP 스푸핑에서는 사기꾼이 잘못된 소스 IP 주소로 패킷을 전송함
### Server Imposter
- 서버 사기꾼은 합법적인 서버를 가장하여 클라이언트로부터 민감한 정보를 획득함
### Denial of Service (DoS) Attack
- 서비스 거부 공격
- 공격으로 인해 서버 리소스에 과부하가 걸리는 요청이 서버에 폭주하여 합법적인 클라이언트에 대한 서비스 거부가 발생함
- 일반적으로 하이재킹되는 여러 대의 컴퓨터로부터의 공동 공격이 포함됨
### TCP SYN Flood
- 일반적인 TCP 3방향 핸드쉐이크의 일부를 악용하여 대상 서버의 리소스를 소비하고 응답하지 못하게 만드는 일종의 분산 서비스 거부 방식
- 공격자는 컴퓨터의 대상이 처리할 수 있는 속도보다 빠르게 TCP 연결 요청을 전송하여 네트워크 포화 상태를 초래함
- SYN Flood 공격에서 공격자는 가짜 IP 주소를 사용하여 대상 서버의 모든 포트에 동일한 패킷을 반복해서 보냄
- 공격을 인지하지 못한 서버는 열린 각 포트에서 동일한 ACK 패킷을 사용하여 각 시도에 대한 응답으로 통신을 설정하라는 보기에는 합법적인 요청을 여러 번 받음
- 공격자는 예상한 ACK를 보내지 않거나 IP 주소가 스푸핑된 경우 애초에 SYN ACK를 받지 않음
- 공격을 받는 서버는 SYN to ACK 패킷이 확인될 때까지 잠시 기다림
- 이 기간 동안에는 서버가 RST 패킷을 전송하여 연결을 크로스바운드할 수 없으며 연결은 열린 상태로 유지됨
- 연결 제한 시간이 초과되기 전에 다른 SYN 패킷이 도착함
- 절반만 열린 연결이 점점 많아짐
- 하프 오픈 공격이라고도 함
- 결국 서버 연결 오버플로 테이블이 꽉 차게 됨
- 정상적인 클라이언트에 대한 서버 연결은 거부되며 작동하지 않거나 충돌할 수 있음
- 네트워크 부품을 고갈시키려 하는 것이 목적
- SYN 패킷은 네트워크 포화 상태를 달성하기 위해 가짜 패킷으로 파이프를 막으려는 DDOS 공격에도 사용될 수 있음
- 패킷 유형은 중요하지 않음
- SYN 패킷은 기본적으로 거부될 가능성이 가장 낮기 때문에 자주 사용됨
### Main-in-the-Middle Attack
- 사기꾼은 자신을 중간에 있는 사람으로 여기며 서버는 합법적인 클라이언트라고, 합법적인 클라이언트는 합법적인 서버라고 설득
### Malicious Code
- 클라이언트가 악성 코드에 감염
- 이메일 메시지의 첨부 파일을 열거나 게시판이나 기타 소스에서 코드를 실행하는 경우를 예
- 바이러스는 실행 시 다른 프로그램에 삽입되는 코드
- 웜 은 네트워크에 연결된 다른 컴퓨터에 자신의 복사본을 설치하는 코드
- 다양한 변형이 있음
### Security Requirements
- Privacy : 정보는 의도한 수신자만 읽을 수 있어야 함
- Integrity : 무결성, 정보 수신자는 메시지가 전송 중에 변경되지 않았는지 확인해야 함
- Authentication : 인증, 발신자 또는 수신자가 자신이 주장하는 사람인지 확인가능
- Non-repudiation : 거부되지 않은 발신자, 해당 메시지를 전송했다는 사실과 정보 및 서비스의 가용성을 거부할 수 없음
- Avaliability
### Countermeasures
- 보안 통신 채널에 대한 대응책으로는 암호화, 암호화 체크섬, 해시 인증, 디지털 서명 등이 있음
- Secure border에는 방화벽, 바이러스 검사, 침입 탐지, 인증 및 액세스 제어가 포함됨