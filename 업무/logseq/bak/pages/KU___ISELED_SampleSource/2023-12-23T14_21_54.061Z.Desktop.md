- 코드분석
  collapsed:: true
	- ILaS_Init_Interface(IseledContainer_0_VS_0.nrOfILaSNetworks, &IseledContainer_0_VS_0)
		- NrOfILaSNetworks
		- ILas_ConfigType *ConfigPtr
	- ILaS_Reset (0,0)
		- address
		- NetworkNr
	- ILaS_Network_Init(&initStruct, &ILaSResultStrip0, 0U)
		- ILaS network 초기화
		- NetworkInitPtr
		- NetworkInitResultPtr
		- NetworkNr
	- ILaS_Set_3PWM_Hi(u8Red, u8Green, u8Blue, 0U, 0U)
		- RGB 값 설정
		- R
		- G
		- B
		- Address
		- NetworkNr
	- ILaS_Set_Fw_Mode(2U, 1U, 0U, 0U)
		- 기능 : the forwarding mode of Branch Devices
		- FwMode
		- NrOfLA
		- Address
		- NetworkNr
	- ILaS_Branch_Read_Item(1U, &ILaSResultStrip1, 1U, 0U)
		- 기능 : Get diagnostic value of all LEDs
		- Param : 0-3 , PWM channel
		- PWM value of all devices
		- Address
		- NetworkNr
	- IseledCb
		- event callback
- PORT
  collapsed:: true
	- 114 PTB14 lpspi1_clk  , input
	- 112 PTB16 lpspi1_sout , input/output
	-
- 궁금한 점
  collapsed:: true
	- NetworkNr / NrOfLA / Address / Strip / PWM channel
- 예제 설명
  collapsed:: true
	- 1. Example Description
	      This application demonstrate the usage of Iseled driver in Real Time Drivers.
	      It is applied to INT220Q - ILAS ADK. However, it can also be adapted for an ISELED strip by updating the NUM_IC from 6U to 17U.
	      1.1 The application software functionality
	          This application includes an S32 Design Studio project that configures Iseled driver to use 2 LED strips.
		- ILaS_Init_Interface
		    Initialize interfaces used.
		- ILaS_Reset
		    Send the reset message to selected strip.
		- ILaS_Network_Init
		    Send initialization message to selected strip.
		- ILaS_Set_3PWM_Hi
		    Set current color for one, many or all LED of the selected strip.
		- ILaS_Set_Fw_Mode
		    Set the forwarding mode of Branch Devices
		- ILaS_Branch_Read_Item
		    Read the respective items from all devices in a selected Branch.
	- 2. Installation steps
		- 2.1 Hardware installation
			- 2.1.1 Supported board sS32K312EVB-Q172 (Evaluation board)
			- 2.1.2 Connections
			  PTB14 (J4.16) control LPSPI1 Clock, connect to strip 0 clock pin.
			  PTB16 (J4.10) control LPSPI1 Data, connect to strip 0 data pin.
			- 2.1.3 Debugger
			  The debugger must be connected to J12 10-pin JTAG Cortex Debug connector.
		- 2.2 Software installation
			- 2.2.1 Importing the S32 Design Studio project
			  After opening S32 Design Studio, go to "File -> New -> S32DS Project From Example" and select this example. Then click on "Finish".
			  The project should now be copied into you current workspace.
	- 3. Generating, building and running the example application
		- 3.1 Generating the S32 configuration
		  Before running the example a configuration needs to be generated.  First go to Project Explorer View in S32 DS and select the current project. Select the "S32 Configuration Tool" menu then click on the desired configuration tool (Pins, Cocks, Peripherals etc...). Clicking on any one of those will generate all the components. Make the desired changes(if any) then click on the menu "ConfigTools->Update Code".
		  In Update Files dialog, select files in Clocks and Peripherals only. Click OK.
		- 3.2 Compiling the application
			- Go to plugin folder Iseled_example_EBT_S32K312/SchM_Iseled, copy SchM_Iseled.c and SchM_Iseled.h and overwrite to RTD/src and RTD/include
			  Right click in Iseled_example_DS project, select Properties
			  Go to C/C++ Build/Settings.
			- In Tool Settings tab, Standard S32DS C Linker, Libraries, add iseled_S32K312_S32DS_DET_ON to Libraries list, or other Iseled library file depend on user need. Then add the folder contains Iseled library file to Library search path.
			  Select the configuration to be built: FLASH (Debug_FLASH) by left clicking on the downward arrow corresponding to the build button in eclipse.
			- Use Project > Build to build the project.
			  Wait for the build action to be completed before continuing to the next step. Check the compiler console for error messages; upon completion, the *.elf binary file
			  should be created.
		- 3.2 Running the application on the board
		  Go to Run and select Debug Configurations. There will be a debug configuration for this project:
		  
		  Configuration Name                  Description
		  -------------------------------     -----------------------
		  $(example)_debug_flash              Debug the FLASH configuration using S32Debugger
		  
		  Select the desired debug configuration and click on Launch. Now the perspective will change to the Debug Perspective.
		  Use the controls to control the program flow.
	-
- hotfix
	- ILaS_Set_Cur_Ch012 기본값이 2 or 3인듯 , max 15
-