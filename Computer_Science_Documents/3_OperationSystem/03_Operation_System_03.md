# 운영체제론 정리3[디스크 스케쥴링 ~ 보안]
## 디스크 스케쥴링
> 사용할 데이터가 디스크 상 여러 곳에 저장되어 있을 경우 데이터를 액세스하기 위해 디스크 헤드가 움직이는 경로를 결정하는 방법.
- 디스크 스케쥴링의 목적은 **처리량 최대화, 평균 응답 시간 최소화, 응답 시간 편차의 최소화**로 볼 수 있다.

> ### FCFS
> ![disk_schedule_fcfs](/Computer_Science_Documents/3_OperationSystem/img/disk_schedule_fcfs.png "disk_schedule_fcfs")
> - 간단한 스케쥴링으로 디스크 큐에 먼저 들어온 트랙에 대한 요청을 먼저 서비스하는 기법.
> - 디스크 대기 큐에 들어온 순서대로 서비스 하기 때문에 우선순위 따위는 씹어 먹어서 공평성을 보장한다.
> ### SSTF
> ![disk_schedule_sstf](/Computer_Science_Documents/3_OperationSystem/img/disk_schedule_sstf.png "disk_schedule_sstf")
> - 탐색 거리가 가장 짧은 트랙에 대한 요청을 먼저 서비스하는 기법.
> - 현재 헤드에서 가까운 거리에 있는 트랙을 헤드로 이동을 시키는 기법.
> - FCFS보다 평균 시간은 짧아지지만, 탐색 패턴이 **가운데 트랙이 서비스를 많이 받는 경향**이 있어서 안쪽, 바깥 쪽은 기아 상태가 발생할 수 있다.
> - 처리량이 많은 일괄 처리 시스템에서 주로 쓴다. 
> ### SCAN
> ![disk_schedule_scan](/Computer_Science_Documents/3_OperationSystem/img/disk_schedule_scan.png "disk_schedule_scan")
> - SSTF가 가지는 탐색 시간의 편차를 줄이기 위한 기법.
> - 현재 헤드의 위치에서 방향이 결정되면 탐색 거리가 짧은 순서에 따라서 안쪽, 바깥쪽 둘 중 하나를 찍고 다시 역방향으로 진행하는 기법.
> ### C-SCAN
> ![disk_schedule_cscan](/Computer_Science_Documents/3_OperationSystem/img/disk_schedule_cscan.png "disk_schedule_cscan")
> - 항상 **바깥 쪽에서 안쪽**으로 움직이면서 짧은 탐색 거리를 갖는 요청을 서비스하는 방법.
> - 트랙은 바깥 쪽에서 안 쪽으로 한 방향으로만 움직이며 끝 방향으로 이동하고 난 후에 안 쪽에 요청이 없다면 바깥 쪽의 끝으로 이동하고 난 후에 다시 안 쪽으로 이동하면서 서비스를 하는 개념이다.
> - 마치 과거의 Mario Bros 게임을 연상하면 더욱 도움이 될 듯 하다.
> ### LOOK
> ![disk_schedule_look](/Computer_Science_Documents/3_OperationSystem/img/disk_schedule_look.png "disk_schedule_look")
> - SCAN 기법을 기초로 사용되어 진행 방향의 마지막 요청을 서비스하고 그 방향의 끝으로 이동하는 것이 아닌 바로 역방향으로 진행하는 기법.
> ### 에센바흐(Eschenbach)
> 부하가 매우 큰 항공 예약 시스템을 위해 개발 되었고 C-SCAN 처럼 움직이며 전체 트랙이 한 바퀴 회전할 동안에 서비스를 받는다.
> ### SLTF
> 섹터 큐잉(Sector Queuing)으로 칭하며 디스크 대기 큐에 있는 여러 요청들을 섹터 위치에 따라 재정렬을 하면서 가장 가까운 섹터 먼저 서비스를 하는 개념.
> ### N-Step SCAN
> SCAN 기법의 무한 대기 발생 가능을 제거하였고 어떤 방향의 진행이 시작 되면 당시에 대기 중이던 요청들만 서비스하고 진행 도중 도착한 요청들은 한 데 모아서 다음 반대 방향 진행 때 서비스하는 기법.

 ## File / File System
 - File은 사용자가 작성한 서로 관련 있는 레코드의 집합체. 프로그램 구성의 기본 단위, **보조기억장치**에 저장된다.
 - File System은 파일의 저장, 액세스, 공유, 보호 등 보조 기억장치에서 파일을 총괄하는 파일 관리 기술로서 사용자와 **보조기억장치** 사이에서 인터페이스를 제공한다.
 - File System에서는 사용자가 주인공으로 적합한 구조를 형성할 수 있고, Backup과 Recovery 기능도 제공하고, 정보를 암호화와 해독할 수 있도록 제공한다.

