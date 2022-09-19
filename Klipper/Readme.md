## Klipper:
<br>


<ul><li><details>
<summary><h3><a href="./TinyFan.cfg">TinyFan Example Config</a></h></summary>
</details></li></ul>


<ul><li><details>
<summary><h3>Board Aliases:</h></summary>
<pre>[mcu tinyfan]<br>serial: /dev/serial/by-id/usb-Klipper_rp2040_XXXXXXXXXXXX<br>restart_method: command<br><br>[board_pins tinyfan]<br>mcu: tinyfan<br>aliases:<br>	#FAN Ports:<br>	FAN1_PWM=gpio0, FAN1_RPM=gpio7,<br>	FAN2_PWM=gpio1, FAN2_RPM=gpio8,<br>	FAN3_PWM=gpio2, FAN3_RPM=gpio9,<br>	FAN4_PWM=gpio3, FAN4_RPM=gpio10,<br>	<br>	#AUX Ports:<br>	AUX1_PWM=gpio4, AUX1_RPM=gpio26,<br>	AUX2_PWM=gpio5, AUX2_RPM=gpio27,<br>	AUX3_PWM=gpio6, AUX3_RPM=gpio28,<br>	<br>	#GPIO Header:<br>	G1=gpio29, G2=gpio11, G3=gpio12,<br>	G4=gpio13, G5=gpio14, G6=gpio15,<br>	<br>	#Waveshare LED:<br>	LED=gpio16</pre>
</details></li></ul>


