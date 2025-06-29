# 1. 클래스 없는 도메인 간 라우팅 (CIDR)
***
### IP Address Problems
- Class A, B, C 주소 구조는 비효율적임
  - B는 대부분의 조직에 너무 크고 C는 너무 작다 
- 인터넷의 네트워크 수가 증가함에 따라 IP 라우팅 테이블 크기도 빠르게 커짐
  - 라우팅 테이블이 커지면 라우터의 처리 능력과 메모리에 부담이 됨
- 단기적 솔루션
  - CIDR
  - 새로운 할당 정책 (RFC 2050)
  - 네트워크 주소 변환 (NAT)
- 장기적 솔루션
  - 주소 공간이 훨씬 큰 IPv6가 해결책
### Classless Interdomain Routing Scheme (CIDR)
- CIDR은 클래스 흐름 체계 대신 임의의 접두사 길이를 사용하여 네트워크 번호를 나타냄
  - ex) 205.100.0.0/22
  - /22는 마스크를 의미함 : 마스크의 길이가 22비트임을 나타냄
- CIDR을 사용하면 주소 클래스 없이 접두사에 따라 패킷이 라우팅됨
  - CIDR 라우팅 테이블의 항목에는 32비트 IP주소와 32비트 마스크가 포함됨
- CIDR을 사용하면 단일 라우팅 항목으로 classful addresses 블록을 처리할 수 있는 슈퍼넷 기술 사용가능
### CIDR Aggregation
- 예를 들어, 회사에 128.56.24.0, 128.56.25.0 ... 128.26.27.0 4개의 연속 /24 네트워크 할당
- 몇 라우터에서는 이러한 네트워크 모두가 동일한 발신 회선을 사용하는 경우가 많음
- CIDR 집계를 수행하여 라우터의 항목 수를 줄일 수 있음
- CIDR 체계는 /22 네트워크 주소를 바이너리 스트림으로 변환하고 비트당 AND 논리연산을 수행하여 라우터의 4개의 엔트리를 1개의 엔트리로 줄임
### CIDR Scheme and Range
- 라우팅 테이블 폭발 문제를 해결하기 위해 CIDR이 제안됨
- 네트워크는 접두사와 마스크로 표시됨
- 모두 동일한 발신 회선을 사용하는 경우 가변 길이 마스크를 사용하여 연속된 Class C 주소 그룹 요약
- 라우팅은 클래스 대신 주소의 접두사에 따라 수행됨
### CIDR Supernetting Example
### New Address Allocation Policy
- CIDR 기능을 활용하고 라우팅 테이블 크기를 줄이기 위한 새로운 주소 할당 정책 제안
- Class A, B는 요구 사항이 명확한 경우에만 할당
- Class C의 연속 블럭에는 최대 64개의 블록이 할당되므로 범위 내 모든 IP 주소에 공통 접두사가 지정됨
- CIDR 집계에서는 블록에 1개의 항목만 필요함
- 논리적 패킷 흐름을 물리적 흐름으로 쉽게 집계할 수 있도록 주소 할당은 네트워크의 물리적 토폴로지를 반영해야 함
### Longest Prefix Match
- 가변 길이 접두사를 사용하려면 지정된 IP 주소와 일치하는 항목이 여러 개 있을 때 가장 긴 접두사를 찾기 위해 라우팅 테이블을 검색해야 함
- ppt 128.25.24.0과 128.56.25.0에 패킷이 주소 128.56.24.1을 가지고 올 경우 주의
***
# 2. ARP, Fragmentation and Reassembly
***
### Address Resolution Protocol (ARP)
- IP 주소는 라우터와 최종 시스템의 논리적 토폴로지 측면에서 정의되기 때문에 논리적이라고 함
- 논리적 IP 주소를 기본 네트워크 기술의 특정 물리적 주소로 변환해야 함
- 현재 이더넷은 IP가 실행되는 가장 일반적인 네트워크임
- 이더넷은 48비트 MAC 주소 형식을 사용함
- 호스트는 IP 주소를 MAC 주소에 어떻게 매핑하는가?
- 주소 확인 프로토콜인 ARP는 IP 주소와 실제 주소 간의 변환을 제공함
### ARP 아이디어 예제
### Fragmentation and Reassembly
- IP의 강점 중 하나는 다양한 물리적 네트워크에서 작동할 수 있다는 것
- 각 물리적 네트워크는 일반적으로 전송할 수 있는 패킷에 특정 패킷 크기 제한을 적용함
- 이를 최대 전송 단위 maximum transmission unit MTU라고 함
- IP가 물리적 네트워크의 MTU보다 큰 패킷을 전송하려는 경우 IP는 패킷을 이에 맞게 더 작은 조각으로 나누어야 함
- 프래그먼트화는 소스 수준 또는 중간 라우터에서 수행할 수 있음
- 대상 IP는 프래그먼트를 원래 패킷에 조립하는 유일한 주체임
- 프래그먼트를 재조합하기 위해 대상은 동일한 패킷에 속하는 모든 프래그먼트를 수신할 때까지 기다림
- 네트워크에서 하나 이상의 프래그먼트가 손실되면 목적지는 어셈블리 프로세스를 중단함
- 프래그먼트화는 좋은 기능처럼 보일 수 있지만, 미묘한 성능 저하가 수반됨
### EX : Fragmenting a Packet
***
# 3. DHCP, NAT
***
### DHCP
- Dynamic Host Configuration Protocol 동적 호스트 구성 프로토콜
- TCP/IP 네트워크에 연결하는 호스트를 자동으로 구성함
- 이전 프로토콜인 부트스트랩 프로토콜에서는 디스크가 없는 워크스테이션을 네트워크에서 원격으로 부팅할 수 있었음
- 서버 포트에는 잘 알려진 UDP 포트 번호 67을, 클라이언트 포트에는 68번을 사용함
- DHCP는 부트스트랩 프로토콜의 기능을 기반으로 구성 정보를 호스트에 전달함
- 이 기능은 인터넷 서비스 공급자가 호스트에 임시 IP 주소를 할당하고 제한된 IP 주소 공간을 최대한 활용하기 위해 광범위하게 사용됨
- 호스트가 IP 주소를 얻으려는 경우, 호스트는 물리적 네트워크에서 DHCP-Discover 메시지를 브로드캐스트함
- 네트워크의 서버는 IP 주소 및 기타 구성 정보를 제공하는 DHCP 제안 메시지로 응답함
- 호스트에 여러 서버와 메모리가 적용되는데 호스트가 오퍼 중 하나를 선택하고 서버 ID가 포함된 DHCP 요청 메시지를 브로드캐스트한다고 하면
- 선택한 서버가 해당 IP 주소를 호스트에 할당하고 일정 기간 동안 호스트에 IP 주소를 할당하는 정확한 DHCP 메시지를 할당함
### Network Address Translation (NAT)
- 이전에는 사설 인터넷에서 사용할 수 있도록 3가지 범위의 사설 주소 또는 등록되지 않은 주소가 따로 지정되어 있는 것을 봄
- 등록되지 않은 개인 주소가 있는 패킷은 해당 사설 네트워크 내에서 유효함
- 하지만 글로벌 인터넷의 라우터에서는 이러한 데이터를 폐기함
- 네트워크 주소 변환, NAT는 사설 인터넷에 있는 호스트의 패킷을 인터넷을 통과할 수 있는 패킷으로 매핑하는 방법이다
- 또한 글로벌 인터넷에서 도착하는 패킷을 사설망의 적절한 대상 시스템으로 전송함
- 이를 위해 기기는 사설망과 공용 네트워크 사이에서 에이전트 역할을 함
- NAT을 사용하면 많은 호스트가 제한된 수의 등록된 IP 주소를 공유할 수 있음
### Placement of Operation of a NAT Box
- 사설망의 시스템이 단일 등록 IP 주소를 공유하는 경우
- 사설망의 시스템이 대상이 사설망 외부인 패킷을 생성하면 해당 패킷은 NAT 라우터로 전송됨
### NAT Operations
- NAT 라우터는 사설 네트워크의 패킷을 인터넷으로 매핑하고 그 반대로 매핑하기 위한 테이블을 유지 관리함
- 컴퓨터가 인터넷으로 향하는 패킷을 생성할 때마다 NAT 라우터의 테이블에 새 엔트리가 생성됨
- 엔트리에는 컴퓨터의 사설 IP 주소와 패킷의 TCP 또는 UDP 포트 번호가 포함됨
- NAT 라우터는 글로벌 IP 주소가 해당 주소인 레지스트리를 사용하여 인터넷에 패킷을 전송함
- 응답 패킷이 도착하면, 대상 패킷과 동일한 글로벌 IP 주소더라도 표를 보고 포트 번호를 사용하여 원래 사설 IP 주소 및 포트 번호를 검색함
### NAT Discussions
- 이론적으로 하나의 공용 IP 주소는 네트워크 주소 변환 기법을 통해 최대 2^16개의 서로 다른 사설 IP 주소를 지원할 수 있음
- TCP/UDP 포트 번호는 NAT 상자의 테이블 항목에 사용할 수 있는 16비트로 이루어지기 때문
- 하지만 NAT 작업과 런타임에 드는 오버헤드를 고려해야 함
- 한 가지 가능한 문제는 NAT가 IP 계층에서 이를 구현하고 있지만 조회 테이블에 TCP/UDP 포트 번호를 사용하여 전송 계층 정보를 활용한다는 것
- 이는 OSI 계층 아키텍쳐 위반임
- 즉, 상위 계층은 하위 수준에서 제공하는 서비스를 사용하지만 반대는 아님
***
# 4. IPv6
***
### IPv6
- IPv4는 수년 동안 인터넷 작업에 필수적인 역할을 해옴
- 그러나 32 비트 IPv4주소로는 결국 폭발적으로 증가하는 주소를 수용할 수 없었음
- 새 버전 IPv6는 v4에서 v6으로의 전환을 완료하는데 몇 년이 걸리기 때문에 IPv4와 상호 운용되도록 설계됨
- 2가지 주요 변경사항
  - 더 긴 주소 필드 : IPv6는 주소 지정에 128비트를 사용하며 3.4 X 10^38 개 호스트까지 지원할 수 있음
  - 단순화된 헤더 형식 : 각 패킷 헤더의 처리 속도를 높이기 위해 모든 필드의 크기가 고정되어 있음
