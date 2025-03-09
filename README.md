# AC_Control_via_Arduino
The project aimed to maintain the ambient temperature at a specific level. Infrared signals from the AC remote were recorded using Arduino. Then, when the temperature went out of the defined range, the pre-recorded infrared signals were sent to the AC receiver via LED using PID control.


During my internship, I carried out a development project. The purpose of this project is as follows:
The calibration laboratory, which is an accredited lab, must maintain a temperature of 20 degrees according to standards. The margin of error is ±1 degree. If the temperature goes outside this range, the validity of their calibration processes will be compromised. This laboratory has two air conditioning units. The main air conditioner operates 24/7 to maintain a constant temperature in the lab. The second air conditioner is manually activated by the staff if the temperature inside exceeds 21 degrees or falls below 19 degrees. I thought that what could be improved here is for the staff to periodically check the ambient temperature and decide whether to activate the second air conditioner. Additionally, I noticed that during times when the laboratory is not in use (during meal breaks or after working hours), no one monitors the temperature, and when they return, they have to wait for the temperature to return to the set level before they can start working again. To address this, I set out to implement my project using an Arduino Uno, a DHT22 temperature sensor, and an IR receiver/transmitter. First, I formulated the algorithm for the code in my mind. To determine the temperatures at which the air conditioner should operate, I examined the historical temperature data of the laboratory and observed when the second air conditioner was activated and how long it took to bring the temperature back to normal levels. Once I had completely established the algorithm, I began writing the code. I set the temperature range in which the second air conditioner would remain off between 19.2 and 20.7 degrees. However, simply opening the air conditioner within these ranges was not sufficient; the operating mode of the air conditioner also had to depend on these two temperatures. I wanted to integrate the PID controller that I learned about in school into this project, so that when the air conditioner first started operating, it would act aggressively and adjust the fan speed and the temperature output in a closed-loop manner using the PID controller as it approached 20 degrees. I intended to send these fan speed, mode, and temperature commands to the air conditioner via an IR LED. To do this, I first needed to capture the signals sent from the air conditioner remote control in written form using the IR receiver. Then we had ordered the needed materials for project. Since I had very little knowledge about IR protocols, I started researching. Initially, I tried common protocols, but after three days of experimenting, I could not get a response from the air conditioner. After extensive research, I learned that the IR signal protocol of the air conditioner I would be implementing the project on was not available in the Arduino library, so I decided to receive and send the signals in their raw form. After sending all the necessary signals from the remote and receiving them with the Arduino circuit I set up, I integrated them into the actual project code I would use. I also determined the values for the PID controller constants by inputting the laboratory’s dimensions and insulation conditions into an AI model for assistance. Finally, I built the circuit on a breadboard and tested it. I conducted my test in the presence of all the supplier quality engineers and obtained successful results. After that, I accepted the congratulations. 
You can find my project code in the "appendices" section. I write this code to implement a climate control system using an Arduino with PID control for heating and cooling, alongside IR signal transmission for air conditioning commands. It integrates the following components:
1.	DHT Sensor:
o	Reads temperature and humidity.
o	Connected to pin 2.
2.	IRremote Library:
o	Controls an IR LED connected to pin 3 for sending predefined raw IR codes.
3.	PID Control:
o	coolingPID operates in DIRECT mode.
o	heatingPID operates in REVERSE mode.
o	Both target a Setpoint of 20°C.
o	Gain values (Kp, Ki, Kd) are specified for each.
4.	Predefined Raw IR Codes:
o	Various IR signal patterns (e.g., rawData_ON, rawData_20_H_L) represent different air conditioner states. In form of rawData_T_M_S which T stands for temperature, M stands for mode and S stands for fan speed. Air conditioner takes the signal from LED as a complete command. So each time you press a button, it sends all of the current information (ex. Even if you only just increase the temperature, remote sends the temperature, mode and fan speed information again.) 
5.	“enum State {MONITORING, COOLING, HEATING};” line is for divide the system loop into three states. 
o	Monitoring state includes sensor reading and checks if temperature is out of the range. If temperature is out of the range, it starts Cooling or Heating state depending on the temperature. 
o	Cooling state monitors the temperature also and includes Cooling PID and a function to convert PID output to temperature value. And lastly a function, which takes its input from temperature value from previous function and depending on that value, sends the necessary raw IR code to air conditioner. When room temperature hits 20.0 degree, it starts Monitoring state again.
o	Heating state does the same as Cooling state but all in Heating mode.
