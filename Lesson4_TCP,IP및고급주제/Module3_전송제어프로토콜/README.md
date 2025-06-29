# 1. UDP 및 TCP
***
### UDP
- 신뢰할 수 없고 연결이 필요 없음
- IP 외에 디멀티플렉싱과 데이터 오류 검사라는 두가지 추가 서비스만 제공하는 매우 간단한 프로토콜
- IP는 패키지를 호스트에 전달하는 방법을 알고 있음
- UDP는 여러 애플리케이션을 구분하고 호스트를 보유하는 메커니즘을 추가함
- 무연결 : 핸드쉐이킹이 없고 명시된 정보가 없다
- 프로토콜의 오버헤드가 적음
- UDP 사용시 데이터그램이 손실되거나 통제 불능상태가 될 수 있음
- 흐름제어, 오류제어, 혼잡제어 없음
### UDP Datagram
- 대상 포트를 통해 UDP 모듈은 데이터그램을 호스트의 올바른 애플리케이션을 디멀티플렉싱할 수 있음
- 소스 포트는 소스 호스트에서 응답을 받을 특정 애플리케이션을 식별함
- 포트 넘버링에는 일련의 규칙이 있음
- ex) 0~255 : 잘 알려진 포트
### UDP De-Multiplexing
- 디멀티플렉싱 프로세스
- 모든 UDP 데이터그램이 IP 주소 B에 도착하고 대상 포트 번호 N이 동일한 프로세스로 전달됨
- 소스 포트 번호는 디멀티플렉싱에 사용되지 않음
### UDP Checksum Calculation
- UDP 체크섬 필드는 데이터그램에서 종단 간 오류를 탐지함
- pseudo 헤더와 그 뒤에 UDP 데이터그램이 뒤따름
- pseudo 헤더에는 잘못된 전송을 감지하기 위한 IP 주소가 포함되어 있음
- UDP 수신기는 체크섬을 다시 계산하고 오류가 감지되면 데이터그램을 자동으로 삭제함
### TCP
- 전송 제어 프로토콜 TCP는 연결지향적이고 안정적인 시퀀스 바이트 스트림 서비스를 제공함
- 오류, 흐름, 혼잡 제어 메커니즘 가능
- 오버헤드가 높지만 안정성 때문에 대부분의 애플리케이션은 TCP를 사용함
### TCP Multiplexing
- 발신자 IP 주소 및 전화 번호, 대상 IP 주소 및 전화 번호라는 4가지 요소로 고유하게 식별됨
- 모든 시스템이 동시에 여러개의 TCP 연결을 지원할 수 있음
### Reliable Byte-Stream Service
- TCP는 메시지 경계를 가정하지 않으며 애플리케이션 계층에서 수신한 데이터를 바이트 스트림으로 취급함
- 바이트를 세그먼트로 그룹화하고 효율성을 높이기 위해 편리한 세그먼트를 전송함
- 예를 들어 애플리케이션은 TCP 회로에 45바이트, 15바이트, 20바이트를 기록 가정
- 트랜스포터 계층은 기본 네트워크를 통한 전송에 가장 적합한 방식으로 정보를 분할하고 결합하여 세그먼트로 패킹할 수 있음
- 각각 40바이트씩 2개의 세그먼트를 연결에 전송할 수 있음
- TCP에서는 오류 감지 및 재전송이 적용됨
### Flow Control
- 역방향으로 이동하는 세그먼트에는 현재 수신기 버퍼에 수용할 수 있는 바이트 수를 송신기에 알려주는 advertised window size가 포함되어 있음
### TCP Segment Format
- 헤더는 고정된 부분과 가변측 옵션 필드로 구성됨
- 패딩을 사용할 경우 헤더 길이는 32비트 단어의 배수여야 함
- 최소 헤더 길이는 20이고 최대 헤더 길이는 60바이트임
- 헤더 길이는 TCP 헤더의 길이를 32비트 워드로 지정함
- 옵션 필드는 가변 길이이므로 수신자는 이 정보를 통해 데이터 영역의 시작 부분을 알 수 있음
### TCP Header
- Window Size
  - 16비트 사이트
  - 송신자가 수락할 수 있는 바이트 수를 지정함
  - 데이터 흐름과 혼잡을 제어하는 데 사용할 수 있음
  - 발신자는 ACK에서 ACK+window 까지 시퀀스 번호가 있는 바이트를 수락함
- TCP Checksum
  - IP 체크섬을 계산하는 데 사용되는 절차와 비슷함
  - TCP pseudo 헤더 및 데이터 세그먼트의 체크섬을 계산함
  - 이렇게 하면 수신기가 세그먼트가 올바른 대상 호스트 및 포트에 도달했으며 프로토타입이 올바른지 확인할 수 있음
  - 체크섬 상태에서 소스 호스트와 대상 호스트가 만든 pseudo 헤더는 실제로 전송되지 않는다는 점에 유의
