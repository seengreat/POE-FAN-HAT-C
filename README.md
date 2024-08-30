POE-FAN-HAT-C from seengreat:www.seengreat.com
 =======================================
# Instructions
## 1.1、Overview
The POE FAN HAT C is a Power over Ethernet (PoE) supply and cooling solution designed for the Raspberry Pi 5. This board supports the IEEE 802.3af/at network standards. When used with a PoE router or switch that supports IEEE 802.3af/at network standards, only one Ethernet cable is needed for both networking and power supply to the Raspberry Pi.<br>
The board also provides an adjustable speed fan and metal heat sink, ensuring the Raspberry Pi 5 operates within a reasonable temperature range. The onboard high-speed adjustable active cooling fan seamlessly connects to the Raspberry Pi 5's 4-pin JST connector, while the included metal heat sink enhances heat dissipation.
Designed based on the Raspberry Pi 40-pin expansion interface, the board is user-friendly and can be directly plugged into the Raspberry Pi's 40-pin expansion interface.<br>
## 1.2Product Specifications
|Dimensions	|65mm (Length) * 56mm (Width)|
|----------------------|------------------------------------|
|Input Voltage	|PoE power input (37V ~ 57V)|
|Output Voltage	|5V|
|Maximum Output Power	|22.5W (5V/4.5A), can reach up to 5V/4.85A in extreme conditions|
|Network Standard	 |IEEE 802.3af/at PoE|
|Fan Speed (Max)	|9000 RPM|
|Weight	|40g(excluding accessories)|

# Raspberry Pi Example Usage
## 2.1、Resource Overview Diagram
Resource overview is as shown in the following diagram:<br>
![image](https://github.com/seengreat/POE-FAN-HAT-C/blob/main/pic1.png)<br>
Figure 2-1: Resource Overview Diagram of POE FAN HAT C<br>
①　Raspberry Pi 5 POE power input female header<br>
②　0.87-inch OLED display screen<br>
③　3007 active cooling fan mounting holes<br>
④　5V power indicator LED<br>
⑤　Raspberry Pi 40-pin expansion pin header<br>
## 2.2、Function Description
The product installation wiring diagram is as shown in Figure 2-2. The POE switch/router needs to support the IEEE 802.3af/at network standard. Then, proceed to install the thermal tapes, metal heat sink, fan wiring in order. Finally, correctly insert the product into the Raspberry Pi 5 and connect the 8-core network cable. <br>
![image](https://github.com/seengreat/POE-FAN-HAT-C/blob/main/pic2.png)<br>
Figure 2-2: Product Wiring Diagram<br>
## 2.3、 Install Python library
Since the example programs use Python 3 environment, installing the smbus library is necessary to run Python example programs:<br>
sudo apt-get install -y python-smbus<br>

##  2.4 Install lgpio library
The C language version of the example program uses the lgpio library. Install the lgpio library on Raspberry Pi 5:
wget https://github.com/joan2937/lg/archive/master.zip<br>
unzip master.zip<br>
cd lg-master<br>
make<br>
sudo make install<br>
##  2.5 Configuring I2C Interface
sudo raspi-config<br>
Enable the I2C interface<br>
Interfacing Options -> I2C -> Yes <br>
Install i2c-tools for confirmation<br>
sudo apt-get install i2c-tools<br>
Check the connected I2C devices<br>
i2cdetect -y 1<br>
You should see the address representing the board's OLED screen connected to the Raspberry Pi. The default address is typically 0x3c.<br>
##  2.6 Running Example Programs:
1）Properly install the POE FAN HAT C onto the Raspberry Pi 5 and boot up the Raspberry Pi.<br>
2）Run the Python program<br>
Navigate to the poe-fan-hat-c/python/ directory.<br>
sudo python3 main.py<br>
After running the program, the IP address of the Raspberry Pi 5 (192.168.2.242) and the real-time CPU temperature (62.8℃) will appear on the OLED screen of the board    <br> 
3）Running C Program:<br>
Navigate to the poe-fan-hat-c/c/ directory and execute the following commands:<br>
sudo make clean<br>
sudo make<br>
sudo ./main<br>
4）Temperature Control Test<br>
Open a new terminal window and install stress using the following command:<br>
sudo apt install stress<br>
 Run the testing command:<br>
stress -c 4<br>
At this point, the CPU temperature will gradually increase. When it exceeds 50℃, the onboard fan will automatically start.<br>

##  2.7 Boot Autostart Test
Using the systemd method:<br>
Create a .service file on the Raspberry Pi, such as poe-fan-hat-c.service. The file content should be as follows (assuming the main.py file is located at /home/pi/poe-fan-hat-c):<br>
<br>
[Unit]<br>
`Description=poe-fan-hat-c service`<br>
After=network.target<br>
<br>
[Service]<br>
Restart=always<br>
RestartSec=5<br>
Type=simple<br>
ExecStart=/usr/bin/python3 -u /home/pi/poe-fan-hat-c/python/main.py<br>
User=pi<br>
<br>
[Install]<br>
WantedBy=multi-user.target<br>
<br>
Save and exit the file, then copy it to the /etc/systemd/system directory:<br>
sudo cp poe-fan-hat-c.service /etc/systemd/system/   <br>                        
Reload all services:<br>
sudo systemctl daemon-reload <br>                                           
Enable the task to run automatically at startup:<br>
sudo systemctl enable poe-fan-hat-c.service   <br>                              
Start the task immediately:<br>
sudo systemctl start poe-fan-hat-c.service      <br>                              
Check the status of the task, the OLED display should continuously show the IP address and CPU temperature in normal conditions. Restart the Raspberry Pi and observe if the OLED screen displays the IP address and CPU temperature correctly.<br>
If you are using a C language example program, change the content from "ExecStart=/usr/bin/python3 -u /home/pi/poe-fan-hat-c/python/main.py" <br>
to <br>
"ExecStart= /home/pi/poe-fan-hat-c/c/main".<br>

__Thank you for choosing the products of Shengui Technology Co.,Ltd. For more details about this product, please visit:
www.seengreat.com__

