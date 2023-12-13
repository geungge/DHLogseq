### 이전 (2023.10.15)
collapsed:: true
	- Maskset
	- OS Task 구성
	- EEPOM Block feature
	  collapsed:: true
		- Immediate block usage
			- 별도의 Queue를 가지고 있지만 인터럽트 할 수 있다
			- Immediate block
			- standard block
		- Write Verification
			- 한 블럭을 쓰고 난 뒤 검증을 한다 -> 속도가 느려질 수 있다
		- Dataset Block type usage ??
			- native block이 1개만 있는것
			- reduntant 2개의 block이 있는것
			- dataset
	- Controlled RAM
		- soft reset : 램값 유지
		- por : 램 clear
	- Rom Tst
		- booting 1회 가능
		- 표준 기능은 아니다
		- utip s19 파일포멧 전환 필요 (MCRT / FBL)
	- Ram Tst
	  collapsed:: true
		- 개별 설정 가
	- Diag
	  collapsed:: true
		- DET
			- 예기치 못한 오류제거 (ClassAutosar 표준 , 초기화오류 , 에러타임 이상)
		- FIM
			- Yes , Triggered By Dem
			- Diagnostic Event Manager (Dem)
			- 표준
			- SW컴포넌트 기능요소를 사용하지 말지 결정
			- Polling : FIM의 메인 함수가 주기적으로 실행 여부를 관리하는
			- TriggerByDem : DEM과 유기적으로 연동되어서 이벤트를 통해서 실행여부 결정
		- DCM (Diagnostic Communication Manager)
			- 대부분 사용
			- ISO14229 와 ES95486 사양을 따른다.
			- 2개 동시 X
		- UDS on protocol
		- UDS Response
			- Response Pending 메시지를 몇 개까지 보낼지 설정하는 부분
			- 설정된 횟수 이후에는 General Reject 를 응답한다
		- Diagnostic Service
			- ES 사양 기반
		- Communication Control Service
		- Supported Security Algorithm
			- 요구되는 사이버 시큐리티 사양에 맞춰 선택
			- DCM에서 HSAC , CSAK 동시 지원 안된다
			- Gateway같은 특정제어기는 CSAC , 나머지는 HSAC 요구
		- Paged Buffer
			- 어떤 SID의 하나의 Frame이 256bytes 일 경우 분할하는 기능
		- DEM (Diagnostic Event Manager)
			- type1 , 모두 used
			- Error를 Event로 define 하는 모듈
			- Event / DTC
				- Combination Event
					- Normal : 1개의 DTC에 1개의 EVENT 만드는 형
					- Type1 : 1개의 DTC에 여러개의 EVENT 만드는 방식
						- 예) BusOff = 단선 + ....
				- DTC Suppression
					- 런타임 중에 특정 DTC를 외부에서 엑세스 하지 못하도록 설정하는ㄴ 기능이다. 단 DEM 내부동작엔 영향을 미치치 않는다
				- Enable condition diagnostic event
					- Event의 처리 여부를 Application에서 설정할 수 있다
					- 만약 Event에 할당된 EnableCondition 을 Application 에서 API Xxx_SetEnableCondtion을 통해 false 로 만들경우 보고된 event는 처리되지 않고 버려진다
				- Storage condition of diagnostic event
					- Enable condition과 유사한 기능이다. Event Memory에 Event를 저장할지 말지를 결정할 수 있다
					- API Xxx_SetStorageCondition
				- Counter based debouncing
					- 특정한 시간 동안 유지 되거나 , 임계치만큼 반복되는지에 따라 Event의 test값을 도출할 수 있는 알고리즘을 선택할 수 있게 옵션 구비
			- Event / DTC status
				- 언급 없음
			- Behavior of ClearDiagnosticInformation
				- 언급 없음
			- Aging / Indicator
				- event 속성 중에서 indicator 와 healing이 있다
				- event가 발생할 경우 경고등을 띄우는 것이 indicator 이고,
				- 그 이후 이를 해제하는 거싱 Healing입니다.
			- DTC data
				- EVENT 저장시에 Data를 Application에서 읽지 않고 미리 저장된 data를  EventMemory에 별도로 저장하는 기능
				- Record Number를 부여하는 방법에는 2가지가 있다
					- Calculated : 순차적 , 일반적으로 사용
					- Configurated : 사용자가 설정을 통해 부여한 비순차적인 방법
		-
			-
	- Watchdog
	  collapsed:: true
		- 대부분은 SRS대로 설정
		- Supervision Feature
			- classic autosar에서 정의하는 Watchdog Manager (WdgM)의 기능
			- Alive Supervision
				- 일반적인 Supervision
				- Task , Runnable , SWC 들이 정해진 주기로 정해진 횟수가 실행되는지 감
			- Deadline Supervision
				- OS의 Timing Protection과 유사하게, 코드상에 Checkpoint를 두지점을 선정하고 그지점 간의 수행시간이 정해진 시간내로 떨어지는지를 보는 것입니다.
			- Logical Supervision
				- 정해진 로직의 흐름이 순서대로 수행되는지를 감시하는 내
				- Internal Supervision : 1개의 Entity
				- External Supervison : 그 이상
			-
	- E2E
	  collapsed:: true
		- 기본적으로 E2E library를 사용하므로 별도의 설정이 필요없다
	- CRYPTO
	  collapsed:: true
		- HSM으로 유도되는 알고리즘 중 어느 내용을 쓸 것인지를 결정할 수 있다
		- HSAC / CSAC / Secure Flashing
		- 선택이 어렵다면 메모 등으로 남겨서 오토에버 담당자에게 알린다
		- Key Set의 경우에는 최대 50개까지 지원
	- Low Power Management
	  collapsed:: true
		- MCU의 모드를 Sleep 상태로 전환할 때 사용
		- SWP(Software Platform)에서는 MCU를 Sleep으로 변경할 경우 위의 call out 함수를 제
	- COM Functions
	  collapsed:: true
		- CAN
			- Tranceiver
				- other로 선택하면 CDD Template 로 제공
				- CDD는 초기화 및 모드 변경에 대한 Call out 함수를 제공하고 사용자는 body부분 구현
			- NM Type
			- Paritial Network Support
			- Paritial Network Identifier
			- Partial Network CAN ID (WUF) : 특정 tranceiver 에서 지원한다
			- J1939 SUpport
				- [warcan_05.pdf (eskorea.net)](http://www.eskorea.net/html/data/technique/warcan_05.pdf)
				- 트럭 및 버스와 같은 상업차량에서 사용되는 네트워크 통신 프로토
			- Support DM Control on abnormal voltage
			- Bus Load Detecting
				- CAN 버스 점유율
					- 예) 통신속도 100bps , 메시지 1개 10bit -> 1개 = 0.1초 
					  1초에 1개 보내면 10% , 2개 보내면 20%
				- 자동차 제조사에서는 일반적으로 버스로드 상한선을 정의해두고 사용한
		- UART
			- MC356 과 RH850은 지원하지만 나머지 MCU는 MCAL에 있는 것을 직접 사용
		- LIN
		- XCP
		  collapsed:: true
			- CANape , INCA 지원
			- CANape
				- 계측
				- calibration
				- CAN으로 memory read/write 등 디버깅이 가능하지만 별도 비용 필요
	- IO
	  collapsed:: true
		- 사용자 IO 들에 대해서 배포할 경우 별도의 설정을 하지 않는다
		- SRS는 사용할 기능에 대해서 설정
		- 배포시에는 sample 설정으로 송부
	- SPI
	- MUX_DEMUX_IF
	- FBL
		- FBL2.0 : secure flash 대응 가능
	- Memory Mapping
	- GENERAL
		- OTA
	- 확인 or 문의
	  collapsed:: true
		- 비용
			- 최초 작성 시 : 항목 추가 시 비용 증가 여부
				- 비용이 들지 않는 문제라면 왜 굳이 used/unused를 선택하라고 하는가?
				- 변경 사항이 있을 때 사용자가 바꿀 수 있는가?
			- 향후 기능 추가 : 가능하다면 나중에 enable 요청 시 비용이 드는가?
		-
		- CRYPTO
			- Secure Access , Secure Flash , FBL  외에 application에서 필요에 따라 사용하는 경우 선택하는 것인가요?
		- COM FUCNTIONS
			- NM은 OSEK or CAN_NM
			- Partital Network 지원 여부
			- J1939 지원 여부
			- Support DmCOntrol on abnormal voltage
			- Bus Lad Detecting
