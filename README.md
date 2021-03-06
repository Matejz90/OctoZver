# OctoZver
Minimalistic, powerfull and good looking app for controlling 3D printer. Work on RPI with 3.5' LCDs to 7'. Work also with Windows PC and with Linux PC  
<p align="center">  
<img src="https://github.com/Matejz90/OctoZver/blob/main/print_screen/start_screen.png?raw=true" width="600" height="400" />  
</p>  
  
Hello there.  
  
Because i didn't find any suitable app without glitches and bugs for controlling my printer with Octoprint and RPI 3B+ i wrote my own app.  
Other solution writen in NodeJS made me only problems with my fat fingers because of drag&drop and solutions writen in proper way was not good because of too much functions and too small icons which i can't click on my 4' LCD display without pen.  
  
[Download here](https://github.com/Matejz90/OctoZver/releases/tag/v0.0.1)
  
App is writen in Python 3.8.3 with Kivy 2.0.0rc4 and packed with pyinstaller for one exe file.  
Because .exe must be decompressed from --onefile, app startup with slower PC-s can be up to 10-20s  
  
### Install:  
#### Windows: 
1.)Download file and unzip it.  
2.)Copy .exe and config.cfg to folder (C:\Program Files\OctoZver) or desktop (exe and cfg MUST be in same folder) and start app with double click (or make shortcut to desktop)  
#### Rpi:  
1.) Download app and config.cfg from github and save to folder (for example ```/pi/home/octoZver```)  
2.) cd to folder and execute unzip with ```/unzip octoZverRpi.zip```  
3.) Don't forget chmod octoZver file ```sudo chmod +x appOctoZverRpi``` or ```sudo chmod +x /pi/home/octoZver/appOctoZverRpi```  
4.) Edit config.cfg with text editor ```sudo nano config.cfg``` or ```sudo nano /pi/home/octoZver/config.cfg```  
  
If you want start app with terminal then don't forget cd to your folder (exsample ```cd /pi/home/octoZver/```)  
Start app with ```./appOctoZverRpi``` (```./pi/home/octoZver/appOctoZverRpi```)  
  
##### RPI auto boot to app  
Execute: ```sudo nano /etc/systemd/system/octozver.service``` and copy in this file:   
```
[Unit]
Description=Let's print!
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=pi
WorkingDirectory=/home/pi/octoZver/
ExecStart=/home/pi/octoZver/appOctoZverRpi
Restart=always
RestartSec=5
Nice=-2

[Install]
WantedBy=multi-user.target
```
Then you need to execute ```sudo systemctl enable octozver.service```  
If you edited file then you need execute ```systemctl daemon-reload``` and before is recommend stop service with ```sudo systemctl stop octozver.service```  
  
#### Ubuntu:  

### Requirements and imports:  
--- You need to install ---  
[Octoprint](https://github.com/OctoPrint/OctoPrint) with [DisplayLayerProgress plugin](https://github.com/OllisGit/OctoPrint-DisplayLayerProgress)  
  
-- If you use one of slic3r (slic3r, prusaslice, superslicer) --  
Go to Printer Settings and select Custom G-Code. In field Before layer change G-code add:  
```;BEFORE_LAYER_CHANGE```  
  
#### Notes  
1.) You need working octoprint and online connection. If your octoprint is not working then app also not work.  
2.) Because i want provide to everyone latest version with bugfixes i implement code that connect to my server and check version.  
3.) For now updater not work automatic and will you remind that you need newer version.  
```
def Version(self):
    link = "http://webpoint.si/version.txt"
    f = requests.get(link)
    if f.text != version:
        print("version not match")
```
4.) Values set in config.cfg files is applyed only for work with this app. If you have diferent feed rates set in slicer then for printing will octoprint use settings from g-code!!  
5.) If DLP plugin in octoprint and my app does not show layer count with some slicer (Slic3r, PrusaSlic3r...), you need to add in slicer [this solution](https://github.com/OllisGit/OctoPrint-DisplayLayerProgress/issues/102#issuecomment-569490240)
  
## Print Screens with instruction  
  
This is first screen. There you select what do you want with app. You can select files for print, start heating heat bead or hotend, turn on fans, extrude and retract some filament, exit app and emergency stop printer motherboard. **If you click on Emergency Button** in top right corner then **you need reset printer** (try with reset button on printer motherboard and reconnect to octoprint or restart complete printer **with power switch**) and **restart app** because it will crash.  
<p align="center">  
<img src="https://github.com/Matejz90/OctoZver/blob/main/print_screen/start_screen.png?raw=true" width="600" height="400" />  
</p>  
  
There you have files screen. If you had uploaded files to octoprint, there you can double click/double tap for start printing.  
<p align="center">  
<img src="https://github.com/Matejz90/OctoZver/blob/main/print_screen/files_screen.png?raw=true" width="600" height="400" />  
</p>  
  
There you have extrude filament screen. You can extrude or retract filament if you have enough heated hotend. In top right corner is Fast/Slow icon. With icon you change extruder feed rate for faster or slower extrusion/retraction.  
<p align="center">  
<img src="https://github.com/Matejz90/OctoZver/blob/main/print_screen/extrude_screen.png?raw=true" width="600" height="400" />  
</p>  
  
There you have extrude Control screen. You can control your printer. First you select one of 4 available distance (1mm, 10mm, 50mm, 100mm, default is 1mm) and then you can move bed to front or back, left and right and z axis up and down with click on icon. On middle is icon for homing (g28). For BLTouch is in right bottom corner mesh bed leveling (g29 command) and in right top corner you have reset BLTouch icon if you accidently screw up something with sensor.  
<p align="center">  
<img src="https://github.com/Matejz90/OctoZver/blob/main/print_screen/control_screen.png?raw=true" width="600" height="400" />  
</p>  
  
There you have Hotend temperature screen. Function is same like with control screen but instead of distance you select one from 4 available temperature steps. Default is 1°C. With temperature up icon and temperature down icon you select temperature which is then displayed in middle of middle icon and with press on that icon you send temperature to octoprint. **If you not click on icon between temp up/temp down, then temperature is not send to printer!**  
<p align="center">  
<img src="https://github.com/Matejz90/OctoZver/blob/main/print_screen/hote_temp_screen.png?raw=true" width="600" height="400" />  
</p>  
  
There you have Bed temperature screen. Function is same like Hotend temperature screen. **If you not click on icon between temp up/temp down, then temperature is not send to printer!**  
<p align="center">  
<img src="https://github.com/Matejz90/OctoZver/blob/main/print_screen/bed_temp_screen.png?raw=true" width="600" height="400" />  
</p>  
  
There you have Fan speed screen. If you click one of 6 icon then you set percentage of printing fan.  
<p align="center">  
<img src="https://github.com/Matejz90/OctoZver/blob/main/print_screen/fan_screen.png?raw=true" width="600" height="400" />  
</p>  
  
## License  
1.) For this app i WILL NOT PROVIDE ANY SOURCE CODE, NOR GUARANTEE AND APP IS LIKE IT IS because i want this app to work and look how i imagine in my head.    
2.) You can use app in commercial or personal purpose, but you are not allow to change code except config.cfg.  
3.) If you have problems with upper statement you are free to pass my app and use something else!  
4.) You can freely open issues, add suggestions, etc. on github or contact me with your issues on my email matejz90 @ gmail.com (without whitespace before and after @)  
5.) If comes to big problems with users or something else i will shut down my version server and app will be not working anymore (for all user).  
6.) With app i don't collect any data nor IP-adresses or something else and app use online connection only for version check.   
7.) This app is writen for myself and i give chance to other people to use it!  