<ul><li>
<details>
 <summary><h3>Installation Steps:</h></summary>
 <li>ssh into your raspberry pi</li>
 <li>stop the klipper service <code>sudo systemctl stop klipper</code></li>
 <li>run <code>ls /dev</code> and keep the list open</li>
 <li>Connect the rp2040 to your raspberry while holding down the boot button</li>
 <li>run again <code>ls /dev</code> and look out for the added device, usually this is sda1</li>
 <li>chroot into your klipper directory <code>cd klipper</code></li>
 <li>cleanup all previous build files <code>make clean</code></li>
 <li>create a new build config <code>make menuconfig</code></li>
 <li>using the following settings</li>
 <p align="center"><img width="auto" src="../Images/klipper.png"></p>
 <li>run the build <code>make</code></li>
 <li>copy the firmware onto the rp2040</li>
 <pre>sudo mount /dev/sda1 /mnt<br>sudo cp out/klipper.uf2 /mnt<br>sudo umount /mnt</pre>
 <li>find your mcu address by running <code>ls /dev/serial/by-id/*</code></li>
 <li>add the new mcu to your printer.cfg</li>
 <pre>[mcu fans]<br>serial: /dev/serial/by-id/usb-Klipper_rp2040_XXXX</pre>
 <li>start the klipper service <code>sudo systemctl start klipper</code></li>
</details></li></ul>

<ul><li>
<details>
 <summary><h3>Alternative Installation:</h></summary>
 <li>ssh into your raspberry pi</li>
 <li>Connect the rp2040 to your raspberry while holding down the boot button</li>
 <li>stop the klipper service <code>sudo systemctl stop klipper</code></li>
 <li>chroot into your klipper directory <code>cd klipper</code></li>
 <li>cleanup all previous build files <code>make clean</code></li>
 <li>create a new build config <code>make menuconfig</code></li>
 <li>using the following settings</li>
 <p align="center"><img width="auto" src="../Images/klipper.png"></p>
 <li>run the build <code>make</code></li>
 <li>run <code>make flash FLASH_DEVICE="2e8a:0003"</code></li>
 <li>find your mcu address by running <code>ls /dev/serial/by-id/*</code></li>
 <li>add the new mcu to your printer.cfg</li>
 <pre>[mcu fans]<br>serial: /dev/serial/by-id/usb-Klipper_rp2040_XXXX</pre>
 <li>start the klipper service <code>sudo systemctl start klipper</code></li>
</details>
</li></ul>

<ul><li>
<details>
 <summary><h3>Outputs and Options:</h></summary>
 <details>
  <summary><h4>Fan1, Fan2, Fan3 and Fan4 are PWM Fans with RPM Output and selectable Voltage for 12 and 24V</h></summary>
  <pre>[fan_generic tinyfan_fan1]<br>pin: tinyfan:FAN1_PWM<br>tachometer_pin: ^tinyfan:FAN1_RPM<br>hardware_pwm: true<br>cycle_time: 0.00004<br><br>[fan_generic tinyfan_fan2]<br>pin: tinyfan:FAN2_PWM<br>tachometer_pin: ^tinyfan:FAN2_RPM<br>hardware_pwm: true<br>cycle_time: 0.00004<br><br>[fan_generic tinyfan_fan3]<br>pin: tinyfan:FAN3_PWM<br>tachometer_pin: ^tinyfan:FAN3_RPM<br>hardware_pwm: true<br>cycle_time: 0.00004<br><br>[fan_generic tinyfan_fan4]<br>pin: tinyfan:FAN4_PWM<br>tachometer_pin: ^tinyfan:FAN4_RPM<br>hardware_pwm: true<br>cycle_time: 0.00004</pre>
 </details>
 • Be aware that the 12V rail is only capable of a max of 1.5A<br>• Aux1, Aux2 and Aux3 are more versatile
 <details>
  <summary><h4>Adding more Thermistors</h></summary>
  <p align="center"><img width="auto" src="../Images/AUX1_THERMISTOR.png"></p>
  <pre>[temperature_sensor aux1]<br>sensor_type: Generic 3950<br>sensor_pin: tinyfan:AUX1_RPM</pre>
 </details>
 <details>
  <summary><h4>Driving more 5V, 12V and 24V Fans</h></summary>
  <p align="center"><img width="auto" src="../Images/AUX1_FAN_MOSFET.png"></p>
  <pre>[fan_generic tinyfan_aux1]<br>pin: tinyfan:AUX1_PWM<br>hardware_pwm: true<br>cycle_time: 0.00004<br><br>[fan_generic tinyfan_aux2]<br>pin: tinyfan:AUX2_PWM<br>hardware_pwm: true<br>cycle_time: 0.00004<br><br>[fan_generic tinyfan_aux2]<br>pin: tinyfan:AUX3_PWM<br>hardware_pwm: true<br>cycle_time: 0.00004</pre>
 </details>
 <details>
  <summary><h4>Driving 5V, 12V and 24V Leds</h></summary>
  <p align="center"><img width="auto" src="../Images/AUX1_LEDS.png"></p>
  <pre>[output_pin aux1_blue]<br>pin: tinyfan:AUX1_PWM<br>pwm: True<br>cycle_time: 0.0005<br>hardware_pwm: true<br><br>[output_pin aux2_red]<br>pin: tinyfan:AUX2_PWM<br>pwm: True<br>cycle_time: 0.0005<br>hardware_pwm: true<br><br>[output_pin aux3_green]<br>pin: tinyfan:AUX3_PWM<br>pwm: True<br>cycle_time: 0.0005<br>hardware_pwm: true</pre>
 </details>
 <details>
  <summary><h4>Driving Neopixels</h></summary>
  <p align="center"><img width="auto" src="../Images/AUX1_NEOPIXEL.png"></p>
  <pre>[neopixel fan_neopixels]<br>pin: tinyfan:AUX1_RPM<br>chain_count: 18</pre>
 </details>
</details>
</li></ul>
<ul><li><details>
 <summary><h3>Pinout and Options (Picturized):</h></summary>
 <p align="center"><img width="auto" src="../Images/TinyFan_Pinout.jpg"></p>
 <p align="center"><img width="auto" src="../Images/TinyFan_Options.jpg"></p>
</details></li></ul>