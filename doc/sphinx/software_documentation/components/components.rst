.. include:: ../../macros.rst



.. _software_documentation_components:

===================
Software Components
===================

.. highlight:: C

The |foxbms| embedded software is made out of the following components:

 - ``mcu-common``
 - ``mcu-freertos``
 - ``mcu-hal``
 - ``mcu-primary``
 - ``mcu-secondary``

mcu-common
==========

Contains drivers that are common to |MCU0| and |MCU1|. This means that any change made in the common directory affects both |MCU0| and |MCU1|. Parts of the drivers that need to be specific to |MCU0| or |MCU1| are put in ``mcu-primary`` or ``mcu-secondary``, with the suffix ``_ex``. This is for example the case for the ADC module, with the files ``adc_ex.c`` and ``adc_ex.h``.

engine
------

The ``engine`` directory contains all the core functions of the BMS.

+-------------+------------------------------------------------------------------------------------+
| Element     | Description                                                                        |
+=============+====================================================================================+
| database    | Implementation of the asynchronous data exchange                                   |
+-------------+------------------------------------------------------------------------------------+

module
------

The ``module`` directory contains all the software modules needed by the BMS.

+-------------+-------------------------------------------------------------------------------------------+
| Element     | Description                                                                               |
+=============+===========================================================================================+
| adc         | Driver for analog to digital converter, measurement of lithium backup battery voltage     |
+-------------+-------------------------------------------------------------------------------------------+
| can         | Driver to receive/transmit CAN message                                                    |
+-------------+-------------------------------------------------------------------------------------------+
| cansignal   | Definition of CAN messages and signals                                                    |
+-------------+-------------------------------------------------------------------------------------------+
| chksum      | Checksum algorithms for modulo 32-bit addition and CRC32                                  |
+-------------+-------------------------------------------------------------------------------------------+
| dma         | Configuration for Direct Memory Access (e.g. used for SPI Communication)                  |
+-------------+-------------------------------------------------------------------------------------------+
| interlock   | Driver for the interlock                                                                  |
+-------------+-------------------------------------------------------------------------------------------+
| io          | Driver and interfaces for I/O ports (control of output pins and read of input pins)       |
+-------------+-------------------------------------------------------------------------------------------+
| ltc         | Driver for battery cell monitoring IC                                                     |
+-------------+-------------------------------------------------------------------------------------------+
| mcu         | MCU-dependent low-level functions (specific registers, MCU timestamps)                    |
+-------------+-------------------------------------------------------------------------------------------+
| meas        | Uses ltc module to perform measurements                                                   |
+-------------+-------------------------------------------------------------------------------------------+
| rcc         | Configuration of the prescaler for the MCU clock system                                   |
+-------------+-------------------------------------------------------------------------------------------+
| rtc         | Real time clock driver, Control and Access of Backup SRAM Registers                       |
+-------------+-------------------------------------------------------------------------------------------+
| spi         | Driver for communication via Serial Peripheral Interface (SPI bus)                        |
+-------------+-------------------------------------------------------------------------------------------+
| uart        | Driver for serial communication (UART, RS232 , RS485)                                     |
+-------------+-------------------------------------------------------------------------------------------+
| utils       | Contains utilities like the LED blink driver                                              |
+-------------+-------------------------------------------------------------------------------------------+
| watchdog    | Driver for the watchdog timer                                                             |
+-------------+-------------------------------------------------------------------------------------------+


mcu-freertos
============

Contains the operating system software (FreeRTOS).

mcu-hal
=======

``mcu-hal`` contains the Hardware Abstraction Layer (HAL). It is used by the system but is provided by the MCU manufacturer, in this case ST-Microelectronics. It is used by |foxbms| but not part of |foxbms|.

+-----------------------+--------------------------------------------------------------------------------+
| Element               | Description                                                                    |
+=======================+================================================================================+
| CMSIS                 | Interface and configuration of CMSIS                                           |
+-----------------------+--------------------------------------------------------------------------------+
| STM32F4xx_HAL_Driver  | STM32F4xx family Hardware Abstraction Layer drivers                            |
+-----------------------+--------------------------------------------------------------------------------+

mcu-primary
===========

Contains the software specific to |MCU0|.