- ### 현재 (2023.10.16)
  collapsed:: true
	- MCU Mask set  : Future 문의
	- CPU Clock : 최대 사용 clock을 적는 것인가? 아니면 사용 clock을 적는 것인가? 추후 변경이 가능한가?
	- Controlled RAM : run time 중에 사용? 추후 on/off 가능?
	- ROMTST : FBL에서 하는 거 아닌가?
	- DIAGNOSTICS
		- FIM : 추후?
		- 10 sub id 90은 어떻게
		- EVENT/DTC status
		- memory displacement
	- COM_FUNCTIONS
		- LIN cluster name
		- Transmission Mode Selection Configuration
		- ADC port for battery voltage input vs IO voltage monitoring
	-
- ### 용어 정리
  collapsed:: true
	- DCM : Diagnostic Communication Manager
	- DEM : Diagnostic Event Manager
	- DET : Diagnostic Event Tracer
	- FIM : Functional Inhibition Manager
	-
- 문의사항 정리(2023.10.26)
  collapsed:: true
	- mnobilgene C Studio에서 설정이 가능한 부분과  오토에버에 변경을 요청해야 하는 항목 구분
	- DIAGNOSTICS
		- 3.4.1 sub function 항목을 정확하게 알 수 없다 (특히 0x10  , 0x27)
		- 3.6 Supported Security Algorithm
		- 항목들에 정확한 이해가 부족한데 추후에 변경하는 것이 가능한
	- Security 관련
		- DIAGNOSTIC / support security algorithm
		- CRYPTO
		- FBL
	- IO
		- IO general 과 Mangager configuiration 차이는?
		-
	-
