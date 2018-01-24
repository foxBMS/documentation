.. include:: ../../macros.rst



.. _general_information_releases:

========
Releases
========

The following table describes the different versions of |foxbms| that were released.
The first line is the most recent one, the last one the oldest one.

+---------------+----------------------------------+----------------------------------------------------------------+------------+
| Documentation | Embedded Software                |                          |foxbms| Hardware                     | |foxConda| |
+               +----------+-----------+-----------+--------------+-----------------+-------------+-----------------+            +
|               | Primary  | Secondary | Common    | |BMS-Master| | |BMS-Interface| | |BMS-Slave| | |BMS-Extension| |            |
+===============+==========+===========+===========+==============+=================+=============+=================+============+
| 1.0.0         | 1.0.0 ;  | 1.0.0 ;   | 1.0.0 ;   | 1.0.2        | 1.0.1           |   1.0.1 ;   | 1.0.2           | 1.0.0      |
|               | 1.0.1    | 1.0.1     | 1.0.1 ;   |              |                 |   2.0.3 ;   |                 |            |
|               |          |           | 1.0.2     |              |                 |   2.1.0     |                 |            |
+---------------+----------+-----------+-----------+--------------+-----------------+-------------+-----------------+------------+
| 0.5.2         | 0.5.2    | 0.5.2     |           | 1.0.2        | 1.0.1           |   1.0.1 ;   | 1.0.2           | 0.5.0      |
|               |          |           |           |              |                 |   2.0.3 ;   |                 |            |
|               |          |           |           |              |                 |   2.1.0     |                 |            |
+---------------+----------+-----------+-----------+--------------+-----------------+-------------+-----------------+------------+
| 0.5.1         | 0.5.1    | 0.5.1     |           | 1.0.2        | 1.0.1           | 1.0.1       | 1.0.2           | 0.5.0      |
+---------------+----------+-----------+-----------+--------------+-----------------+-------------+-----------------+------------+
| 0.5.0         | 0.5.0    | 0.5.0     |           | 1.0.2        | 1.0.1           | 1.0.1       | 1.0.2           | 0.5.0      |
+---------------+----------+-----------+-----------+--------------+-----------------+-------------+-----------------+------------+
| 0.4.4         | 0.4.4    | 0.4.4     |           | 1.0.1        | 1.0.1           | 1.0.1       | 1.0.1           | 0.4.3      |
+---------------+----------+-----------+-----------+--------------+-----------------+-------------+-----------------+------------+
| 0.4.3         | 0.4.3    | 0.4.3     |           | 1.0.0        | 1.0.0           | 1.0.0       | 1.0.0           | 0.4.3      |
+---------------+----------+-----------+-----------+--------------+-----------------+-------------+-----------------+------------+
| 0.4.2         | 0.4.2    | 0.4.2     |           | 1.0.0        | 1.0.0           | 1.0.0       | 1.0.0           | 0.4.0      |
+---------------+----------+-----------+-----------+--------------+-----------------+-------------+-----------------+------------+
| 0.4.1         | 0.4.1    | 0.4.1     |           | 1.0.0        | 1.0.0           | 1.0.0       | 1.0.0           | 0.4.0      |
+---------------+----------+-----------+-----------+--------------+-----------------+-------------+-----------------+------------+
| 0.4.0         | 0.4.0    | 0.4.0     |           | 1.0.0        | 1.0.0           | 1.0.0       | 1.0.0           | 0.4.0      |
+---------------+----------+-----------+-----------+--------------+-----------------+-------------+-----------------+------------+

The following section summarizes the release notes for the different versions of the documentation.

Release 1.0.2 mcu-common
------------------------

Version 1.0.2 of mcu-common must be used with version 1.0.1 of mcu-primary and mcu-secondary.

Release notes:

The LTC driver was rewritten and now uses interrupts in addition to timings. With 8 modules or less, voltages are measured with a period not higher than 20ms,
which means voltages can be measured with a 50Hz frequency.

The LTC measurement cycle is not triggered from the |mod_meas| anymore, measuring is done automatically.

Write and read access to the external EEPROM on the slaves was implemented.

The function ``LTC_SetMUXChCommand()`` was corrected. In some cases, it did not switch the multiplexer inputs correctly.