***
# 2. TCP Three-way Handshake
***
### TCP Connection Management
- TCP는 최선형 IP를 기반으로 구축되었으므로 이전 연결의 오래된 세그먼트가 수신자에게 도착할 수 있음
- 중복 할당을 제거하는 작업이 복잡함
- TCP는 32비트 길이의 시퀀스 번호를 사용하고 컬렉션 설정 중에 무작위로 선택된 초기 시퀀스 번호를 설정하여 이 문제를 해결함
- 수신기는 언제든지 훨씬 작은 윈도우에서 시퀀스 번호를 수락함
- 아주 오래된 메시지를 받아들일 가능성은 매우 낮음
- 또한 TCP는 네트워크가 오래된 세그먼트를 지울 수 있도록 각 연결 종료 시 최대 세그먼트 수명 Maximum segment lifetime (MSL)을 적용함
- MSL은 보통 2분이지만 왕복 지연에 따라 달라짐
### TCP Header - Seq and Ack
- Sequence Number
  - TCP 시퀀스 번호는 송신자의 바이트 스트림에서 이 세그먼트의 첫번재 바이트 위치를 식별하며, 32바이트의 데이터를 전송한 후 시퀀스 번호는 다시 0으로 되돌아감
  - 참고로 TCP는 각 바이트의 시퀀스 번호를 식별함
  - 예를 들어 시퀀스 번호가 100 이고 데이터 영역에 5바이트가 포함된 경우 이 TCP가 다음에 세그먼트를 전송할 때 시퀀스 번호는 105가 됨
  - 플래그 비트 SYN 이 1로 설정되면 연결 설정 중에 초기 시퀀스 번호인 Initial sequence number (ISN) 이 선택됨
  - 이 스트림 바이트에 대한 첫번째 바이트 데이터의 시퀀스 번호는 ISN에 1을 더한 값이 됨
  - 중요한 점은 TCP 연결은 각 지점마다 고유한 시퀀스 번호가 독립적으로 유지된다는 점
- Acknowledgement Number
  - 수신 확인 번호는 발신자가 수신할 것으로 예상하는 다음 데이터 바이트의 시퀀스 번호를 식별함
  - 발신자가 이전의 모든 데이터를 성공적으로 수신했음을 나타냄
  - ACK flag 비트가 설정된 경우에만 유효함
