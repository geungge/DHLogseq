### 문의사항
collapsed:: true
	- Com 에서 signal base로 생성되지 않고 group base로만 설정된다
	- can group rx 는 되지만 void 형식으로 만 callback된다
	- can rte write는 만들어 졌지만, 기본적으로 어디에선가 보내고 있는데
		- rte write하면 원하는 data가 write되지 않는다
		- 기존 timing으로 data만 update하려면
	- can rx polling
	- Com_GaaRamInit8Bit
	- LIN Rx는 왜 event가 사용되지 않고 모양이 다른가?
	- logseq.order-list-type:: number
- e_ku_pe_sal_asr_swp_fbl_R231213
  collapsed:: true
	- main
		- EcuM_Init
			- StartOS
				- OsTask_BSW_Init
					- EcuM_StartupTwo
						- BswM_Init
						  BswM_EcuM_CurrentState
							- Rule_EcuStateTo_STARTUP_TWO
								- TrueAL_EcuStateTo_STARTUP_TWO
									- BswM_RequestMode
										- Rule_EcuState_STARTUP_TWO
											- TrueAL_EcuState_STARTUP_TWO
												- Rte_Switch_BswM_modeSwitchPort_InitState_InitState
													- Rte_Switch_BswM_modeSwitchPort_InitState_InitState
														- AppMode_InitCompleted(void)
	- TAS_OsTask_ASW_FG1_100ms
		- AppMode_Test
	- Fota_BootManager
		- Fota_CheckForAppl
			- Fbl_GetMainSwEntryPointAddress
			- Fota_FuncPtr
	- Os_ExceptionVectorTable
		- Reset_Handler
	- EcuM_Gen_AL_DriverInitOne
		- CanCM_Init
		- Wdg_43_Instance0_Init
		- Crypto_Init
		- Crypto_Exts_FormatKeyCatalogs
			- Crypto_Ipw_FormatKeyCatalogs
				- Crypto_Hse_SendMsg
					- Hse_Ip_ServiceRequest
						- Mu_Ip_IsResponseReady
- 포트 설정
  collapsed:: true
	- Configuration/ECU/MCal/ECud_Port.arxml/AUTOSAR/Port/Port/Containers/PortConfigSet/Sub Containers/PortContainer_A/Sub Containers
		- 좌측에서 AUTOSAR/Port/Port 선택하고
		- 우측 밑에서 Container 선택
	- IoHwAb/IoHwAbGeneral/IoHwAbPortPinChs/Port Pin Ch/+
	- IoHwAb/IoHwAbConfig/IoHwAbDigitalDirect/Logical/+
- Crypto 버전 에러
  collapsed:: true
	- CryIf_Cfg.c 에서 버전 비교 삭제
		- Generated/Bsw_Output/src/CryIf_Cfg.c
	- ECU/MCal/Cryto/CommonPublishedInformation/ArReleaseMintorVersion
		- 4->7
- APP jump 후 reset현상
  collapsed:: true
	- Hse_ip.c
		- Static_Code/Modules/b_mcal_nxp_S32K3xx/b_mcal_Crypto_nxp_S32Kxx/src
		- Hse_Ip_ServiceRequest 에서 Mu_Ip_IsResponseReady() 대기 부분 삭제
		-
- CAN RX
  collapsed:: true
	- CanIf_RxIndication
		- Can_Ipw_ParseData
			- Can_43_FLEXCAN_Ipw_ProcessRxMesgBuffer
				- Can_43_FLEXCAN_ProcessMesgBufferCommonInterrupt
					- Can_43_FLEXCAN_CommonIrqCallback
						- FlexCAN_IRQHandlerRxMB
							- FlexCAN_IRQHandler
	- TASK_OsTask_BSW_FG1_5ms_Com
		- Com_MainFuncitonRx
			- Com_RxSigGrpProcessing
				- Rte_COMCbk_ComISignalGroup_MsgGr_E2E_CAN_FU_L_BDC_FD_01_200ms
	-