There was an error in the LTC and CANSIGNAL module when less than 12 battery cells per module were used.  This lead to incorrect transmitted cell voltages on the CAN bus. This error was corrected.

Changelog:

- mcu-common/src/module/ltc.c: fixed function LTC_SetMUXChCommand()
- mcu-common/src/module/ltc.c: redesigned structure for automatic measurement and 50Hz voltage measurement
- mcu-common/src/module/ltc.c: LTC driver now uses interrupts
- mcu-common/src/module/ltc.c: implemented/improved access to slave features (IO port-expander, external temperature sensor, EEPROM)

Release 1.0.1 mcu-common
------------------------

Release notes:

The %PATH% to foxconda was wrong for the clean command in eclipse for the primary project.
The state of the interlock was correctly returned to the calling function, but not written back into the interlock state variable

Changelog:

- mcu-common repository: mcu-common/src/module/interlock.c: fixed the above mentioned bug
- tools repository: tools/eclipse/foxbmsfoxbms-eclipse-project.zip: fixed the above mentioned bug


Release 1.0.0
-------------

Release notes:

Based on all the feedback received during the last 2 years by |foxbms| partners and users, the embedded software and the computer software have been restructured to provide a clearer structure and allow enhanced flexibility.

A new repository structure has been implemented: common drivers for |primary| and |secondary| are now no longer defined separately but have a dedicated repository and directory.

The software modules ``bmsctrl`` and ``sysctrl`` have been renamed and redefined. Now the state machine implemented in the |mod_sys| is the first to start, before other state machines are started. The |mod_diag| handles error counting and their thresholds. The |mod_bms| implements the overall BMS application. Depending on the requests received per CAN and on the system state read via flags in the database, the |mod_bms| makes request to the |mod_contactor| and to the |mod_interlock|.

The graphical interface FrontDesk has been discontinued and is no longer supported. The reason for this is the lack of flexibility and its limitations, when compared to a development environment like Eclipse. As a replacement to FrontDesk, a fully configured Eclipse workspace is now provided and available to work with the |foxbms| source code. Flashing can be done directly from the Eclipse workspace. Further, debugging can be done from the Eclipse workspace by using for example the Segger J-Link debugger and plugin.

The |mod_bal| has been redesigned and uses a dedicated state machine. It is disabled by default to ensure that balancing will not start automatically during a measurement in the laboratory.

The CAN matrix was also completely redesigned to ensure that new CAN messages can be added easily. The multiplexed voltage and temperature messages have been separated in non-multiplexed messages, thus making them easier to be extended in specific battery configurations. The number of battery modules is no longer limited to 16: it is virtually limited by the number of CAN messages and CAN bus load. Further, a DBC file is now provided for the new CAN matrix.

|foxconda| 1.0.0 must be installed to work with the release 1.0.0 of |foxbms|. It can be downloaded from the Fraunhofer-IISB `server <https://iisb-foxbms.iisb.fraunhofer.de/foxbms/>`_ containing the |foxconda| installers.

Changelog:

- by default a current sensor must be connected, otherwise |foxBMS| will not start. Details on how to change this behavior can be found in :ref:`faq_current_sensor`.
- completely restructured embedded software architecture for enhanced modularity and flexibility
- optimized embedded software module structure for more comprehensive adaptions
- added a common repository for common primary and secondary drivers
- added an Eclipse workspace to replace the FrontDesk graphical user interface to increase the flexibility
- redesigned CAN matrix for easier addition of new CAN messages
- replacement of FrontDesk by an Eclipse workspace


Release 0.5.2
-------------

Release notes:

- A software bug in the LTC driver leading to a non-functional temperature sensing on the |foxBMS| |BMS-Slave| version 1.xx was fixed. The |BMS-Slave| version is configuration for the primary MCU in ``foxBMS-primary\src\module\config\ltc_cfg.h`` by the define SLAVE_BOARD_VERSION and for the secondary MCU in ``foxBMS-secondary\src\module\config\ltc_cfg.h`` by the define SLAVE_BOARD_VERSION.

 - Set SLAVE_BOARD_VERSION to ``1`` if version 1.xx of the |BMS-Slave| is used.
 - Set SLAVE_BOARD_VERSION to ``2`` if version 2.xx of the |BMS-Slave| is used. Version 2.xx is the default configuration from now on.

