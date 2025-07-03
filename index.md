# Robotic Arm
For my project, I am building and programming a robotic arm with four joints, two to bend the arm up and down, one to spin its base, and one to open and close the claw. My modification was to separate the controller from the arm and make them communicate via Bluetooth. This is my first time doing a large coding project, and I am very proud with how it turned out!

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Scott M | West Torrance High School | Mechanical Engineering | Incoming Junior

![IMG_1128](https://github.com/user-attachments/assets/9db9a5bd-58c2-4cd4-8310-8a001db1bc48)

# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/x956hUlhUo8" title="Scott M Milestone 3" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My final milestone is finishing the modification I made to my robotic arm. The modification I chose was to separate the joysticks from the arm and use two HC05 Bluetooth modules to send commands between the joysticks and the arm, making them wireless. If I turn them on and move around, I can control the arm no matter where I am. I have two separate codes, one for the controller and one for the arm. The controller code takes the x- and y-values from each of the joysticks and turns them into single-character commands, which are then sent to an HC05. This HC05, connected to the controller, sends the characters to the HC05 connected to the arm. The arm then takes those characters and translates them into commands that make the arm move. There were a lot of technical difficulties and small errors I had to fix, but in the end, it works, and I'm really proud of how it turned out.



# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/edQ2cWIXHfk" title="Scott M  Milestone 2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My second milestone is finishing the base project of my robotic arm. I assembled all the hardware, and I have all the programming finished. My arm can go up and down, and it can open and close. Unfortunately, it has problems with turning left and right, but it seems like it’s more of a problem with the strength of the base servo than the programming. I reused the code from my first milestone in the final code to measure the joystick coordinates while constantly telling me those coordinates, which is very useful. The hardest part in this milestone was understanding how to do the rest of the code, because none of it made sense to me, and I spent an entire day trying to figure it out on my own without any success. The next day, thankfully, I got help from my instructor and ended up writing all the rest of the code in that one day, which was pretty crazy. This code runs the joystick coordinates through a series of parameters that, depending on the values of the coordinates, move the servos on the arm in different ways.

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/Y9cHcGCAhAc" title="Scott M  Milestone 1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My first big milestone was being able to understand some of the code that I would be using in this project. I hadn't had much coding experience, if any, before this, so being able to do what I did here was a big deal to me. At the beginning of the camp, we were given prewritten code in C++ to test the joysticks. When I began writing the code for my project, I took this code and used it in my actual project, after a few tweaks to make it useful. It's a pretty small thing in itself, but doing this proves I had a general understanding of what the different parts of the code did and how to edit or even create them to make then do what I want to. This milestone isn't hugely impactful to my actual project, but getting a foundation of understanding in C++ was an immense achievement for me.

# Schematics 
![image](https://github.com/user-attachments/assets/4cf293ee-62a2-4153-a188-fd0da88f11ff)

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

Controller code
```c++
#include "src/CokoinoArm.h"
#include <SoftwareSerial.h>

int rx = 3;
int tx = 2;

SoftwareSerial BT_Serial(rx, tx);
char bt_data;

int xL,yL,xR,yR = 0;

void setup() { 
Serial.begin(38400); 
BT_Serial.begin(38400);

//! - if this is false, do this
} 
void loop(){

int xLcenter = 503;
int yLcenter = 513;
int xRcenter = 502;
int yRcenter = 505;
int deadzone = 25;
xL = analogRead(A0);
yL = analogRead(A1); 
xR = analogRead(A2); 
yR = analogRead(A3); 

//L to R, T to B - xL:0<503<1023, yL:1023<512.5<0, xR:0<502<1023, yR:1023<505<0
if((xLcenter - deadzone) < xL && xL < (xLcenter + deadzone) && (yLcenter - deadzone) < yL && yL < (yLcenter + deadzone) && (xRcenter - deadzone) < xR && xR < (xRcenter + deadzone)){
  BT_Serial.write('n');
  Serial.println("no movement");
}
if(!((xLcenter - deadzone) < xL && xL < (xLcenter + deadzone))){
  if(0 <= xL && xL <= 239){
    BT_Serial.write('F');
    Serial.println("sent F");delay(20);}
  if(239 < xL && xL <= 478){
    BT_Serial.write('f');
    Serial.println("sent f");delay(20);}
  if(528 < xL && xL <= 776){
    BT_Serial.write('b');
    Serial.println("sent b");delay(20);}
  if(776 < xL && xL <= 1023){
    BT_Serial.write('B');
    Serial.println("sent B");delay(20);}
}
if(!((yLcenter - deadzone) < yL && yL < (yLcenter + deadzone))){
  if(0 <= yL && yL <= 239){
    BT_Serial.write('R');
    Serial.println("sent R");delay(20);}
  if(239 < yL && yL <= 478){
    BT_Serial.write('r');
    Serial.println("sent r");delay(20);}
  if(528 < yL && yL <= 776){
    BT_Serial.write('l');
    Serial.println("sent l");delay(20);}
  if(776 < yL && yL <= 1023){
    BT_Serial.write('L');
    Serial.println("sent L");delay(20);}
}
if(!((xRcenter - deadzone) < xR && xR < (xRcenter + deadzone))){
  if(0 <= xR && xR <= 239){
    BT_Serial.write('C');
    Serial.println("sent C");delay(20);}
  if(239 < xR && xR <= 478){
    BT_Serial.write('c');
    Serial.println("sent c");delay(20);}
  if(528 < xR && xR <= 776){
    BT_Serial.write('o');
    Serial.println("sent o");delay(20);}
  if(776 < xR && xR <= 1023){
    BT_Serial.write('O');
    Serial.println("sent O");delay(20);}
}
}
```

Arm code
```c++
#include"src/CokoinoArm.h"
#include <SoftwareSerial.h>

int tx = 2;
int rx = 3;

SoftwareSerial BT_Serial(rx, tx);
char bt_data;

CokoinoArm arm;
int xLcenter = 503;
int yLcenter = 513;
int xRcenter = 502;
int yRcenter = 505;
int deadzone = 25;

//bt_data is the variable
//servo1.write

void setup() { 
  arm.ServoAttach(4,5,6,7);
Serial.begin(38400);
BT_Serial.begin(38400); 
int xL,yL,xR,yR;

pinMode(tx, OUTPUT);
pinMode(rx, INPUT);
//! - if this is false, do this
} 
void loop(){

  if (BT_Serial.available() > 0) {
    bt_data = BT_Serial.read();
    Serial.print("Received: ");
    Serial.println(bt_data);
  }
  
  if(bt_data == 'F'){arm.down(50); Serial.println(" down 1");delay(10);}
  if(bt_data == 'f'){arm.down(50); Serial.println(" down 2");delay(10);}
  if(bt_data == 'b'){arm.up(50); Serial.println(" up 2");delay(10);}
  if(bt_data == 'B'){arm.up(50); Serial.println(" up 1");delay(10);}
  if(bt_data == 'R'){arm.right(5); Serial.println(" right 1");delay(10);}
  if(bt_data == 'r'){arm.right(50); Serial.println(" right 2");delay(10);}
  if(bt_data == 'l'){arm.left(50); Serial.println(" left 2");delay(10);}
  if(bt_data == 'L'){arm.left(5); Serial.println(" left 1");delay(10);}
  if(bt_data == 'C'){arm.close(0); Serial.println(" close 1");delay(10);}
  if(bt_data == 'c'){arm.close(50); Serial.println(" close 2");delay(10);}
  if(bt_data == 'o'){arm.open(50); Serial.println(" open 2");delay(10);}
  if(bt_data == 'O'){arm.open(0); Serial.println(" open 1");delay(10);}
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Robotic Arm Kit | Contains physical components used to build the arm | $49.99 | <a href="https://www.amazon.com/LK-COKOINO-Compliment-Engineering-Technology/dp/B081FG1JQ1"> Link </a> |
| Servo Shield | Expansion shield for Arduino Nano | $10.99 | <a href="https://www.amazon.com/HiLetgo-Expansion-Sensor-Arduino-Duemilanove/dp/B07VQRCC8F/ref=sr_1_1_sspa?crid=IY8280UJPZ8D&dib=eyJ2IjoiMSJ9.gOnvWbSP2fpJyjlzThZoFsFPHoeaF2QpSk_jNdngKIr1twGn_LzcDoaoxYvFyCU-mVjs0xm0675XcM9jJCRLlzDOmjbGgP1sIqUhTjt4NviT5cbtoA-UvEYAIHWDWIfkb2aFMmhgHU544Wc7YJiipzzt3fuSGamCrVeh0ONFUE7GqEzOyVIpGdjm_kZqEYrk4l6Ol054nebh1I2eZg7hcYRPAX8iNqbzSBQnTX3EaUY.ewdYdtnT9O7qRCuhV_2P0vAhp7a5Ue2sdk1REW8_gKI&dib_tag=se&keywords=arduino+nano+servo+shield&qid=1716857827&s=toys-and-games&sprefix=arduino+nano+servo+shield%2Ctoys-and-games%2C85&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| Screwdriver Kit | Used to help assemble the arm | $7.99 | <a href="https://www.amazon.com/Small-Screwdriver-Set-Mini-Magnetic/dp/B08RYXKJW9/"> Link </a> |
| Electronics Kit | Contains components needed to wire the physical parts together | $13.49 | <a href="https://www.amazon.com/Smraza-Electronics-Potentiometer-tie-Points-Breadboard/dp/B0B62RL725/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=2IC3T44H3U3WG&cv_ct_cx=breadboard%2Bkit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard%2Bkit&pd_rd_i=B0B62RL725&pd_rd_r=67e1f4ff-e3b9-44e4-b441-b4ae282f036b&pd_rd_w=UjFaP&pd_rd_wg=0xRoC&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=BFGP77H27ZN31W4PZAW6&qid=1715911733&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard%2Bkit%2Caps%2C109&sr=1-2-9f062ed5-8905-4cb9-ad7c-6ce62808241a&th=1"> Link </a> |
| 9V Barrel Jack | Allows 9V batteries to power the Arduinos | $5.99 | <a href="https://www.amazon.com/DZS-Elec-Connector-Experimental-5-5x2-1mm/dp/B07FDS11ZY/ref=sr_1_5?crid=2KDQRHR9QTG87&dib=eyJ2IjoiMSJ9.QXzrFs_APhSZ1IJhcXZvMQHwewvRuQ3vr1brQtDco3W0bnAprDG7jH7ie8dBlokDPWbOLcDtgbrHrNUzcyb61YgxbGO0UFeN6K8ktLZDkV3jlxoO940ZYOk8jrd3G8yxrkH-cUJgXaiOka1FWDDJJssGcdvyH2WlPRHUtZKQgBpoGa4M3j8wwx3yssPZrOJK32Pfs9ZLtCibGXHxhNbXOBuXOisFlpDByQ2NJcndu5iOa0dZ8jknYgybT1KOyzP9_lSVyQNCkcxcjanEjyf4Z6jMdRX-G08K6SY7IM-agSA.UzM8eWF_dtBmatnqwrbt1mCm8-reUmM7Mqm3SWpbviM&dib_tag=se&keywords=9v%2Bto%2Bbarrel%2Bjack&qid=1716857906&s=electronics&sprefix=9v%2Bto%2Bbarrel%2Bjack%2Celectronics%2C98&sr=1-5&th=1"> Link </a> |
| Digital Multimeter | Used to measure the strength of electric current | $15.99 | <a href="https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b%3Aamzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b&cv_ct_cx=digital%2Bmultimeter&dib=eyJ2IjoiMSJ9.5LQumrfBR8l0mKnJCJlRg73dxpou0gqYD_ffU3srgs0Utegwth8GcQCSVXVzeZeLSJx5J3itz5TLdmJHsrVITQ.-00jRPoT-bBy26YC4LzQ-S4cYdztgmSMGb83_WEm6HY&dib_tag=se&keywords=digital%2Bmultimeter&pd_rd_i=B01ISAMUA6&pd_rd_r=e1ff2570-7e4a-4906-bc55-6f819d48d1bc&pd_rd_w=h7HgL&pd_rd_wg=0ZcFH&pf_rd_p=e8da13fc-7baf-46c3-926a-e7e8f63a520b&pf_rd_r=R6YKX3NXTDQ1PQP4H8RM&qid=1715911879&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sr=1-1-7efdef4d-9875-47e1-927f-8c2c1c47ed49-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&th=1"> Link </a> |
| 9V Batteries | Used to power the Arduinos | $8.99 | <a href="https://www.amazon.com/dp/B00MH4QM1S/ref=vp_d_pb_TIER4_cml_lp_B0BJ26CHZB_pd?_encoding=UTF8&pf_rd_p=b8d9960f-63a9-4d69-a8de-de9514a27e41&pf_rd_r=1RRARBM9YNNHR89D8B2N&pd_rd_wg=FwKYY&pd_rd_i=B00MH4QM1S&pd_rd_w=XrNnI&content-id=amzn1.sym.b8d9960f-63a9-4d69-a8de-de9514a27e41&pd_rd_r=edb0610d-b8f5-4671-814f-f6cb22938f22&th=1"> Link </a> |
| HC05 Bluetooth Module | Connects the controller and arm wirelessly | $9.99 | <a href="https://www.amazon.com/DSD-TECH-HC-05-Pass-through-Communication/dp/B01G9KSAF6/ref=sr_1_3?dib=eyJ2IjoiMSJ9.VZL1p5RDGQw7c8DXaqrVkRyfFEBz0HhuagQj9O7D5y7Vz0Nu_seyhu0n8hd8O9KK08SsCjmDeY_2P9Hk-FrkFijctdchIkLgZUp68jXK86DL7wGHNi8ABkwzQHmWckh7p3YPqxt2_tJwb6ZLjEG79qlYPAdrp6AQQKLwbhbHEtKwtgLRksyGneDiMASf2h3DDrDllfZYyWlT-lJUwDn4JG8OumBtLbqGp2DgRUAnQUU.he_EIFXFJ6p5daY1g4C0cEdTs6ovGbLpleSeb1OCG9c&dib_tag=se&keywords=hc05+bluetooth+module&qid=1751574750&sr=8-3"> Link </a> |
| Arduino Nano | We need an extra Arduino to run the controller | $24.99 | <a href="https://www.amazon.com/Arduino-A000005-ARDUINO-Nano/dp/B0097AU5OU/ref=sr_1_2?crid=24HC4I1HJ53EN&dib=eyJ2IjoiMSJ9.UR9t6Z2D5rIVJlr8NPSrk8lsooCrlbXp6PW8NiTHZI1_D9tr9puHNx6d2oy5xaXQRl8lRriprovLWa_p5KYDSQ7kQLffHueQv6DIDbn516eCGAKTDoN0O2PqSOT3lY9yD63zf32QndU85Hs8dZ6AI2Y_ZGRiJp64Ku4Q67A9TlI2J1ARiKFOI4KiDH-aQC-tovAsJmG6B50uP-Kbywnbj88N-d_Jo00Mgmi7gjMk7aE.r4dYVrYIYSPV9d6seSvWyoefCu0utd6fh4VmknJNTsY&dib_tag=se&keywords=arduino+nano&qid=1751574940&sprefix=arduino+nano%2Caps%2C874&sr=8-2"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
