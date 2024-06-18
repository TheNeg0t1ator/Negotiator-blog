---
title : "PXL game tiles"
date : 2024-06-13
description : "PXL RGB game Tiles"

summary : "PXL RGB game Tiles"
---
{{< katex >}}
## Intro
The "2EAI-PEN-2324-Tiles" project is all about creating an exciting and interactive tile set. It consists of 36  tiles. This project blends technology and creativity to make gaming more fun and engaging.


Each tile consists of a pico, two pressure sensors, and rs485 communication.
In the previous research project of this, the pico's were programmed in rust,
with rather pleasing results, with a few flaws. 
when this engineering project started, we decided, for best coarse of action to program them in python.


## Code
Here i will go over the code that runs on the pico's
as RS485 only specifies the the io and the differential signaling,
the code for the project mainly consists of a protocol that has been especialy designed for the project.

it consists of a few lib files, that each contain classes.

### RS485.py

this contains two classes, one to store and use datapackets, the other to send and receive them

the datapacket class:
```python
class datapacket:
    channel: int
    cmd: int
    message: list[int]
    def __init__(self, channel, cmd, message):
        self.channel = channel
        self.cmd = cmd
        self.message = message
    
    def to_bytes(self):
        return bytes([self.channel, self.cmd]) + bytes(self.message)

    def __str__(self):
         return str("ch: " + str(self.channel) + " cmd: " + str(self.cmd) + " msg: " + str(self.message))
```

and the RS485 class, that uses `uart_TX` and `uart_RX` to send the data on the bus.
this only transmits `datapacket` and receives them. it does not have any logic.
this class is used int tileclass to facilitate easier communication

<!-- ```python
class CommRS485:
    master: bool = False
    uart_RX = machine.UART(0, baudrate=115200, tx=0, rx=1)
    uart_TX = machine.UART(1, baudrate=115200, tx=4, rx=5)
    selfAdress : int = 0
    
    def __str__(self) -> str:
        output:str= "Master: " + str(self.master) + "\n"
        output += "Adress: " + str(self.selfAdress) + "\n"
    
    
    class Command:
        SET_CHANNEL = 0x00
        SET_RGB_DATA = 0x01
        GET_PRESSURE_DATA = 0x02
        SET_RECEIVE_DATA = 0x03
        RESET_TILES = 0x04
        DEBUG = 0x05
        SEND_PRESSURE_DATA = 0x06
    class ReservedChannels:
        UNINITIALIZED = 0x00
        MASTER = 0xFE
        BROADCAST = 0xFF
    def setselfAdress(self, adress:int)->None:
        self.selfAdress = adress
    
    def Setmaster(self)->None:
        self.master = True
        self.selfAdress = 0xFE
    
    def SetSlave(self)->None:
        self.master = False
        
    def Send(self, packet:datapacket)->None:
        self.uart_TX.write(packet.to_bytes())
    
    def Receive(self):
        if self.uart_RX.any():
            received_message = self.uart_RX.read()  # Read the received message
            channel = received_message[0]  # Extract the channel byte
            
            # print("debug: " + str(self.selfAdress) + " " + str(channel)) ### self.selfAdress wrong adress
            
            if channel != self.selfAdress and channel != self.ReservedChannels.BROADCAST:
                return None
            cmd = received_message[1]  # Extract the cmd byte
            message = list(received_message[2:5])  # Convert the remaining bytes to a list of 3 bytes
            return datapacket(channel, cmd, message)
        else:
            return None
        
    def SendReceive(self, packet: datapacket)->datapacket:
        self.Send(packet)
        return self.Receive()
``` -->

### Tile.py
> not really correctly named this one 🤦🏻‍♂️

This lib contains the way the protocol acts when receiving and transmitting `datapacket`'s
it handles the addresses, it checks what type of command it is, and handles the commands
This clas is the same wether it is a `Slave` tile or a `Master` tile.
This also contains the `TileArray` class, this contains the way the tiles should act together.
it contains the list of current in use addresses, and the position of each.

<!-- TileClass:
```python

``` -->


## The protocol

### Command List

| Command | Description                                             | Arguments                             |
|---------|---------------------------------------------------------|---------------------------------------|
| 0x00    | Set Channel                                              | 1 byte (channel number)               |
| 0x01    | Set RGB data                                             | 3 bytes (red, green, blue values)     |
| 0x02    | Get pressure data                                        | None                                  |
| 0x03    | Set RGB and return pressure data                         | 3 bytes (red, green, blue values)     |
| 0x04    | Reset tiles                                              | None                                  |
| 0x05    | Debug                                                    | 1 byte (debug data)                   |
| 0x06    | Send/receive pressure data                               | None                                  |

### Preserved Channels

| Channel | Description                                              |
|---------|----------------------------------------------------------|
| 0x00    | Reserved uninitialized channel (for tiles without a defined channel) |
| 0xFE    | Reserved for master channel                              |
| 0xFF    | All tiles listen to this channel (broadcast channel)     |
## Demo's

these are some of the demonstrations we made

### Communication Demo

Here is a demonstration from early on in the project, this shows how the Tiles communicate via RS485.
<iframe width="560" height="315" src="https://www.youtube.com/embed/pm9GXSGA8R8?si=BbwNb-bkXP5wh0fR" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>




### 3x2 Demo

This is a demonstration from when the wooden array was fully done, and we could try a 3x2 demonstration
<iframe width="560" height="315" src="https://www.youtube.com/embed/-YoPWQ6t9J8?si=1ZBjjuikFaBvAncm" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>