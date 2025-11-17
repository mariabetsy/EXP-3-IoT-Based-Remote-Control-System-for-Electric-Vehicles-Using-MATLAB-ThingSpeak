# EXP-3-IoT-Based-Remote-Control-System-for-Electric-Vehicles-Using-MATLAB-ThingSpeak

## AIM
To design and implement an IoT-based remote control system for an electric vehicle (EV) using MATLAB and ThingSpeak API. The system enables remote operations such as:
•	Locking/Unlocking doors
•	Starting/Stopping the engine
•	Turning lights ON/OFF
The MATLAB script communicates with ThingSpeak, which acts as an intermediary between MATLAB and an embedded system (ESP32).
 
## THEORY
1. Internet of Things (IoT) in Electric Vehicles
IoT enables remote monitoring and control of EV systems using cloud-based communication. By integrating MATLAB with ThingSpeak, users can send control signals and retrieve the vehicle’s status remotely.
2. ThingSpeak Cloud Platform
ThingSpeak is a cloud-based IoT analytics platform that allows:
✅ Data collection from embedded devices
✅ Remote command transmission using APIs
✅ Real-time monitoring and visualization
3. Working Principle
1.	User Inputs Command in MATLAB → e.g., "Start Engine"
2.	MATLAB Sends Command to ThingSpeak API
3.	ESP32 Reads Command from ThingSpeak and performs the action
4.	ESP32 Updates Status on ThingSpeak
5.	MATLAB Reads Status from ThingSpeak and displays it
 
## PROCEDURE
🔹 Step 1: Setup ThingSpeak Account
1.	Create a Free ThingSpeak Account → https://thingspeak.com
2.	Create a New Channel and Add Two Fields: 
o	Field 1 → Control Commands
o	Field 2 → EV Status
3.	Note Down the Write & Read API Keys
🔹 Step 2: Implement MATLAB Code
1.	Open MATLAB and create a new script.
2.	Copy and paste the MATLAB code (provided earlier).
3.	Replace YOUR_WRITE_API_KEY, YOUR_READ_API_KEY, and YOUR_CHANNEL_ID with your ThingSpeak credentials.
4.	Run the script and select an action from the menu.
🔹 Step 3: ESP32 Integration (Optional)
1.	ESP32 reads the command from ThingSpeak using Wi-Fi.
2.	Executes the command (e.g., turns ON engine).
3.	Updates the EV status back to ThingSpeak.
🔹 Step 4: Display Vehicle Status in MATLAB
1.	MATLAB retrieves the latest EV status from ThingSpeak.
2.	Displays the confirmation message to the user.



## MATLAB Code (Without MQTT Client Toolbox)
```
clear; clc;
 
% Define ThingSpeak API Details
writeAPIKey = 'YOUR_WRITE_API_KEY';  % Replace with your ThingSpeak Write API Key
readAPIKey = 'YOUR_READ_API_KEY';    % Replace with your ThingSpeak Read API Key
channelID = 'YOUR_CHANNEL_ID';       % Replace with your ThingSpeak Channel ID
 
% Remote Control Menu
disp('Choose a remote function:');
disp('1 - Lock Doors');
disp('2 - Unlock Doors');
disp('3 - Start Engine');
disp('4 - Stop Engine');
disp('5 - Turn On Lights');
disp('6 - Turn Off Lights');
 
choice = input('Enter your choice (1-6): ');
 
% Define EV control commands
commands = ["LOCK", "UNLOCK", "START", "STOP", "LIGHT_ON", "LIGHT_OFF"];
 
% Validate choice and send command
if choice >= 1 && choice <= 6
    commandSent = commands(choice);
    url = ['https://api.thingspeak.com/update?api_key=', writeAPIKey, '&field1=', commandSent];
    
    % Send HTTP request to update ThingSpeak
    response = webwrite(url, []);
    disp(['Command Sent: ', commandSent]);
    
    % Wait for ESP32 to update status
    pause(3);
    
    % Read EV Status from ThingSpeak
    statusURL = ['https://api.thingspeak.com/channels/', channelID, '/fields/2/last.txt?api_key=', readAPIKey];
    evStatus = webread(statusURL);
    
    % Display received status
    disp(['EV Status: ', evStatus]);
else
    disp('Invalid choice. Please enter a number between 1 and 6.');
end
```


## Output:
<img width="1288" height="703" alt="image" src="https://github.com/user-attachments/assets/e7097dba-2a43-4e3f-b7a2-25f66b2bf9e6" />

## Result:

This experiment demonstrated how MATLAB can be used to remotely control an EV using ThingSpeak API. The system provides a secure, cloud-based approach to IoT-enabled vehicle automation.