- 빌드 FAST
  collapsed:: true
	- GenerateRte
	- GenerateIoHwAb
- Can Rx
  collapsed:: true
	- Event 방식
		- Rte_COMCbk_ComISignalGroup_MsgGr_E2E_CAN_FU_L_BDC_FD_01_200ms
		- SetEvent
		- OsTasK_ASW_FG1_RxTest
		- 메시지는 어떻게 전달할까?
	- Polling 방식
- Can Tx
  collapsed:: true
	- TASK_OsTask_BSW_FG1_10ms
		- CanNM_MainFunction
			- CanNm_LocalProcessNmMessage
				- NM은 RMS 또는 Normal일 경우에만 전송
				- CanNm_IntCanIfTransmit
					- CanIf_Transmit
						- CanIf_CanWriteService
							- Can_43_FLEXCAN_Write
	- TASK_OsTask_BSW_FG1_5ms_Com
		- Com_MainFunctionTx
			- Com_Transmit
				- PduR_UpTransmit
					- CanIf_Transmit
						- CanIf_CanWriteService
							- Can_43_FLEXCAN_Write
			- 타임아웃에 의해서 Com_Transmit가 호출된다 (LddNoOfTxIpdu = 5)
				- TX I-PDU 5
				- Com_GaaRamInit8Bit[52] 부터 실제 출력 데이터이다
				  id:: 6584e6b2-01dc-4b69-92bb-04dc1e7aece6
				- 이 값을 어떻게 바꿀수있나? 현재 코드상에 api는 없는 듯 하다
				- 초기값은 어디에서 설정하나?