Changelog:

- foxBMS primary

  - fixed LTC temperature sensing bug

- foxBMS secondary

  - fixed LTC temperature sensing bug


Release 0.5.1
-------------

Release notes:

- Update from waf version 1.8.12 to version 1.9.13
- Rewrite of the non-volatile random-access memory driver on the primary MCU.
  It is now located in ``\foxBMS-primary\src\module\nvram``
- The bootstrap script of foxBMS-setup repository now supports self-updating
  by calling ``python bootstrap.py --update``. This will allow easier version
  updates in the future.

Changelog:

- foxBMS-setup

  - added parameter '-u', '--update' to bootstrap.py for updating the setup repository.

- foxBMS-primary

  - updates for waf 1.9.13 support
  - updated ``module\EEPROM`` and migrated to ``module\nvmram``
  - minor code adaptations and cleanup

- foxBMS-secondary

  - support for waf 1.9.13
  - minor code adaptations and cleanup

- foxbMS-tools

  - updated waf from version 1.8.12 to version 1.9.13


Release 0.5.0
-------------

A new project structure is now used by foxBMS. The documentation is no more contained in the embedded software sources and has its own repository. FreeRTOS and hal have their own repository, too. A central repository called foxBMS-setup is now used. It contains several scripts:

    - bootsrap.py gets all the repositories needed to work with foxBMS
    - build.py is used to compile binaries and to generate the documentation
    - clean.py is used to removed the generated binaries and documentation

Release notes:

    - New project structure
    - Embedded Software

        - Added support for external (SPI) EEPROM on the BMS-Master
        - Redesign of can and cansignal module to simplify the usage
        - Added support for triggered and cyclic current measurement of Isabellenhuette current sensor (IVT)
        - Current sensor now functions by default in non-triggered modus (no reprogramming needed for the sensor)

    - Sphinx Documentation:

        - Updated and restructured complete documentation
        - Restructured file and folder structure for the documentation
        - Added safety and risk analysis section
        - Cleaning up of non-used files in the documentation
        - Consistency check and correction of the naming and wording used
        - Addition of the source files (e.g., Microsoft Visio diagrams) used to generate the figures in the documentation
        - Reformatted the licenses text formatting (no changes in the licenses content)
        - Updated the battery junction box (BJB) section with up-to-date components and parameters

Release 0.4.4
-------------

Release notes:

    - Full update and correction of the documentation based on the feedback received
    - Consistency check and update of the wording and naming used in the hardware, software and documentation
    - Improved checksum process

        - faster implementation of checksum script
        - checksum script is called automatically after ``python tools/waf-1.8.12 configure build``
        - build command ``python tools/waf-1.8.12 configure build chksum`` NO longer supported

Release 0.4.3
-------------

Starting from this version, a checksum mechanism was implemented in |foxbms|. If the checksum is active and it is not computed correctly, it will prevent the flashed program from running. Details on deactivating the checksum can be found in the :ref:`software_documentation_faq`, in :ref:`faq_checksum`.

Release notes:

    - Important: Changed contactor configuration order in the software to match the labels on the front

        - Contactor 0: CONT_PLUS_MAIN
        - Contactor 1: CONT_PLUS_PRECHARGE
        - Contactor 2: CONT_MINUS_MAIN

    - Fixed an bug which could cause an unintended closing of the contactors after recovering from error mode
    - Increased stack size for the engine tasks to avoid stack overflow in some special conditions
    - Added a note in the documentation to indicate the necessity to send a periodic CAN message to the BMS
    - Fixed DLC of CAN message for the current sensor measurement
    - Added checksum verification for the flashed binaries
    - Updated linker script to allow integration of the checksum tool
    - Activated debug without JTAG interface via USB

Release 0.4.2
-------------

Release notes:

    - Removed schematic files from documentation, registration needed to obtain the files
    - Added entries to the software FAQ



Release 0.4.1
-------------

Release notes:

    - Corrected daisy chain connector pinout in quickstart guide
    - Corrected code  for contactors, to allow using contactors without feedback
    - Corrected LTC code for reading balancing feedback
    - Quickstart restructured, with mention of the necessity to generate the HTML documentation

Release 0.4.0
-------------

Beta version of |foxbms| that was supplied to selected partners for evaluation.
