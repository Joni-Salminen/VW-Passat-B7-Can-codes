# RNS-315 to MFD communication
Everything below is represented as hexadecimal numbers!


Table below repsesents message flow from RNS-315 -> MFD, when it writes "BLuetooth audio" on the screen.
All the texts are written with same schema to the "AUDIO" page of the display, for navigation and rest i believe its all the same, with the expection of different ID used when transmitting, but i have not looked into that too deeply yet.
| ID | DLC | Byte 7 | Byte 6 | Byte 5 | Byte 4 | Byte 3 | Byte 2 | Byte 1 | Byte 0
| ------ | ------ | ----- | ------ | ------ | ------ | ------ | ----- | ----- | ----- |
| 66C | 8 | 75 | 6C | 42 | 09 | 55 | 4C | 14 | 80
| 66C | 8 | 05 | 68 | 74 | 6F | 6F | 74 | 65 | C0
| 66C | 8 | 02 | 00 | 6F | 69 | 64 | 75 | 61 | C1
| 66C | 3 |    |    |    |    |    | FF | FF | C2

## First message
Bytes 0, 2 & 3 are always the same, something to do with the protocol i believe.

Byte 2, Payload lenght, 0x14 == 20 bytes payload, there are (8 * 3 + 3 == 27) bytes in the example thought, when we minus first byte of each message(those are control bytes aswell) and bytes 1, 2 & 3 from first message we get 20, which is the actual payload.

Byte 4, This is count of ascii characters before line break(This is how the MFD knows when to go on next line)

Bytes 5-7, these are payload bytes allready containing ascii characters "Blu" from 5 to 7

## Second Message
Byte 0 is the index of multi message.(C0)

Bytes 1-7 are payload bytes, "etooth" from 1 to 6

Byte 7, is the most interesting one, as it does indicate line break and that there is 5 ascii characters coming up

## Third Message 
Byte 0 is the index of multi message. (C1)

Bytes 1-7 are payload again

Byte 6, this is most likely padding byte

Byte 7, this is line break again and indication of 2 more characters coming(?)

## last message

Byte 0 is the index of multi message. (C3)
Byte 1 & 2 are always (FF) for "Bluetooth" related messages, might have something to do with the BT icon shown on the screen

#Some notes
Every bluetooth related audio message ends to FF FF 02, at the moment i assume it might make the screen show BT icon
