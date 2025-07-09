# PSoC&trade; 4: CAPSENSE&trade; Pipeline scan and process

This code example demonstrates how to implement pipeline scanning in CAPSENSE&trade; to improve performance. When a particular widget is being processed, the next widget in line can be scanned in parallel. This is better than the traditional method of scanning all the widgets and then processing the scanned data later. 

## Requirements

- [ModusToolbox&trade;](https://www.infineon.com/modustoolbox) v3.5 or later (tested with v3.5)

- Board support package (BSP) minimum required version: 3.1.0
- Programming language: C
- Associated parts: [PSoC&trade; 4 HV (High Voltage)](https://www.infineon.com/cms/en/product/microcontroller/32-bit-psoc-arm-cortex-microcontroller/32-bit-psoc-4-hv-arm-cortex-m0/)

## Supported toolchains (make variable 'TOOLCHAIN')

- GNU Arm&reg; Embedded Compiler v11.3.1 (`GCC_ARM`) – Default value of `TOOLCHAIN`
- Arm&reg; compiler v6.22 (`ARM`)
- IAR C/C++ compiler v9.50.2 (`IAR`)

## Supported kits (make variable 'TARGET')

- [PSoC&trade; 4 HVMS-64K Evaluation Kit](https://www.infineon.com/KIT_PSoC4-HVMS-64K_LITE) (`KIT_PSoC4-HVMS-64K_LITE`) - Default value of `TARGET`
- [PSoC&trade; 4 HVMS-128K Evaluation Kit](https://www.infineon.com/KIT_PSoC4-HVMS-128K_LITE) (`KIT_PSoC4-HVMS-128K_LITE`)

## Hardware setup

This example uses the board's default configuration. See the kit user guide to ensure that the board is configured correctly.

> **Note:** Some of the PSoC&trade; 4 kits ship with KitProg2 installed. ModusToolbox&trade; requires KitProg3. Before using this code example, make sure that the board is upgraded to KitProg3. The tool and instructions are available in the [Firmware Loader](https://github.com/Infineon/Firmware-loader) GitHub repository. If you do not upgrade, you will see an error like "unable to find CMSIS-DAP device" or "KitProg firmware is out of date".

## Software setup

See the [ModusToolbox&trade; tools package installation guide](https://www.infineon.com/ModusToolboxInstallguide) for information about installing and configuring the tools package.

This example requires no additional software or tools.



## Using the code example

### Create the project

The ModusToolbox&trade; tools package provides the Project Creator as both a GUI tool and a command line tool.

<details><summary><b>Use Project Creator GUI</b></summary>

1. Open the Project Creator GUI tool.

   There are several ways to do this, including launching it from the dashboard or from inside the Eclipse IDE. For more details, see the [Project Creator user guide](https://www.infineon.com/ModusToolboxProjectCreator) (locally available at *{ModusToolbox&trade; install directory}/tools_{version}/project-creator/docs/project-creator.pdf*).

2. On the **Choose Board Support Package (BSP)** page, select a kit supported by this code example. See [Supported kits](#supported-kits-make-variable-target).

   > **Note:** To use this code example for a kit not listed here, you may need to update the source files. If the kit does not have the required resources, the application may not work.

3. On the **Select Application** page:

   a. Select the **Applications(s) Root Path** and the **Target IDE**.

   > **Note:** Depending on how you open the Project Creator tool, these fields may be pre-selected for you.

   b. Select this code example from the list by enabling its check box.

   > **Note:** You can narrow the list of displayed examples by typing in the filter box.

   c. (Optional) Change the suggested **New Application Name** and **New BSP Name**.

   d. Click **Create** to complete the application creation process.

</details>

<details><summary><b>Use Project Creator CLI</b></summary>

The 'project-creator-cli' tool can be used to create applications from a CLI terminal or from within batch files or shell scripts. This tool is available in the *{ModusToolbox&trade; install directory}/tools_{version}/project-creator/* directory.

Use a CLI terminal to invoke the 'project-creator-cli' tool. On Windows, use the command-line 'modus-shell' program provided in the ModusToolbox&trade; installation instead of a standard Windows command-line application. This shell provides access to all ModusToolbox&trade; tools. You can access it by typing "modus-shell" in the search box in the Windows menu. In Linux and macOS, you can use any terminal application.


The following example clones the "[PSoC&trade; 4: CAPSENSE&trade; Pipeline scan and process](https://github.com/Infineon/mtb-hv-example-psoc4-capsense-pipeline-scan-and-process)" application with the desired name "PipelineScan" configured for the [KIT_PSoC4-HVMS-64K_LITE] BSP into the specified working directory, *C:/mtb_projects*:

   ```
   project-creator-cli --board-id KIT_PSoC4-HVMS-64K_LITE --app-id mtb-hv-example-psoc4-capsense-pipeline-scan-and-process --user-app-name PipelineScan --target-dir "C:/mtb_projects"
   ```
The 'project-creator-cli' tool has the following arguments:

Argument | Description | Required/optional
---------|-------------|-----------
`--board-id` | Defined in the <id> field of the [BSP](https://github.com/Infineon?q=bsp-manifest&type=&language=&sort=) manifest | Required
`--app-id`   | Defined in the <id> field of the [CE](https://github.com/Infineon?q=ce-manifest&type=&language=&sort=) manifest | Required
`--target-dir`| Specify the directory in which the application is to be created if you prefer not to use the default current working directory | Optional
`--user-app-name`| Specify the name of the application if you prefer to have a name other than the example's default name | Optional

> **Note:** The project-creator-cli tool uses the `git clone` and `make getlibs` commands to fetch the repository and import the required libraries. For details, see the "Project creator tools" section of the [ModusToolbox&trade; tools package user guide](https://www.infineon.com/ModusToolboxUserGuide) (locally available at {ModusToolbox&trade; install directory}/docs_{version}/mtb_user_guide.pdf).

</details>

### Open the project

After the project has been created, you can open it in your preferred development environment.

<details><summary><b>Eclipse IDE</b></summary>

If you opened the Project Creator tool from the included Eclipse IDE, the project will open in Eclipse automatically.

For more details, see the [Eclipse IDE for ModusToolbox&trade; user guide](https://www.infineon.com/MTBEclipseIDEUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_ide_user_guide.pdf*).

</details>

<details><summary><b>Visual Studio (VS) Code</b></summary>

Launch VS Code manually, and then open the generated *{project-name}.code-workspace* file located in the project directory.

For more details, see the [Visual Studio Code for ModusToolbox&trade; user guide](https://www.infineon.com/MTBVSCodeUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_vscode_user_guide.pdf*).

</details>

<details><summary><b>Keil µVision</b></summary>

Double-click the generated *{project-name}.cprj* file to launch the Keil µVision IDE.

For more details, see the [Keil µVision for ModusToolbox&trade; user guide](https://www.infineon.com/MTBuVisionUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_uvision_user_guide.pdf*).

</details>

<details><summary><b>IAR Embedded Workbench</b></summary>

Open IAR Embedded Workbench manually, and create a new project. Then select the generated *{project-name}.ipcf* file located in the project directory.

For more details, see the [IAR Embedded Workbench for ModusToolbox&trade; user guide](https://www.infineon.com/MTBIARUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_iar_user_guide.pdf*).

</details>

<details><summary><b>Command line</b></summary>

If you prefer to use the CLI, open the appropriate terminal, and navigate to the project directory. On Windows, use the command-line 'modus-shell' program; on Linux and macOS, you can use any terminal application. From there, you can run various `make` commands.

For more details, see the [ModusToolbox&trade; tools package user guide](https://www.infineon.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mtb_user_guide.pdf*).

</details>


## Operation

1. Connect the board to your PC using the provided USB cable through the KitProg3 USB connector.

2. Program the board using one of the following:

   <details><summary><b>Using Eclipse IDE </b></summary>

      1. Select the application project in the Project Explorer.

      2. In the **Quick Panel**, scroll down, and click **\<Application Name> Program (KitProg3_MiniProg4)**.
  
   </details>

   <details><summary><b>In other IDEs</b></summary>

   Follow the instructions in your preferred IDE.
   
   </details>
   
   <details><summary><b>Using CLI</b></summary>

     From the terminal, execute the `make program` command to build and program the application using the default toolchain to the default target. The default toolchain is specified in the application's Makefile, but you can override this value manually:
      ```
      make program TOOLCHAIN=<toolchain>
      ```

      Example:
      ```
      make program TOOLCHAIN=GCC_ARM
      ```
   </details>

3. After programming, the application starts automatically. The status of the on-board user buttons can be visually observed through their corresponding on-board user LEDs. By default or during a no-press condition, LEDs are turned OFF. A button press or slider segment press is visually indicated by turning ON the corresponding LED.

4. You can implement the same design process in your application. The higher the number of sensors in the application, the more noticeable and faster the response.

### Monitor data using CAPSENSE&trade; Tuner

1. Launch the CAPSENSE&trade; tuner from the 'BSP Configurators' section in the IDE **Quick Panel** to monitor the CAPSENSE&trade; data.

2. Ensure that the kits are in CMSIS-DAP Bulk mode (KitProg3 Status LED is ON and not blinking). See [Firmware Loader](https://github.com/Infineon/Firmware-loader) to learn how to update the firmware and switch modes in KitProg3.

3. In the tuner application, click on the **Tuner communication setup** icon or select **Tools** > **Tuner Communication Setup**. In the window that appears, select the I2C checkbox under KitProg3 and configure it as follows:

- **UART baudrate: 115200**

These are the same values set in the UART resource.

**Figure 1. Tuner communication setup parameters**

![](images/tuner-comm-setup.jpg)

4. Click **Connect** or select **Communication** > **Connect** to establish a connection.

   **Figure 2. Establish connection**
   
   ![](images/establish-connection.png)

5. Click **Start** or select **Communication** > **Start** to start data streaming from the device.
 
   **Figure 3. Start tuner communication**
  
   ![](images/start-tuner-communication.png)

6. Set the **Read mode** to Synchronized mode. Navigate to the **Widget view** tab and notice that the **Button0** widget is highlighted in blue when you touch it.

   **Figure 4. Widget View tab of the CAPSENSE&trade; Tuner**
  
   ![](images/tuner-widget-view.png)

7. The Tuner GUI displays the data from the sensor in the **Widget View** and **Graph View** tabs.

## Scan architecture

Pipeline scanning is a technique to scan and process widgets in parallel rather than sequentially scanning all the sensors/widgets and then processing them altogether. A faster method is when the CAPSENSE&trade; controller scans a particular widget while the previous widget is processed by the processor and the device is put into Deep Sleep until the scan is complete. The WDT (Watchdog Timer) is used to wake up the device periodically from Deep Sleep. This reduces the total effective scan time and minimizes the power consumption of the application.

A comparison between the two scanning methods is shown in **Figure 5**.

**Figure 5. Scan flow for pipeline and sequential scan methods**

![](images/scan_architecture.png)

## Debugging

You can debug the example to step through the code.

<details><summary><b>In Eclipse IDE</b></summary>

Use the **\<Application Name> Debug (KitProg3_MiniProg4)** configuration in the **Quick Panel**. For details, see the "Program and debug" section in the [Eclipse IDE for ModusToolbox&trade; user guide](https://www.infineon.com/MTBEclipseIDEUserGuide).

</details>

<details><summary><b>In other IDEs</b></summary>

Follow the instructions in your preferred IDE.

</details>

**Note:** For all kits, the debug port is by default disabled in order to achieve low power. If debugging is required, do the following:

1. Enable the **Debug mode** under the **Systems** tab in the **Device Configurator** and change the **Debug mode** setting to **SWD**.

2. Select the pins P3[2] (SWDIO) and P3[3] (SWDCK) under the **SWD pin** setting.

3. Select the Drive mode of both the pins as "Strong Drive,Input buffer on" under the **Pins** tab in the **Device Configurator**.

## Design and implementation

The project contains multiple button widgets is configured in CSX sensing mode. See the "CAPSENSE&trade; CSX sensing method" section in the [AN85951 – PSoC&trade; 4 and PSoC&trade; 6 MCU CAPSENSE&trade; design guide](https://www.infineon.com/AN85951)  for details on CAPSENSE&trade; CSD and CSX sensing modes. 

The sensing elements are mapped to the on-board user buttons. The status of an on-board user button is conveyed by controlling the LED state. The LED turns ON when a button press is registered and remains OFF when the button is not pressed.

Refer [PSoC&trade; 4 MCU: CAPSENSE&trade; CSX button tuning](https://github.com/Infineon/mtb-example-psoc4-capsense-csd-button-tuning) code example to tune CSX sensors.

The WDT in PSoC&trade; 4 is a 16-bit timer and uses the internal low-speed oscillator (ILO) clock of 40 kHz as a source. The accuracy of ILO is (- 50% to +100%). Therefore, the match value of WDT is set after compensating the ILO with IMO. The firmware flow is as follows:

1. Enable the ILO, which is the source for the WDT. Start ILO measurement and get the value of `ilo_compensated_counts` which must be set after every interrupt match.

2. Write the match value. The WDT can generate an interrupt when the WDT counter reaches the match count. The match count is generated using `DESIRED_WDT_INTERVAL`.

3. Enable interrupt generation and assign the interrupt service routine(`wdt_isr`).

4. Enable the WDT. Because the ILO has low accuracy, the `ilo_compensated_counts` are calculated, and the match value of the WDT is updated following a WDT interrupt.

5. The System is put into Deep Sleep in idle mode to save power. Because the watchdog timer works on a low-frequency clock (LFCLK), its operation will not be affected when the system is put into Deep Sleep mode. The watchdog timer interrupt will wake the device from Deep Sleep mode.

**Note:**

1. The WDT is configured to generate interrupts at `WDT_INTERRUPT_INTERVAL_MS` intervals. The default value of `WDT_INTERRUPT_INTERVAL_MS` is 10 ms. The WDT generates an interrupt on reaching the match value. The WDT counter is not reset on a match; it continues to count across the full 16-bit resolution. Therefore, the new match value of the WDT counter is generated and updated on every WDT interrupt event to generate an interrupt after the present interrupt. The WDT interrupt flag is set inside the WDT interrupt service routine; it is checked in the main loop.

2. You can enable the WDT using device configurator as shown in **Figure 6**.

3. You can configure the values of the WDT interrupt interval and WDT interrupt priority using macros in main.c as shown in **Figure 7** and update them.

**Figure 6. WDT settings**

![](images/wdt-settings.png)

**Figure 7. Accessing macros**

![](images/accessing-macros.png)

### Set up the VDDA supply voltage and Debug mode in the Device Configurator

1. Open the Device Configurator from the **Quick Panel**.

2. Navigate to the **System** tab. Select the **Power** resource and set the VDDA value under **Operating conditions**.

3. By default, SWD pins are active in all device power modes. Disable **Debug mode** to disable SWD pins and thereby reduce power consumption as follows:

**Figure 8. Disable Debug mode in the System tab of Device Configurator**
   
![](images/debug-disable.png)

<br>

### Resources and settings
<br>

**Figure 9. UART settings**

![](images/uart-config.png)

</details>

<br>

## Related resources

| Resources  | Links |
| ---------- | ----------- |
Application notes  | [AN0034](https://www.infineon.com/dgdl?fileId=8ac78c8c93dda25b01954cc962534907) – Getting started with PSOC&trade; 4 HV MS MCUs in ModusToolbox&trade;<br>  [AN85951](https://www.infineon.com/AN85951) – PSoC&trade; 4 and PSoC&trade; 6 MCU CAPSENSE&trade; design guide
Code examples  | [Using ModusToolbox&trade;](https://github.com/Infineon/Code-Examples-for-ModusToolbox-Software) on GitHub
Device documentation | [PSOC&trade; high voltage (HV) mixed signal (MS) automotive MCU 128K datasheets](https://www.infineon.com/dgdl/?fileId=8ac78c8c956a0a47019581095cec5cf6) <br>[PSOC&trade; high voltage (HV) mixed signal (MS) automotive MCU 64K datasheets](https://www.infineon.com/dgdl/?fileId=8ac78c8c956a0a470195817712a75d7a) <br>[PSOC&trade; high voltage (HV) mixed signal (MS) MCU: PSOC&trade; HVMS-128K registers reference manuals](https://www.infineon.com/dgdl/?fileId=8ac78c8c95650102019567b74fb62a38) <br>[PSOC&trade; high voltage (HV) mixed signal (MS) MCU: PSOC&trade; HVMS-64K registers reference manuals](https://www.infineon.com/dgdl/?fileId=8ac78c8c93dda25b019562232806264b&da=t) <br>[PSOC&trade; high voltage (HV) mixed signal (MS) MCU architecture reference manuals](https://www.infineon.com/dgdl/?fileId=8ac78c8c93dda25b0195297d34bf3ee6&da=t)
Development kits | Select your kits from the [Evaluation board finder](https://www.infineon.com/cms/en/design-support/finder-selection-tools/product-finder/evaluation-board) page.
Libraries on GitHub | [mtb-pdl-cat2](https://github.com/Infineon/mtb-pdl-cat2) – PSoC&trade; 4 Peripheral Driver Library (PDL)<br> [mtb-hal-cat2](https://github.com/Infineon/mtb-hal-cat2) – Hardware Abstraction Layer (HAL) library
Middleware on GitHub  | [capsense](https://github.com/Infineon/capsense) – CAPSENSE&trade; library and documents <br> [psoc4-middleware](https://github.com/Infineon/modustoolbox-software#libraries) – Links to all PSoC&trade; 4 MCU middleware
Tools  | [ModusToolbox&trade;](https://www.infineon.com/modustoolbox) – ModusToolbox&trade; software is a collection of easy-to-use libraries and tools enabling rapid development with Infineon MCUs for applications ranging from wireless and cloud-connected systems, edge AI/ML, embedded sense and control, to wired USB connectivity using PSoC&trade; Industrial/IoT MCUs, AIROC&trade; Wi-Fi and Bluetooth&reg; connectivity devices, XMC&trade; Industrial MCUs, and EZ-USB&trade;/EZ-PD&trade; wired connectivity controllers. ModusToolbox&trade; incorporates a comprehensive set of BSPs, HAL, libraries, configuration tools, and provides support for industry-standard IDEs to fast-track your embedded application development.

<br>


## Other resources

Infineon provides a wealth of data at [www.infineon.com](https://www.infineon.com) to help you select the right device, and quickly and effectively integrate it into your design.

## Document history

Document title: *CE237533 - PSoC&trade; 4: CAPSENSE&trade; Pipeline scan and process*

| Version | Description of change      |
| ------- | ---------------------      |
| 1.0.0   | New code example |
| 2.0.0   | Updated wdt function |
| 2.1.0   | Changed PSoC HV MS kit name to PSOC4 |
| 2.2.0   | Added support for KIT_PSOC4-HVMS-128K_LITE-02 and KIT_PSOC4-HVMS-64K_LITE-02 |
<br>

---------------------------------------------------------

© Cypress Semiconductor Corporation, 2024-2025. This document is the property of Cypress Semiconductor Corporation, an Infineon Technologies company, and its affiliates ("Cypress").  This document, including any software or firmware included or referenced in this document ("Software"), is owned by Cypress under the intellectual property laws and treaties of the United States and other countries worldwide.  Cypress reserves all rights under such laws and treaties and does not, except as specifically stated in this paragraph, grant any license under its patents, copyrights, trademarks, or other intellectual property rights.  If the Software is not accompanied by a license agreement and you do not otherwise have a written agreement with Cypress governing the use of the Software, then Cypress hereby grants you a personal, non-exclusive, nontransferable license (without the right to sublicense) (1) under its copyright rights in the Software (a) for Software provided in source code form, to modify and reproduce the Software solely for use with Cypress hardware products, only internally within your organization, and (b) to distribute the Software in binary code form externally to end users (either directly or indirectly through resellers and distributors), solely for use on Cypress hardware product units, and (2) under those claims of Cypress's patents that are infringed by the Software (as provided by Cypress, unmodified) to make, use, distribute, and import the Software solely for use with Cypress hardware products.  Any other use, reproduction, modification, translation, or compilation of the Software is prohibited.
<br>
TO THE EXTENT PERMITTED BY APPLICABLE LAW, CYPRESS MAKES NO WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, WITH REGARD TO THIS DOCUMENT OR ANY SOFTWARE OR ACCOMPANYING HARDWARE, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE.  No computing device can be absolutely secure.  Therefore, despite security measures implemented in Cypress hardware or software products, Cypress shall have no liability arising out of any security breach, such as unauthorized access to or use of a Cypress product. CYPRESS DOES NOT REPRESENT, WARRANT, OR GUARANTEE THAT CYPRESS PRODUCTS, OR SYSTEMS CREATED USING CYPRESS PRODUCTS, WILL BE FREE FROM CORRUPTION, ATTACK, VIRUSES, INTERFERENCE, HACKING, DATA LOSS OR THEFT, OR OTHER SECURITY INTRUSION (collectively, "Security Breach").  Cypress disclaims any liability relating to any Security Breach, and you shall and hereby do release Cypress from any claim, damage, or other liability arising from any Security Breach.  In addition, the products described in these materials may contain design defects or errors known as errata which may cause the product to deviate from published specifications. To the extent permitted by applicable law, Cypress reserves the right to make changes to this document without further notice. Cypress does not assume any liability arising out of the application or use of any product or circuit described in this document. Any information provided in this document, including any sample design information or programming code, is provided only for reference purposes.  It is the responsibility of the user of this document to properly design, program, and test the functionality and safety of any application made of this information and any resulting product.  "High-Risk Device" means any device or system whose failure could cause personal injury, death, or property damage.  Examples of High-Risk Devices are weapons, nuclear installations, surgical implants, and other medical devices.  "Critical Component" means any component of a High-Risk Device whose failure to perform can be reasonably expected to cause, directly or indirectly, the failure of the High-Risk Device, or to affect its safety or effectiveness.  Cypress is not liable, in whole or in part, and you shall and hereby do release Cypress from any claim, damage, or other liability arising from any use of a Cypress product as a Critical Component in a High-Risk Device. You shall indemnify and hold Cypress, including its affiliates, and its directors, officers, employees, agents, distributors, and assigns harmless from and against all claims, costs, damages, and expenses, arising out of any claim, including claims for product liability, personal injury or death, or property damage arising from any use of a Cypress product as a Critical Component in a High-Risk Device. Cypress products are not intended or authorized for use as a Critical Component in any High-Risk Device except to the limited extent that (i) Cypress's published data sheet for the product explicitly states Cypress has qualified the product for use in a specific High-Risk Device, or (ii) Cypress has given you advance written authorization to use the product as a Critical Component in the specific High-Risk Device and you have signed a separate indemnification agreement.
<br>
Cypress, the Cypress logo, and combinations thereof, ModusToolbox, PSoC, CAPSENSE, EZ-USB, F-RAM, and TRAVEO are trademarks or registered trademarks of Cypress or a subsidiary of Cypress in the United States or in other countries. For a more complete list of Cypress trademarks, visit www.infineon.com. Other names and brands may be claimed as property of their respective owners.

