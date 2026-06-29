# Mini CNC Plotter

Capstone Project

Electrical and Electronics Engineering

Bahçeşehir University

## Team Members

Ali Eren Gündüz
Aytunç Aydın

## Advisor

Assoc. Prof. Dr. Ömer Polat

## Project Description

This project presents the design and implementation of a low-cost Mini CNC Plotter based on Arduino Uno, TinyCNC firmware, recycled CD/DVD stepper mechanisms, L293D motor drivers, and an SG90 servo motor.

The project aims to provide an educational CNC platform for demonstrating embedded systems, motion control, and hardware–software integration.

## Repository Structure

Firmware/
/*
Mini CNC Plotter firmware, based in TinyCNC https://github.com/MakerBlock/TinyCNC-Sketches Send GCODE to this Sketch using gctrl.pde https://github.com/damellis/gctrl
Convert SVG to GCODE with MakerBot Unicorn plugin for Inkscape...
More info: http://www.makerblog.at/2015/02/projekt-mini-cnc-plotter-aus-alten-cddvd-laufwerken/
*/
#include <Servo.h> #include <Stepper.h>
#define LINE_BUFFER_LENGTH 512

// Servo positions
const int penZUp	= 80; const int penZDown = 40; const int penServoPin = 6;
const int stepsPerRevolution = 20;	

Servo penServo;
Stepper myStepperY(stepsPerRevolution, 2,3,4,5);
Stepper myStepperX(stepsPerRevolution, 8,9,10,11);

struct point { float x, y, z; }; struct point actuatorPos;

// Drawing parameters
float StepsPerMillimeterX = 6.0; float StepsPerMillimeterY = 6.0;
float Xmin=0, Xmax=40, Ymin=0, Ymax=40; float Xpos=Xmin, Ypos=Ymin, Zpos=1; boolean verbose = false;
int StepDelay = 0, LineDelay = 50;

void setup() { Serial.begin(9600); penServo.attach(penServoPin); penServo.write(penZUp); myStepperX.setSpeed(250); myStepperY.setSpeed(250);
Serial.println("Mini CNC Plotter ready!");
}

void loop() {
char line[LINE_BUFFER_LENGTH]; int idx=0;
boolean inComment=false, inSemi=false;

while (Serial.available()>0) { char c = Serial.read();

if (c=='\n'||c=='\r') { if (idx>0) {
line[idx]='\0';
if (verbose) Serial.println(line); processIncomingLine(line, idx); idx=0;
}
inComment=inSemi=false; Serial.println("ok");
}
else if (!inComment && !inSemi) { if (c=='(') inComment=true; else if (c==';') inSemi=true;
else if (c>='a'&&c<='z') line[idx++]=c-'a'+'A'; else if (c>' ') line[idx++]=c;
}
else if (c==')') inComment=false;
}
}

CAD/
Firmware/
    MiniCNC.ino

Documentation/
    Final_Report.pdf

Images/
    Figure3_Isometric.png
    SystemArchitecture.png

README.md
Schematics/
├── Circuit_Diagram.pdf
│   ├── Circuit_Diagram.png
│   └── Wiring_Diagram.png
Mechanical/
Documentation/
Images/
