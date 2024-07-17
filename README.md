
<!-- PROJECT LOGO -->
<br />
<p align="center">
  <h3 align="center">Mobile Spyware</h3>
</p>
Mobile SpyWare is an educational tool designed for the command and control of Android devices via a web GUI, leveraging Metasploit and ADB.

## About The Project
Mobile Spy Ware utilizes Docker containers to simulate a Kali Linux environment with the Metasploit framework. It communicates with Metasploit via RPC calls and interacts with Android devices using ADB over LAN.

Disclaimer: This software is strictly for educational purposes. Using it for spying on real devices is illegal. Use at your own risk.

## Support Me
If you like my work, consider buying me a coffee:

[![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/mwakideu)

<!-- ABOUT THE PROJECT -->
## Dashboard
![Dashboard](screenshots/dashboard.png)




The tool:
* Uses Docker containers to simulate a Kali Linux environment with the Metasploit framework
* Uses RPC calls to communicate with the Metasploit framework API
* Uses ADB to install the payload on the target device & implement additional functionalities (the device must be in the same LAN)

### Disclaimer
This software is for educational purposes only. Using the tool for spying on real devices is strictly illegal. USE THE SOFTWARE AT YOUR OWN RISK. THE AUTHOR ASSUMES NO RESPONSIBILITY FOR YOUR USAGE.

We strongly recommend you have coding and Docker knowledge. Do not hesitate to read the source code and understand the mechanism of the tool.
### Built With
* [Bootstrap](https://getbootstrap.com)
* [JQuery](https://jquery.com)
* [Rails](https://rubyonrails.org/)
* [Metasploit](https://www.metasploit.com/)
* [ActiveAdmin](https://activeadmin.info/)
* [Docker](https://www.docker.com/)
* [Android Debug Bridge](https://developer.android.com/studio/command-line/adb)

### Features

**Available actions using Metasploit framework**

- [x] Dump System Info
- [x] Dump Location
- [x] Live Webcam Stream
- [x] Dump Messages
- [x] Change Audio Mode
- [x] Dump Call Logs
- [x] Dump Local Time
- [x] Microphone Recording - Only working for Android < 9.0
- [x] Unistall App
- [x] List Installed Apps
- [x] Send Message
- [x] Dump Contacts
- [x] Lock/Unlock Screen
- [x] Run Shell Command
- [x] Webcam Snap
- [x] Open App
- [x] Install App
- [x] Device Info
- [x] Hide/Show payload app icon

**Additional actions via ADB**

- [x] Dump Whatsapp conversations from backup DB
- [x] Dump Wi-Fi Information
- [x] Screen Snap
- [x] Upload File
- [x] Record Screen
- [x] Pull File
- [x] Start Monero crypto miner in the background



<!-- GETTING STARTED -->
## Getting Started

### Prerequisites
- [Docker](https://www.docker.com/products/docker)
- [Chocolatey](https://chocolatey.org/install)

### Installation

1. Install ADB via Chocolatey. From an elevated powershell prompt (Run as administrator):
```
choco install adb --version=1.0.39
```
2. Create a project directory, and download docker-compose file. You can use curl:
```
curl https://raw.githubusercontent.com/JayMwakideu/Mobile_Spyware/main/docker/docker-compose.yml -o docker-compose.yml
```
3. Download required images:
```
docker-compose pull
```

### Deployment
1. Start ADB server from a command prompt:
```
adb server
```
2. Optional, set your Google MAPS API Key in docker-compose file for dashboard widget
3. Start the container (inside the project directory):
```
docker-compose up
```
4. Wait for the containers to initialize, and access the application in the browser via http://localhost/admin
The files directory will be created. It will contain the payloads and the actions outputs.
  
### Usage
1. Ensure USB Debugging is enabled on your Android device [tutorial HERE](https://www.embarcadero.com/starthere/xe5/mobdevsetup/android/en/enabling_usb_debugging_on_an_android_device.html)
2. Optional: Plug-in your Android device to the laptop via USB - Otherwise you won't be able to use ADB functions and you will have to install the malware manually
3. Open the rails app in the browser: http://localhost/admin and login. Default credentials:

User: **admin@example.com**

Password: **password**

4. Generate APK
* Check your machine IP address on LAN. For windows you can use
```
ipconfig
```
![Ipconfig Screen Shot](screenshots/ipconfig.png)
* Go to Payloads (http://localhost/admin/apk_payloads) and Create New
* Select port 4444, input the machine IP address and give a name for the APK
* The APK payload will be generated in [project path]/files/payloads
5. Install APK
* Go to APK Installations (http://localhost/admin/apk_installations) and Create New
* Select your previously generated apk from the list and leave the target as usb
* You might have to approve a prompt on the device
6. Run Actions
* Go to Remote (http://localhost/admin/remote)
* Open the installed app on your device. You should see green light for your MSF connectivity right after. The ADB should also turn green if your device is plugged via USB
* Run actions by clicking on the app-looking icons on the device widget. The output will be displayed in the terminal widget, and the output will be stored in [project path]/files/dumps

![Remote Screen Shot](screenshots/remote.png)
  
### Debugging Issues
**MSF/ADB connection issues (green light not appearing in Remote page)**
* Ensure you generated the apk with correct machine IP
* Ensure connection from smartphone to your machine is not blocked by local firewall. Otherwise you should allow connection on ports 2222, 3333, 4444. 
To check that, you can try to access http://[your machine IP]:2222 . You should be able to access the MSF container file system via http server
* Ensure Docker container - HOST connectivity is working. Host machine is accessed via **gateway.docker.internal**, which is set in docker-compose file. If that doesn't work for you, you can also try to replace it with **docker.host.internal**
* For ADB connection, you can try to restart the local server:
```
adb kill-server
adb server
```
* For ADB connection, ensure USB debugging is enabled

### Build

**Software requirements**
- [Docker](https://www.docker.com/products/docker)
- [NodeJS & npm](https://nodejs.org/en/download/)
- [Ruby 2.6.6](https://rubyinstaller.org/downloads/archives/)

Rails Server build steps:
```
git clone https://github.com/JayMwakideu/Mobile_Spyware.git
cd Mobile-Spyware
gem install bundler:2.1.4
bundle
npm install yarn -g
yarn install --check-files
```
Ensure the following environment variables are set:
```
ANDROIDSPYWARE_DATABASE_HOST=localhost
ADB_HOST=localhost
RAILS_HOST=127.0.0.1
```
Run the server:
```
rails server
```
* Note: local postgresql database can also be used as alternative, but you will need to run the rake scripts for initialization:
```
rake db:create //alternatively run createdb android_spyware_[developmen|test|production]
rake db:schema:load
rake db:seed /too add mock data required for start-up
```
## Roadmap
- [ ] Implement covert payload
- [ ] Implement option for persistence script (connection is lost after reboot)
- [ ] Implement option for public IP handler (either ngrok or cloud solution) to be able to track device outside of LAN
- [ ] Implement remote payload delivery.

## License
Distributed under the MIT License. See `LICENSE` for more information.

<!-- CONTACT -->
## Contact
[Linkedln](https://www.linkedin.com/in/jaymwakideu/)

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
## Acknowledgments

I want to express my sincere gratitude to Canciu Costin for his original repo.

## Contributors