- ### HSM  / Secure Flash / Secure Boot
  collapsed:: true
	- 차이점
		- HSM (Hardware Security Module)과 SecureFlash는 두 가지 서로 다른 보안 기술입니다. HSM은 주로 암호화 키 및 보안 연산을 보호하고 관리하는 데 사용되는 하드웨어 장치입니다. 이는 주로 암호화, 전자 서명, 인증 등과 관련된 보안 작업을 지원합니다. 반면, SecureFlash는 데이터 저장 및 보안에 중점을 둔 기술로, 플래시 메모리에 대한 보안적인 접근 방법을 의미합니다. 이 기술은 데이터를 안전하게 저장하고 보호하기 위해 플래시 메모리에서 사용됩니다.두 기술은 서로 다른 용도와 목적을 가지고 있지만, 보안에 관련된 측면에서 공통점을 갖고 있을 수 있습니다. 예를 들어, HSM은 암호화 키를 안전하게 저장하고 사용자 인증에 사용될 수 있으며, SecureFlash는 저장된 데이터의 무결성과 보안을 유지하는 데 활용될 수 있습니다.
	- 링크
		- [HSM(Hardware Security Module)을 이용한 AUTOSAR 자동차 보안 : 네이버 블로그 (naver.com)](https://m.blog.naver.com/mdstec_auto/222070249876)
- ### 12/5
	- #### 미구현
		- 진단
		- 전압 모니터링 후 제어 V_MONITOR_OUT
		- 와치독
		- LIN 수신 처리
	- #### 변경 필요
		- OS Task Delay 사용 금지
		- 컴포넌트간에 bit field 데이터 사용 불가
	- #### 컴포넌트
		- CAN
			- 모빌진 commstack에 송신,수신 구멍이 생긴다
			- ASW에도  동일한 인터페이스를 만들어야
		- 2개
			- 1개
				- Rte_Write
				- Rte_Read
		- V cycle
			- 아키텍처 설계  & 모빌진 설정
			  logseq.order-list-type:: number
			- 상세설계 : 손코딩
			  logseq.order-list-type:: number
			- SRS
			  logseq.order-list-type:: number
				- 품질보증
				  logseq.order-list-type:: number
				- 형상관리
				  logseq.order-list-type:: number
				- 문제해결관리
				  logseq.order-list-type:: number
				- 변경요청관리
				  logseq.order-list-type:: number
	- #### 절차
		- 아키텍처 설계
		  logseq.order-list-type:: number
			- 컴포넌트 2개 (ASW10ms + CDD 10ms)
			  logseq.order-list-type:: number
			- Rte_Write , Rte_Read
			  logseq.order-list-type:: number
			- 레거시는 계속 진행
			  logseq.order-list-type:: number
			- 문의
			  logseq.order-list-type:: number
				- 모빌진 나오기 전에 준비하는 형식인가?
				  logseq.order-list-type:: number
		- 모빌진 설정
		  logseq.order-list-type:: number
			- 문의
			  logseq.order-list-type:: number
				- 교육없이 진행이 가능한가?
				  logseq.order-list-type:: number
		- 상세설계 
		  logseq.order-list-type:: number
			- 기본 손코딩
			  logseq.order-list-type:: number
			- 리버스 엔지니어링으로 산출물 추출
			  logseq.order-list-type:: number
	-
-
-