## 파일 디스크립터(File Discriptor)
- File을 관리하기 위해 시스템이 필요한 파일에 대한 정보를 갖고 있는 제어 블록(FCB - File Control Block)의 개념이다.
- 보통 보조 기억장치 내에 저장이 되었다가 Open이 된다면 주 기억장치로 옮겨지게 된다. 보조 기억 장치는 상품 목록으로 생각하면 되고, 주 기억장치는 장바구니 내에 있던 상품 목록에서 일부를 주문한 개념으로 생각하면 되겠다.
- 당연히 File 마다 독립적이고 시스템(Window, Mac etc.)에 따라 다른 구조를 지니고 있다.
- 파일 시스템이 관리해서 **사용자는 쳐다도 못 본다.**
> ### 파일 디스크립터 내에는...
> - 파일 관련
>> 파일 ID, 파일 이름, 파일 크기, 파일 구조, 파일 유형, 파일 생성 날짜와 시간, 파일 제거 날짜와 시간, 파일 최종 수정 날짜와 시간, 파일 액세스 횟수, 파일 액세스 제어 정보 등
> - 보조 기억장치 관련
>> 보조기억장치 유형, 보조기억장치에서 파일 위치

## 순차 파일
- 레코드를 **논리적인 처리 순서**에 따라서 연속된 물리적 공간에 넣는 원리.
- 순차 접근이 가능한 **자기 테이프**에서 사용.
- 파일 구성 용이, 기억 공간의 이용 효율 향상, 처리 비용 절감 등의 장점이 있다.
- 파일에 새로운 레코드 삽입, 삭제를 할 때 파일 전체를 복붙해야 되어 효율이 낮고 응답 시간이 느리다.

## 인덱스 파일
- 순차 파일과 직접 파일에서 지원하는 편성 방법이 결합된 개념.
- 각 레코드를 키 값 순으로 논리적으로 저장하고 시스템은 각 레코드의 실제 주소가 저장된 색인을 관리한다.
- 레코드 참조 시에 색인을 탐색한 이후 색인(Index)이 가리키는 포인터를 사용해 직접 참조가 가능하다.
> - 구성으로 기본 영역, 색인 영역, 오버플로 영역으로 나뉜다.
> - 그 와중에 색인 영역에는 트랙 색인 영역, 실린더 색인 영역, 마스터 색인 영역 3가지가 있다.
- 당연히 순차 처리, 임의 처리 모두 가능하고 효율적으로 CRUD를 할 수 있는 장점이 있다.
- 색인이나 오버플로 처리를 위한 추가 저장공간이 필요한 단점이 있다.

## 직접 파일
- 파일을 구성하는 레코드를 **임의의 물리적 저장공간**에 기록하는 것이다.
- 레코드에 특정 기준으로 Key(키)가 할당되어, Hashin' Function(해시 테이블에서 Division, Multiple 등이 있었다. 까먹었다면 알고리즘을 참고할 것.)을 이용해 보조 기억장치의 물리적 상대 레코드 주소를 계산한 후 해당 주소에 레코드를 저장한다.
- 레코드는 해싱 함수에 의해 계산된 물리적 주소를 통해 접근이 가능하다.
- 임의 접근이 가능한 자기 디스크에서 활용한다.
- 파일 각 레코드에 직접 접근, 기록을 할 수 있어서 CRUD는 매우 빠른 편인 장점을 지닌다.
- 레코드를 해싱 함수로 주소 변경 과정이 필요해서 이 과정으로 인한 시간이 소요되고 기억 장치의 물리적 구조에 대한 지식도 가지고 있어야 하는 단점도 있음.