application
-----------

The ``application`` directory contains the user applications.

+-------------+-----------------------------------------------------------------------------------+
| Element     | Description                                                                       |
+=============+===================================================================================+
| bal         | Driver for balancing                                                              |
+-------------+-----------------------------------------------------------------------------------+
| bms         | Decision are taken here by the BMS (e.g., open contactors in case of a problem)   |
+-------------+-----------------------------------------------------------------------------------+
| com         | Serial port communication layer (for debug purposes)                              |
+-------------+-----------------------------------------------------------------------------------+
| config      | Contains the configuration for the user applications (e.g., task configuration)   |
+-------------+-----------------------------------------------------------------------------------+
| sox         | Coulomb-counter (current integrator) and State-of-Function calculator             |
+-------------+-----------------------------------------------------------------------------------+
| task        | User specific cyclic tasks (10ms and 100ms)                                       |
+-------------+-----------------------------------------------------------------------------------+

Application tasks should be used to call user-defined functions.

engine
------

The ``engine`` directory contains all the core functions of the BMS.

+-------------+------------------------------------------------------------------------------------+
| Element     | Description                                                                        |
+=============+====================================================================================+
| config      | Contains the configuration of engine components (e.g., task configuration)         |
+-------------+------------------------------------------------------------------------------------+
| diag        | With this software module, other modules can report problems                       |
+-------------+------------------------------------------------------------------------------------+
| sys         | System state machine, starts all other state machines                              |
+-------------+------------------------------------------------------------------------------------+
| task        | Cyclic engine tasks (1, 10 and 100ms) that call system related functions           |
+-------------+------------------------------------------------------------------------------------+

general
-------

The ``general`` directory contains the main function and configuration files.

+-------------+-----------------------------------------------------------------------------------+
| Element     | Description                                                                       |
+=============+===================================================================================+
| main.c      | Initialization of hardware modules, of interrupts and of the operating system     |
+-------------+-----------------------------------------------------------------------------------+
| config      | Contains the configuration for the system initialization:                         |
|             | configuration and interface functions to HAL and FreeRTOS,                        |
|             | global definitions, interrupt configurations and startup code                     |
+-------------+-----------------------------------------------------------------------------------+
| nvic.c      | interrupt initialization                                                          |
+-------------+-----------------------------------------------------------------------------------+
| includes    | Contains the standard types                                                       |
+-------------+-----------------------------------------------------------------------------------+
| version.c   | Sets version number                                                               |
+-------------+-----------------------------------------------------------------------------------+


module
------

The ``module`` directory contains all the software modules needed by the BMS.

+-------------+-------------------------------------------------------------------------------------------+
| Element     | Description                                                                               |
+=============+===========================================================================================+
| adc         | Driver for analog to digital converter, measurement of lithium backup battery voltage     |
+-------------+-------------------------------------------------------------------------------------------+
| config      | Contains the configuration for the software modules                                       |
+-------------+-------------------------------------------------------------------------------------------+
| contactor   | Driver to open/close contactors and read contactor feedback                               |
+-------------+-------------------------------------------------------------------------------------------+
| intermcu    | Driver for communication between |MCU0| (primary) and |MCU1| (secondary)                  |
+-------------+-------------------------------------------------------------------------------------------+
| isoguard    | Driver for monitoring galvanic isolation in the system                                    |
+-------------+-------------------------------------------------------------------------------------------+
| nvram       | Non-volatile Memory: Eeprom and button cell buffered SRAM (BKP_SRAM)                      |
+-------------+-------------------------------------------------------------------------------------------+
| timer       | Driver for MCU timer peripherals                                                          |
+-------------+-------------------------------------------------------------------------------------------+

os
--

The ``os`` directory contains configurations for the operating system FreeRTOS.

+-------------+----------------------------------------------------------------------------------------+
| Element     | Description                                                                            |
+=============+========================================================================================+
| os.c        | Interface to FreeRTOS (e.g., wrapper functions of cyclic application and engine tasks) | 
+-------------+----------------------------------------------------------------------------------------+


mcu-secondary
=============

Contains the software specific to |MCU1|. These are the same elements as ``mcu-primary``, adapted for |MCU1|.  