### TCP Header - Control bits
- 6개의 제어 비트가 있음
- ACK : 승인
- RST : 수신 TCP 모듈에 연결을 중단하라고 지시함
- SYN : 새 연결을 요청
- FIN : 발신자가 더 이상 전송할 데이터가 없음을 수신자에게 알리는 것
- 송신자는 여전히 다른 방향에서 데이터를 수신할 수 있으며 여기서는 FIN 비트 세트에서 1초를 수신함
### TCP Connection Establishment
- 호스트가 데이터를 보내려면 먼저 연결을 설정해야 함
- TCP는 3방향 핸드쉐이크 프로토콜을 사용하여 연결을 설정함
1. 호스트 A는 초기 시퀀스 번호 x를 임의로 생성하고 A는 동기화 비트를 설정하여 호스트 B에게 연결 요청을 보냄, 호스트 A는 사용할 초기 시퀀스 번호를 등록함
2. 호스트 B는 해당 ACK 비트를 설정하고 수신할 예상 바이트 x에 1을 더한 ACK 번호를 지정하여 요청을 승인함. 승인은 시퀀스 번호 하나를 사용하기 때문에 경로가 필요함. 동시에 호스트 B는 해당 측에서 초기 시퀀스 번호 Y를 임의로 생성하여 SYN 비트로 설정하고 사용할 초기 시퀀스 번호를 지정함
3. 호스트 A는 ACK 비트를 설정하여 B의 요청을 승인하고 수신할 다음 데이터 바이트를 계산함. ACK 번호는 Y+1 값임. 호스트 B에서 수신하면 연결이 설정됨
- 연결 설정 인터페이스 중에 호스트 중 한 명이 연결 요청을 거부하기로 결정하면 재설정 세그먼트를 보냄
- TCP 세그먼트가 지연, 손실, 복제될 수 있으므로 IST 비트를 설정하면 됨
### if host always uses the same ISN
- 호스트가 연결을 요청할 때마다 초기 시퀀스 번호가 달라야 함
- 설정된 연결 중 이전 연결의 가장 최근 세그먼트가 도착함
- 유효한 시퀀스 번호이므로 호스트 B가 이를 수신함
- 현재 연결의 세그먼트가 나중에 도착하면 호스트 B는 중복된 것으로 간주하여 거부함
- 즉, 호스트 B는 지연된 세그먼트와 새 세그먼트를 구분할 수 없음
### TCP Connection closing
- 연결의 각 방향이 독립적으로 종료된든 원활한 종료를 제공함
- 해당 TCP 엔티티는 수신자로부터 승인을 받으면 데이터 전송을 완료함
- FIN 비트가 설정된 세그먼트를 보장함
- FIN 세그먼트를 수신하면 TCP 엔티티는 다른 엔티티가 데이터 전송을 종료했음을 애플리케이션에 알림
- 호스트 A의 TCP 엔티티는 FIN 세그먼트를 발행하여 전송을 종료함
- 호스트 B는 A로부터 FIN 세그먼트를 수신하는 역할을 하는 승인을 전송함
- B가 FIN 세그먼트를 수신한 후에도 B에서 A로의 흐름방향은 여전히 열려 있음
- 호스트 A의 TCP는 정상 종료를 위한 시간 대기 상태로 전환됨
***
# 3. TCP Flow Control and Data Transfer
***
### TCP Flow Control
- 신뢰할 수 있는 데이터 전송을 제공하기 위해 3방향 핸드쉐이크 절차를 통해 TCP 연결이 설정된 후 TCP는 선택적 반복 ARQ 프로토콜을 사용하며, Positive ACK 기능은 슬라이딩 윈도우로 구현됨
- 윈도우가 패킷 단위가 아닌 바이트 단위로 슬라이드 된다는 점이 차이점
- TCP는 윈도우 크기를 동적으로 advertising 하며 연결에 흐름 제어를 적용할 수 있음
### TCP Connection Management
- 초기 시퀀스 번호는 이전 연결의 세그먼트로부터 보호하는데 사용됨
- 네트워크에서 순환하다가 훨씬 늦게 도착할 수 있음
- TCP는 로컬 클럭을 사용하여 초기 시퀀스 번호를 선택함
- 전체 사이클을 거치는 데 걸리는 클럭에 걸리는 시간은 세그먼트의 최대 수명 (보통 120초) 보다 커야 함
- 고대역폭 링크는 문제를 야기함
### Sequence Number Wraparound
- 고속 전송 라인을 사용하여 시퀀스 번호 공간을 둘러보는 데 시간이 얼마나 걸리는가
- 32비트 시퀀스 번호는 2^32 바이트 데이터가 전송되면 자동으로 정렬됨
- 초당 1기가비트 전송라인에서는 시퀀스 번호가 최대 세그먼트 수명보다 짧은 34초만에 끝날 수 있음
- 새 세그먼트가 기존 세그먼트와 섞일 수 있다는 것
- 타임스탬프 옵션을 사용하면 전송 TCP는 전송하는 각 세그먼트의 헤더에 있는 4바이트 필드에 32비트 타임스탬프를 추가로 삽입할 수 있음
- 32비트 타임스탬프와 32비트 시퀀스 번호를 조합하면 64개의 시퀀스 번호를 얻어 랩어라운드 문제를 해결할 수 있음
***
# 4. TCP 혼잡 제어
***
### TCP Congestion Control
- TCP 프로토콜은 광고된 윈도우를 사용하여 수신기 버퍼가 오버플로우되지 않도록 함
- 인접 라우터의 버퍼는 여전히 오버플로 가능
- 모든 패킷 흐름의 총 도착 속도가 일정 기간 동안 라우터의 발신 대역폭을 초과하면 혼잡이 발생함
- 혼잡이 발생하면 라우터 멀티플렉서의 버퍼가 가득 차서 패킷이 손실될 수 있음
### Phases of Congestion Behavior
- 일반적으로 트래픽 로드에는 3단계
- Light traffic
  - 라우터에서의 총 도착속도가 발신 대역폭 길이보다 훨씬 작음
  - 수신 패킷은 지연 시간이 짧음
- Knee (Congestion onset)
  - 총 수신 속도가 계속 증가함에 따라 라우터에서의 총 수신 속도가 발신 대역폭 길이에 근접했다는 결과가 나옴
  - 패킷의 지연이 증가하고 전체 기울기가 포화되기 시작함
- Congestion collapse
  - 총 도착 속도가 다른 발신 대역폭 길이보다 길면 congestion collapse 현상 발생
  - 패킷은 더 큰 지연 및 손실을 경험
  - 유효 처리량이 크게 떨어짐