- CAN NO COMM (Sleep)
  collapsed:: true
	- TASK_OsTask_BSW_FG1_5ms_Com
		- CanSM_MainFunction
			- CanSM_SilentCommRequested
				- CanIf_SetPduMode (CANIF_SET_TX_OFFLINE)
				- LpNetwork->ucModeStatus = CANSM_SILENTCOMMUNICATION;
		- CanSM_MainFunction
			- CanSM_SilentCommunication
				- CanSM_SetCanTrcvMode
					- CanSM_SetCanTrcvMode
						- CanIf_SetTrcvMode
						  id:: 658cd6b3-638d-4973-b880-26e1edb68232
							- CanTrcv_SetOpMode
							  id:: 658cd6b9-79de-47cd-af33-8c9f1e68af08
								- CanTrcv_StandbyMode
								- CanTrcv_SleepMode
								- CanSM_FullCommunication
		- CanSM_MainFunction
			- CanSM_NoCommunication
			- CanSM_SilentCommunication
		- CanSM_GaaNetwork[LddCanSmNwIndex].ucModeStatus
			- 초기화
				- 6 (CANSM_NOTOFULLCOMMREQUESTED
					- CanSM_MainFunction
						- CanSM_NoToFullCommRequested
				- 5 : CANSM_CHECKBUSOFF
					- CanSM_NoToFullCommRequested
				- 2 : CANSM_FULLCOMMUNICATION
					- CanSM_CheckBusOff
			- Sleep 요청
				- 8 : CANSM_SILENTCOMMREQUESTED
					- 시간이 좀 지나고
						- CanSM_FullCommunication
							- LpNetwork->ucReqModeStatus 값이 바뀌어서 호출이 가능한 듯
							- CanSM_SilentCommRequested
				- 1 : CANSM_SILENTCOMMUNICATION
				  id:: 658cdb29-54ce-4660-abe5-55fa59480b2c
					- CanSM_FullCommunication
						- CanSM_SilentCommRequested
		- ComM_MainFunction
			- ComM_GaaCurComMode[LddChnlIndex]
				- ComM_NoComMode
				- ComM_SilentComMode
					- ComM_SwitchComMMode
						- ComM_GaaSMReqComModeFun
						- CanSM_RequestComMode
							- LpNetwork->ucReqModeStatus = (uint8)ComM_Mode;
				- ComM_FullComMode
					- ComM_GaaSubComMode[LucIndex]
						- ComM_FullComNetwReq
						- ComM_FullComReadySleep
			- 슬립 변경시
				- ComM_FullComMode
					- ComM_FullComReadySleep
						- ComM_FullComNetwReq
						- ComM_SwitchComMMode
		- CanNm_MainFunction
			- LpChannel->ddModeStatus
			- LpChannel->ddNextModeState
			- LpChannel->ddModeStatus
		- Com_RxIndication
		  id:: 658d45f5-bdb1-4717-a8fe-4e627031d760
			- PduR_LoRxIndication
			  id:: 658d460c-1e05-47e2-805d-f05746a4a61d
				- CanNm_LocalProcessEIRATimer
					- CanNm_MainFunction
						- TASK_OsTask_BSW_FG1_10ms
						  id:: 658d463d-5b94-462a-9bff-92b9dd9ddb33
- LIN 통신 안됨
	- TASK_OsTask_BSW_FG1_5ms_Com
		- LinIf_MainFunction
			- LinIf_SendFrame
			  id:: 6585084c-7912-44ff-8e2b-8065f6c09a69
	- OsTask_BSW_ComMModeRequest
		- Rte_Runnable_BswM_BswM_Immediate_SwcModeRequest_ComMMode_LPUART_0_Start
		- BswM_Immediate_SwcModeRequest_ComMMode_LPUART_0
	- TASK_OsTask_BSW_ComMModeRequest
		- BswM_Immediate_SwcModeRequest_ComMMode_LPUART_0
			- Rule_ComModeRequest_LPUART_0_FULL_COM
				- TrueAL_ComModeRequest_LPUART_0_FULL_COM
					- ComM_RequestComMode
						- ComM_RequestComModeDirectChnl
	- TASK_OsTask_BSW_FG1_5ms_Com
		- ComM_SwitchComMMode
			- ComM_SwitchComMMode
				- LinSM_RequestComMode
	- AppLin_GucTestSchedule_LPUART_0 값을 1로 설정하니 LIN 통신이 되었다
		- Rte_Write_App_Lin_P_SR_LIN_SCHEDULE_CHANGE_LPUART_0_LinSchedule_LPUART_0
			- OsTask_ASW_FG2_LinScheduleRequest
			  id:: 658a2c07-fd97-4a0d-8896-4f235ebb9619
				- BswM_Immediate_SwcModeRequest_LinSchedule_LPUART_0
					- 여기서 table이 설정된다 (AppLin_GucTestSchedule_LPUART_0 )
	- Lin_43_LPUART_FLEXIO_au8LinChStatus  값이 2로 남아있다
		- 해결방안 : LinIf_GaaLtChannelInfo[].ucStartUpState 가  이전버전에서는 '1' , 현재버전에서는 '0'
		- LinIf_Init()에서 LinIf_GaaLtChannelInfo[LddChnlIndex].ucStartUpState == LINIF_CHNL_STARTUP_NORMAL 조건에 걸려서
		  LinIf_GaaChannelInfo[].pChannelRamData.ucChannelSts 값이  LINIF_CHANNEL_WAKEUP 상태로 남아있고
		  LinSM_RequestComMode -> LinIf_Wakup에서 LinIf_Wakup이 호출되지 않음
		-
- LIN TX DATA
	- LinIf_MainFunction
		- LinIf_SendFrame
			- PduR_LinIfTriggerTransmit 에 LaaResponseData 변경
				- PduR_LoTriggerTransmit
					- PduR_GaaIfTriggerTransmit
						- Com_TriggerTransmit
							- memcpy((P2CONST(Com_GaaTxPduInfoPtr)
								- 0 UART0_ MLM0_Comm1
								  1  UART0_ MasterReq
								  2 UART1_ MLM1_Comm1
								  3 UART1_MasterReq
								  4 CAN FU
								  5 CAN SAL
									- Com_GaaTxPduInfoPtr[0].SduDataPtr[0]++;
			- Lin_43_LPUART_FLEXIO_SendFrame  = Lin_SendFrame : PduInfoPtr
				- Lin_43_LPUART_FLEXIO_Ipw_SendFrame : PduInfoPtr
					- Lpuart_Lin_Ip_SendFrame : PduInfo
- LIN RX
  collapsed:: true
	- Lpuart_Lin_Ip_IRQHandler
		- Lpuart_Lin_Ip_FrameIrqHandler
			- Lpuart_Lin_Ip_ProcessReceiveFrameData
				- Lpuart_Lin_Ip_GetBytetoBuffer
					- 여기에서 checksum 에러가 발생한
				- Lin_43_LPUART_FLEXIO_Ipw_Callback
	- CAN RX 와 다르게 Event가 사용되지 않는다
- POLLING READ
  collapsed:: true
	- Runnable에서 Can Be Invoked Concrrently를 disable로 해도 아무런 변화가 없다
- MODE
  collapsed:: true
	- TASK_OsTask_BSW_Mem_Process
		- NvM_MainFunction
			- NvM_ProcessReadAllBlocks
				- BswM_NvM_CurrentJobMode
					- Rule_EcuStateTo_STARTUP_THREE
						- TrueAL_EcuStateTo_STARTUP_THREE
							- BswM_RequestMode
								- Rule_EcuState_STARTUP_THREE
									- TrueAL_EcuState_STARTUP_THREE
										- Rte_Switch_BswM_modeSwitchPort_InitState_InitState
											- AppMode_InitCompleted
	- TASK_OsTask_BSW_Mem_Process
		- NvM_MainFunction
			- NvM_ProcessReadAllBlocks
				- BswM_NvM_CurrentJobMode
					- Rule_EcuStateTo_STARTUP_THREE
						- TrueAL_EcuStateTo_STARTUP_THREE
							- BswM_RequestMode
								- Rule_EcuStateTo_RUN_From_STARTUP
									- TrueAL_EcuStateTo_RUN_From_STARTUP
										- Rte_Switch_BswM_modeSwitchPort_EcuMode_EcuMode
											- AppMode_EcuModeSwitched (STARTUP -> RUN)
												- Rte_Call_clientPort_StateRequest_RequestRUN
													- EcuM_RequestRUN
														- BswM_RequestMode((BswM_UserType)ECUM_STATEREQ_ID, ECUM_REQUEST_RUN);
	- TASK_OsTask_BSW_Mem_Process
		- NvM_MainFunction
			- NvM_ProcessReadAllBlocks
				- BswM_NvM_CurrentJobMode
					- Rule_EcuStateTo_STARTUP_THREE
						- TrueAL_EcuStateTo_STARTUP_THREE
							- BswM_RequestMode
								- Rule_EcuState_STARTUP_THREE
									- TrueAL_EcuState_STARTUP_THREE
										- Rte_Start
											- AppMode_ComMModeSwitched_PNC120
											- AppMode_ComMModeSwitched_LPUART_0
											- AppMode_ComMModeSwitched_LPUART_1
											- AppMode_PduGroupSwitched_CAN_FU_PNC120_Rx
											- AppMode_PduGroupSwitched_CAN_FU_PNC120_Tx
											  id:: 658bcfe6-1313-4b44-a441-be049f96a5a7
											- AppMode_CanSMBorStateSwitched_CAN_FU
	- TASK_osTask_ASW_FG1_100ms
		- AppMode_Test
- NM
  collapsed:: true
	- CanNm_LocalRepeatMessageState
		- CanNm_IntRequestNormalOperationState
		- CanNm_IntRequestReadySleepState
	- CanNm_RxIndication
		- CanNm_IntRequestRepeatMessageState
			- LpChannel->ddNextModeState = CANNM_REPEAT_MESSAGE_STATE;
- Booting
	- App 시작 주소 : 0x00432200 (_start_T)