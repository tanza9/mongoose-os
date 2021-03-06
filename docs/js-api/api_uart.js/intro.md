---
title: "UART"
items:
---

UART API. Source C API is defined at:
[mgos_uart.h](https://github.com/cesanta/mongoose-os/blob/master/fw/src/mgos_uart.h)


## **`UART.setConfig(uartNo, param)`**
Set UART config. `param` is an
object with the following optional fields:

- `baudRate`: baud rate, integer, default: 115200;
- `rxBufSize`: size of the Rx buffer, integer, default: 256;
- `rxFlowControl`: whether Rx flow control (RTS pin) is enabled, boolean,
   default: false;
- `rxLingerMicros`: how many microseconds to linger after Rx fifo
  is empty, in case more data arrives. Integer, default: 15;
- `txBufSize`: size of the Tx buffer, integer, default: 256;
- `txFlowControl`: whether Tx flow control (CTS pin) is enabled, boolean,
  default: false;

Other than that, there are architecture-dependent settings, grouped in
the objects named with the architecture name: "esp32", "esp8266", etc.

Settings for esp32:

```
  esp32: {
     /*
      * GPIO pin numbers, default values depend on UART.
      *
      * UART 0: Rx: 3, Tx: 1, CTS: 19, RTS: 22
      * UART 1: Rx: 13, Tx: 14, CTS: 15, RTS: 16
      * UART 2: Rx: 17, Tx: 25, CTS: 26, RTS: 27
      */
     gpio: {
       rx: number,
       tx: number,
       cts: number,
       rts: number,
     },

     /* Hardware FIFO tweaks */
     fifo: {
       /*
        * A number of bytes in the hardware Rx fifo, should be between 1 and 127.
        * How full hardware Rx fifo should be before "rx fifo full" interrupt is
        * fired.
        */
       rxFullThresh: number,

       /*
        * A number of bytes in the hardware Rx fifo, should be more than
        * rx_fifo_full_thresh.
        *
        * How full hardware Rx fifo should be before CTS is deasserted, telling
        * the other side to stop sending data.
        */
       rxFcThresh: number,

       /*
        * Time in uart bit intervals when "rx fifo full" interrupt fires even if
        * it's not full enough
        */
       rxAlarm: number,

       /*
        * A number of bytes in the hardware Tx fifo, should be between 1 and 127.
        * When the number of bytes in Tx buffer becomes less than
        * tx_fifo_empty_thresh, "tx fifo empty" interrupt fires.
        */
       txEmptyThresh: number,
     },
   }
```

TODO: implement esp8266-specific settings



Apply arch-specific config



## **`UART.setDispatcher(uartNo, callback, userdata)`**
Set UART dispatcher
callback which gets invoked when there is a new data in the input buffer
or when the space becomes available on the output buffer.

Callback receives the following arguments: `(uartNo, userdata)`.



## **`UART.write(uartNo, data)`**
Write data to the buffer. Returns number of bytes written.

Example usage: `UART.write(1, "foobar")`, in this case, 6 bytes will be written.



## **`UART.writeAvail(uartNo)`**
Return amount of space available in the output buffer.



## **`UART.read(uartNo)`**
It never blocks, and returns a string containing
read data (which will be empty if there's no data available).



## **`UART.readAvail(uartNo)`**
Return amount of data available in the input buffer.



## **`UART.setRxEnabled(uartNo)`**
Set whether Rx is enabled.



## **`UART.isRxEnabled(uartNo)`**
Returns whether Rx is enabled.



## **`UART.flush(uartNo)`**
Flush the UART output buffer, wait for the data to be sent.



Load arch-specific API