### Congestion Window
- TCP 발신자가 너무 많은 패킷을 전송하여 너무 공격적이면 네트워크에 혼잡이 발생할 수 있음
- TCP 발신자가 너무 보수적이면 네트워크 사용률이 낮아짐
- TCP 혼잡 제어의 목적은 네트워크를 포화상태로 유지하면서 과부하가 걸리지 않도록 각 발신자가 적절한 양의 데이터만 전송하도록 하는 것
- TCP 프로토콜은 혼잡을 유발하지 않고 TCP 발신자가 전송할 수 있는 최대 바이트 수를 지정하는 혼합 윈도우를 정의함
- TCP 헤더에서 이 유효 기간은 혼잡 기간 및 광고된 윈도우 중 최소값
- 문제는 발신자가 사용 가능한 대역폭에서 어느 정도의 비율을 차지해야 하는지 모른다는 것
- TCP 솔르션은 사용 가능한 네트워크 경계에 맞게 동적으로 조정됨
- 전송 속도가 최적 지점 근처에서 안정되는 것이 이상적
- TCP 혼잡 알고리즘이 어떻게 혼잡 윈도우를 동적으로 바꾸는가?
- 트래픽이 적은경우 각 세그먼트를 빠르게 인식해야 발신자가 혼잡 발생 범위를 빠르게 늘릴 수 있음
- Knee 포인트 부근에서는 구간 승인이 도착하지만 더 느림, 발신자는 혼잡 시간이 늘어나는 것을 늦춰야 함
- 혼잡이 감지되면 세그먼트에 더 큰 지연이 발생하여 재전송 시간 초과가 발생함
- 또한 세그먼트가 라우터 버퍼에서 삭제되어 승인이 중복됨
- 발신자는 전송 속도를 줄인 다음 다시 검색해야 함
### TCP Congestion Control 1: Slow Start
- 혼잡 제어의 작동은 3단계로 나눌 수 있음
- 혼잡 기간을 작은 값으로 설정하여 수행됨
- 보통 한 세그먼트로 시작하여 발신자가 제한 시간이 초과되기 전에 수신자로부터 확인을 받을 때마다 발신자는 혼잡 시간을 한 세그먼트씩 늘림
- 따라서 첫 번째 세그먼트를 보낸 뒤 발신자가 제한 시간이 초과되기 전에 확인을 받으면 혼잡 기간은 최대 두 세그먼트로 늘어남
- 나중에 이 두 세그먼트가 이를 승인하면 혼잡 기간은 4개로 늘어남
- 슬로우 스타트에서는 혼잡 윈도우 크기가 기하급수적으로 커짐
- 기하급수적으로 증가하는 이유는 네트워크 리소스를 최대한 활용하려면 파이프를 최대한 빨리 채워야 하기 때문
### TCP Congestion Control 2: Congestion Avoidance
- 알고리즘은 정체 임계값을 점진적으로 설정함
- 혼잡 윈도우 크기가 임계값보다 크면 슬로우 스타트 페이즈가 종료되고 혼잡 방지 기능이 그 자리를 대신함
- 왕복 시간당 한 구간씩 혼잡 시간이 선형적으로 늘어남
### TCP Congestion Control 3: Congestion
- TCP가 시간 초과 또는 중복 승인 수신으로 인한 네트워크 혼잡을 감지하면 혼잡 기간은 더 이상 늘어나지 않음
- 알고리즘이 정체 임계값을 현재 혼잡 기간의 절반으로 설정하여 다음 번에 링크 대역폭을 조사할 수 있도록함
- 정체 기간을 적극적으로 한 세그먼트로 다시 설정함
- 슬로우 스타트 기법을 적용하여 다시 시작함
- 초기 혼잡 임계밗이 16개의 세그먼트로 설정된 것을 ppt 그림으로 확인 가능
- 혼잡이 감지되었을 때 혼잡 기간의 절반으로 준다
### Fast Retransmit & Fast Recovery
- 혼잡이 심각하지 않은 경우 혼잡 윈도우 크기를 다시 한 세그먼트로 줄이는 것은 너무 공격적일 수 있음
- 실제로 전송 중에 세그먼트가 하나만 삭제되는 경우 TCP 발신자는 더 빠르게 복구할 수 있음
- 후속 세그먼트에서 TCP 수신자가 중복 승인을 전송하도록 트리거하기 때문임
- 이러한 트리거된 확인은 일반적으로 재전송 타이머가 만료되기 전에 TCP 센터에 도착함
- TCP 센터는 3개의 중복 승인을 수신하면 마지막 세그먼트를 다시 재전송하여 전송 속도가 더 빠름
- 그런 다음 센터에서는 앞에서 나온대로 혼잡 임계값을 설정하여 빠른 복구를 수행함
- 혼잡 윈도우 크기를 한 세그먼트로 줄이는 대신 혼잡 임계값에 3개의 세그먼트를 더한 값으로 설정함
- 알고리즘은 혼잡 방지 단계에 머물게 됨
- 타임아웃이 없는 경우 혼잡 윈도우 크기가 최적의 값으로 변동함