### IPv4 vs IPv6 Overview
- 둘다 버전 유지
- IPv6는 헤더길이, ID, 플래그, 오프셋 및 헤더 체크섬을 모두 넘김
- IPv6는 데이터그램 길이를 페이로드 길이로 추가로 대체함
- 헤더별 프로토콜 유형, 홉 제한 별 TTL(이탈 시간), 트래픽 클래스별 TOS(서비스 유형)
- 새 필드인 플로우 필드 생성
### IPv6 Header Format
- 헤더와 선택적 확장 헤더로 구성됨
- 기본 헤더의 형식은 그림에 나와 있음
- 트래픽 클래스는 차별화된 서비스를 지원하기 위한 것
- 플로우 레이블 필드를 사용하여 패킷에서 요청한 서비스 품질을 식별할 수 있음
- IPv6의 플로우는 특정 소스에서 특정 목적지로 향하는 패킷의 시퀀스로 정의되며, 해당 소스에 대해 중간 라우터에서 특별한 처리를 필요로 함
- 페이로드 길이는 35535B까지의 data excluding header의 길이, 64kb로 제한되지만 확장 헤더의 옵션을 사용하여 더 큰 페이로드를 전송할 수 있음
### Extension Headers
- 기본 헤더에서 제공하지 않는 추가 기능을 지원하기 위해
- IPv6에서는 기본 헤더와 페이로드 사이에 임의의 수의 확장 헤더를 배치할 수 있음
- 확장 헤더는 다음 헤더 필드에 연결됨
- 확장 헤더는 IPv4에서 옵션처럼 작동하지만 더 효율적이고 유연함
- IPv6에서는 확장 헤더를 사용하여 64kb를 초과하는 페이로드 크기를 점보 패킷이라고 함
- 고속 네트워크, 대규모 데이터 애플리케이션, 슈퍼 컴퓨팅 애플리케이션은 더 큰 페이로드 크기를 사용하는 것을 장려함
- 기본 헤더의 페이로드 길이는 0으로 설정해야 함
- 옵션 길이 필드는 점보 페이로드 길이 필드의 크기를 바이트 단위로 지정함
- 마지막으로 32비트 점보 페이로드 길이 필드는 최대 4GB까지 가능한 페이로드 크기를 지정함
- IPv6에서는 소스 호스트만 프래그먼트화를 수행할 수 있음
- 중간 라우터는 더이상 프래그먼트화를 수행하지 않음
- 그 이유는 중간 라우터에서 라우팅 속도를 높이기 위함
- 패킷 길이가 네트워크의 최대 전송 단위 MTU 보다 길면 라우터는 단순히 패킷을 삭제하고 ICMP 오류 메시지를 소스로 다시 보냄
- 소스 홀드는 경로 MTU 검색 절차를 수행하여 소스에서 목적지까지의 경로를 따라 최소 MTU를 찾을 수 있음
- 한 가지 단점은 소스와 대상 간의 경로가 상당히 고정되어 있어야 한다는 것
- MTU 검색은 경로로서 업데이트된 정보를 만들지 않음
- 소스가 패킷을 프래그먼트화하려는 경우 소스는 패킷의 각 프래그먼트에 대해 그림에 표시된 대로 프래그먼트 확장 헤더를 포함
- IPv4와 마찬가지로 IPv6도 소프트 호스트가 패킷이 목적지에 도달하기 위해 방문하는 라우터의 순서를 지정할 수 있음
- 이 옵션은 ppt 그림에 표시된 라우팅 확장 헤더에 의해 정의됨
### IPv6 Addressing
- 3가지 범주로 분류됨
  - Unicast
  - Multicast
  - Anycast
- IPv6 의 긴 주소에 사용할 수 있는 표기법은 16진수 표기법
### Migration from IPv4 to IPv6
- IPv4 네트워크와 호스트가 널리 배포되었으므로 IPv4에서 IPv6으로의 전환시간을 줄이려면 마이그레이션 문제를 해결해야 함
- 현재 솔루션은 주로 IPv4 및 IPv6 기능이 모두 제공되는 이중 IP 계층 또는 이중 스택 접근 방식을 기반으로 함
- 라우터는 IPv4 프로토콜과 IPv6 프로토콜을 모두 지원하며 두 가지 유형의 패킷을 모두 전달할 수 있음