.. include:: ../../../macros.rst



====
UART
====

.. highlight:: C

The |mod_uart| provides the capability to send and receive data using the RS232/UART interface.



Module Files
~~~~~~~~~~~~

Driver:
 - ``src\module\uart\uart.c``
 - ``src\module\uart\uart.h``

Driver Configuration:
 - ``src\module\config\uart\uart_cfg.c``
 - ``src\module\config\uart\uart_cfg.h``


Description of the |mod_uart|
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The |mod_uart| uses a user-defined buffer for transmitting data. A custom IRQ handler is responsible for handling all receive/transmit processes. Those operations are dispatched to sub-functions according to the set flags in the status register.

UART Configuration
~~~~~~~~~~~~~~~~~~

The default configuration is done within the ``UART_Init()`` function, which gets called during startup. Below is an example showing the UART configuration which uses usart3 with a baudrate of 115200Bd:

.. code-block:: C

    UART_HandleTypeDef uart_cfg[UART_NUMBER_OF_USED_UART_CHANNELS] = {
           {
                .Instance = USART3,
                .Init.BaudRate = 115200,
                .Init.WordLength = UART_WORDLENGTH_8B,
                .Init.StopBits = UART_STOPBITS_1,
                .Init.Parity = UART_PARITY_NONE,
                .Init.Mode = UART_MODE_TX_RX,
                .Init.HwFlowCtl = UART_HWCONTROL_NONE,
                .Init.OverSampling = UART_OVERSAMPLING_16,
           }
    };




Usage
~~~~~

The initialization has to be done during startup for subsequent function calls to succeed. ``UART_IntRx`` and ``UART_IntTx`` are interrupt driven and do not need to be called by the user. Assuming initialization was done properly, the user only needs to copy content from the internal ring buffer for reading purpose, or call ``UART_vWrite`` for writing purpose.

