![System](https://images-na.ssl-images-amazon.com/images/I/41LF%2BbP1QDL._SY355_.jpg)

The "Teufel CC 21 RC" was part of e.g. the "Teufel C200 USB" 2.1 speaker system.  
Lacking the abbility to attach an IR remote, it's difficult to use the system for e.g. in a living room situation. Reverse engineering the (wire-based) protocol may give the ability to build a microcontroller (using an Arduino or ESP8266) to control the speakers remotely using IR/WiFi.

## Connector Pinout

![Pinout][pinout]

## Protocol

Rx, TX lines are plain UART with 19200 baud rate. You can inspect the traffic by attaching a Arduino UART Converter and these [hterm  Settings][htermsettings] (see [this][1]). The remote and subwoofer are communicating over plain 8 bit messages, following the protocol:

| Byte (dec) | Description | Direction |
|------------|-------------|-----------|
| 161        | Turn on     | <-->      |
| 177        | Turn off    | <-->      |
| 162        | Volume +    | -->       |
| 163        | Volume -    | -->       |
| 164        | Bass +      | -->       |
| 165        | Bass -      | -->       |

The volume +/- messages are only working in one direction (from the remote to the subwoofer), so it's impossible to implement a "man in the middle"-microcontroller.  
If you send a `162` to the remote, the LED indicator will *not* change.

There are 28 Volume levels, that I'll call `0-28` of which `0` turns off the power amplifier (you will hear a relay clicking, when the sub reaches this level) and +/- 5 Bass levels, `-5 -4 -3 -2 -1 0 1 2 3 4 5`. After plugging in the remote it starts at volume level `8` and bass level `0`.


[htermsettings]: /docs/hterm.png
[pinout]: /docs/Pinout.png
[1]: /docs/Debug-Circuit.jpg