## Directory
> 운영체제론 문제에서 빈번하게 나오니 기억해두고 넘어가자.
- 디렉토리는 파일 시스템 내부에 내장되어 있어서 효율적인 파일 사용을 위해 파일에 대한 여러 정보를 가지고 있는 특수 형태 파일이다.
> ### 1단계 디렉토리
> - 가장 간단하고 모든 파일이 하나의 디렉토리 내에 위치하여 관리되는 구조.
> - 모든 파일들이 유일한 이름을 가지고 있어야 함.
> - 파일의 수나 사용자가 증가되면 파일 관리가 복잡해진다.
> ### 2단계 디렉토리
> - 중앙에 **마스터 파일 디렉토리(MFD)** 이 있고, 이외 노드에 사용자 별로 제공되는 **사용자 파일 디렉토리(UFD)** 가 있는 2 계층 구조.
> - 서로 다른 디렉토리에는 동일한 파일 이름 적용 가능.
> ### 트리 디렉토리
>> 현재 우리가 쓰고 있는 **Window, UNIX의 디렉토리 구조**
> - 하나의 루트 디렉토리와 여러 개 종속 디렉토리로 구성됨.
> - 동일한 이름의 파일이나 디렉토리 생성 가능.
> - 포인터를 사용해 디렉토리 탐색 및 파일 경로명은 절대 경로, 상대 경로를 사용.
> ### 비순환 그래프 디렉토리
> - 하위 파일이나 하위 디렉토리를 공동으로 사용할 수 있어 **사이클이 허용되지 않은** 구조.
> - 하나 파일이나 디렉토리가 여러 개 경로르 가질 수 있다.
> - 공유된 파일을 삭제할 경우 **고아 포인(Dangling Pointer)** 도 염려해야 함.
> ### 일반 그래프 디렉토리
> - 트리 구조에 링크를 첨가시켜 사이클을 그나마 허용하는 그래프 구조.
> - 불필요한 파일 제거를 위해 참조 계수기(카운터)가 필요하다.

## 자원 보호 기법
- 자원 보호 기법은 컴퓨터 시스템에서 사용자, 프로세스 등 같은 주체가 CPU, 기억장치 등과 같은 객체에 불법적으로 접근하는 것을 제어하고 객체에 물리적인 손상을 방지하는 개념.
> 자원 보호 기법의 종류는 각각 4가지로 나뉜다.
> - 접근 제어 행렬 : 자원 보호의 일반적인 모델로 객체에 대한 접근 권한을 행렬로 표기함.
> - 전역 테이블 : 가장 단순한 구현 방안으로 3개 순서 쌍인 영역, 객체, 접근 권한의 집합을 목록 형으로 정리한 기법.
> - 접근 제어 리스트 : 접근 제어 행렬에 있는 각 열에 대하여 즉 객체를 중심으로 접근 리스트를 구성한 기법.
> - 권한 리스트 : 접근 제어 행렬에 있는 각 행, 이는 영역을 중심으로 구성하여 각 사용자들에 대해 자격들로 구성되며(마치 MongoDB처럼) 자격은 객체와 그 객체에 허용된 연산 리스트로 볼 수 있다.

## 보안(Security)
- 컴퓨터 시스템 내에 있는 프로그램과 데이터에 대해 통제된 접근 방식을 제공하는 방안을 다룬다.
- 물리적, 환경적 취약함을 이용해 침입, 방해, 절도 등의 행위로 부터 막는 행위.

> 보안의 요건으로 기밀성(인가된 사용자에게만 제공), 무결성(인가된 사용자만 수정 가능), 가용성(인가 받은 사용자는 언제라도 사용 가능), 인증(패스워드, 인증 카드, 지문 검사 등), 부안 방지(데이터 송-수신 사실을 알 수 없도록 함) 5가지가 존재한다.

## 파일 보호 기법 / 보안 유지 기법
- 파일 보호 기법은 파일에 대한 일방적인 접근과 손상, 파괴를 방지하기 위한 기법이다.
> ### 파일 보호 기법 종류
> - 파일 명명(**Naming'**) : 접근하는 파일 이름을 모르는 사용자를 접근 대상에서 제외하는 기법. 이 개념은 의외로 나온다.
> - 비밀번호 : 우리가 늘 쓰는 비밀번호 그거 맞다.
> - 접근 제어 : 사용자에 따라 공유 데이터에 접근 권한을 제어하는 방법.
> ### 보안 유지 기법
> - 외부 보안 : 시설 보안, 운용 보안
> - **사용자 인터페이스 보안** : 운영체제가 사용자에게 신원을 확인하고 권한이 있는 사용자에게만 시스템의 프로그램과 데이터를 접근하는 보안 기법.
> - 내부 보안 : 하드웨어나 운영체제의 내장된 보안 기능을 이용해 시스템 신뢰성 유지와 보안 문제를 해결하는 기법.

