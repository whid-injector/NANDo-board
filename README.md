
# NANDo-Board
A Multipurpose Breakout for the FT2232HL to easily conduct Hardware Security tests and Hack (I)IoT devices! 

<a href='https://ko-fi.com/X7X6L82L' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi4.png?v=0' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

![image](https://user-images.githubusercontent.com/26245612/168490116-b03b252d-4717-4295-81d7-af8a828ac3a6.png)

 
# Bill-of-Materials
Please check the BOM.txt for the components needed. Most likely (except the R470OHM 2010) you may have all you need already.<br>
Of course, you still need to purchase a part the FT2232HL CJMCU (e.g. https://s.click.aliexpress.com/e/_DeK6Oor )

# Tips:<br>
- This breakout is designed for the FT2232HL CJMCU board (e.g. https://s.click.aliexpress.com/e/_DeK6Oor )<br>
- Pull-up Resistor is 470Ohm.<br>
- Screws to hold the PCB to the 3D-printed case are 2x6mm.<br>
- BE SURE to mount the FT2232HL with the FTDI chipset FACING UP!!!

## UART (Channel A)
Command to run the UART console feature:
Configure minicom/putty/whatever-terminal-you-are-used-to (e.g.<br> ```screen /dev/ttyUSB0 115200```<br>
```screen -L /dev/ttyUSB0 115200```<br>
```screen -L -Logfile UART.log /dev/ttyUSB0 115200```)

## JTAG (Channel B)
Command to run the JTAG debugging feature:<br>
```sudo openocd -f NANDo-Board_JTAG_OpenOCD.cfg  -f target_device.cfg```

## SWD (Channel B) (this works on openocd v.0.11 but NOT on v.0.10!!!)
Command to run the SWD debugging feature (remember to move the SWD Enable switch on the PCB before using this feature!):

```cd openocd-v.0.11```<br>
```./openocd -s /opt/openocd/share/openocd/scripts/ -f /home/FOO/Desktop/NANDo-Board/OpenOCD_Configs/NANDo-Board_SWD_OpenOCD.cfg -f target_device.cfg```<br>

## SPI Dumping (Channel B)
Command to run the SPI dumping feature:<br>
```flashrom -p ft2232_spi:type=2232H,port=B -r firmware.bin```

In case you need also to write a SPI flash... please do enable the WRITE PROTECT (WP) Jumper on the PCB (i.e. SPI WP Enable).

## NAND Dumping
Dump Raw Image:<br>
```yand_cli.py -r -f nand_raw_dump_withOOB.bin```

Remove OOB Data:<br>
```python Nand-dump-tool.py -i nand_raw_dump_withOOB.bin  -o nand_raw_dump_cleaned.bin --page-size 2048 --oob-size 64 --layout separate```<br>
OR <br>
```python Nand-dump-tool.py -i nand_raw_dump_withOOB.bin --layout=guess -I <ID-CODE-HERE> -o nand_raw_dump_cleaned.bin```<br>

## Logic Analyzer with Pulseview
The strip pins AD0-AD7 labeled "Pulseview FTDI-LA" can be used as low-frequency Logic Analyzer Channels with Pulseview/Sigrok tool.
Here the configuration <ADD IMAGE FROM images DIR>

## Multipurpose Pin Headers/Sockets
On the lower part of NANDo-Board's PCB there are some pin headers/sockets that are not connected with the FT2232HL. They are there just in case you need to mess-up with many flying-wires and you want to keep all connections clean and in order like with an usual breadboard, but with screwdown terminal blocks & co. 
