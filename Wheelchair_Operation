#Built for Toggle system, meaning once a command is pressed there is no active user input until the sequence is finished unless interrupted by a safety sensor.
#This code is longer than necessary and full of commented out sections which were switched back on during testing.
#Be kind, this was my first project :)

#Active Sequence:
#Button on screen is pressed
#Screen Locks Out
#Def Switcher decides program to follow, based on previous state
#An Actuator is activated
#Time Delay invoked (Or an alternative program statement for Safety Sensors)
#Display updated to show delay, with some emergency buttons available
#At end of Time Delay Actuator Reset to False
#Next Actuator in program selected
#At end of routine returns to main code waiting for inputs from Human Interface

#For Reference:
#Red '#B22222'
#Yellow '#FFFF00'
#Green '#008000'
#Blue '#87CEFA'
#Ivory '#FFFFF0'
#LightSkyBlue '#87CEFA'

import time
import RPi.GPIO as GPIO
from tkinter import *
import tkinter.font
import tkinter as tk

# tkinter GUI basic settings
root = tk.Tk()
root.title("Wheelchair Controls")
root.config(background = "#FFFFF0" ) #hexcode
root.minsize(700,300)
modeselect = tk.Frame(root)
modeselect.pack()
permset = tk.Frame(root)
permset.pack()
Font1 = tkinter.font.Font(family = 'Helvetica', size = 24, weight = 'bold')

#Information = Tk()
#Information.title("Wheelchair Info")
#Information.config(background ="#AFEEEE")
#Information.minsize(700,300)

#Pin Assignments
GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)

Seat_Rise 		=       18
Seat_Retract	    	=       16
DRails_Extend		=       31
DRails_Retract		=       33
Paws_Extend		=       40
Paws_Retract		=       38
HRails_Extend		=       35
HRails_Retract		=       37
Sensor_Brake_R		=	32
Sensor_Brake_L		=	13
Sensor_Press_R	        =	12
Sensor_Press_L	        =	7
HIGH_Sig                =       11  #This is a reference (5V) High Signal

Ind_Seat_Rise 		=   False
Ind_Seat_Retract	=   False
Ind_DRails_Extend	=   False
Ind_DRails_Retract      =   False
Ind_Paws_Extend		=   False
Ind_Paws_Retract	=   False
Ind_HRails_Extend	=   False
Ind_HRails_Retract	=   False
Ind_Sensor_Brake_R	=   False
Ind_Sensor_Brake_L	=   False
Ind_Sensor_Press_R      =   False
Ind_Sensor_Press_L      =   False

global Previous_Mode
global Resume_Mode
global Current_Mode
global Goal_Mode

Previous_Mode = "Mobile"
Current_Mode = ""
Goal_Mode = ""
Resume_Mode = ""

GPIO.setup(Seat_Rise, GPIO.OUT, initial = GPIO.HIGH)
GPIO.setup(Seat_Retract, GPIO.OUT, initial = GPIO.HIGH)
GPIO.setup(DRails_Extend,GPIO.OUT, initial = GPIO.HIGH)
GPIO.setup(DRails_Retract,GPIO.OUT, initial = GPIO.HIGH)
GPIO.setup(Paws_Extend,GPIO.OUT, initial = GPIO.HIGH)
GPIO.setup(Paws_Retract,GPIO.OUT, initial = GPIO.HIGH)
GPIO.setup(HRails_Extend,GPIO.OUT, initial = GPIO.HIGH)
GPIO.setup(HRails_Retract,GPIO.OUT, initial = GPIO.HIGH)
GPIO.setup(Sensor_Brake_R,GPIO.IN, pull_up_down = GPIO.PUD_DOWN)
GPIO.setup(Sensor_Brake_L,GPIO.IN, pull_up_down = GPIO.PUD_DOWN)
GPIO.setup(Sensor_Press_R,GPIO.IN, pull_up_down = GPIO.PUD_DOWN)
GPIO.setup(Sensor_Press_L,GPIO.IN, pull_up_down = GPIO.PUD_DOWN)
GPIO.setup(HIGH_Sig, GPIO.OUT, initial = GPIO.HIGH)

###Actuator Delays for Testing, minimal delay
##Seat_Rise_Delay	        = 1
##Seat_Retract_Delay      = 1
##DRails_Extend_Delay	= 1
##DRails_Retract_Delay    = 1
##Paws_Extend_Delay	= 1
##Paws_Retract_Delay  	= 1
##HRails_Extend_Delay	= 1
##HRails_Retract_Delay	= 1
##Brakes_Delay            = 1
##Press_Delay             = 1

#Actuator Delays
Seat_Rise_Delay	        = 7
Seat_Retract_Delay      = 7
DRails_Extend_Delay	= 16
DRails_Retract_Delay    = 16
Paws_Extend_Delay	= 2.5
Paws_Retract_Delay  	= 10
HRails_Extend_Delay	= 8
HRails_Retract_Delay	= 11
Brakes_Delay            = .5
Press_Delay             = .5

def Actuators_Off():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay
    global Press_Delay
    global Brakes_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop

    global Ind_Seat_Rise
    global Ind_Seat_Retract
    global Ind_DRails_Extend
    global Ind_DRails_Retract
    global Ind_Paws_Extend
    global Ind_Paws_Retract
    global Ind_HRails_Extend
    global Ind_HRails_Retract
    global Ind_Sensor_Brake_R
    global Ind_Sensor_Brake_L
    global Ind_Sensor_Press_R
    global Ind_Sensor_Press_L

    GPIO.output(Seat_Rise,GPIO.HIGH)
    GPIO.output(Seat_Retract,GPIO.HIGH)
    GPIO.output(DRails_Extend,GPIO.HIGH)
    GPIO.output(DRails_Retract,GPIO.HIGH)
    GPIO.output(Paws_Extend,GPIO.HIGH)
    GPIO.output(Paws_Retract,GPIO.HIGH)
    GPIO.output(HRails_Extend,GPIO.HIGH)
    GPIO.output(HRails_Retract,GPIO.HIGH)
    
    Ind_Seat_Rise 	= False
    Ind_Seat_Retract 	= False
    Ind_DRails_Extend 	= False
    Ind_DRails_Retract 	= False
    Ind_Paws_Extend 	= False
    Ind_Paws_Retract 	= False
    Ind_HRails_Extend 	= False
    Ind_HRails_Retract 	= False

#Tripped Sensor Programs are written for use with expected interrupts
##
##def Tripped_Sensor_Brake_R():
##    print("Right Brakes Disengaged")
##    Ind_Sensor_Brake_R     =   False
##    while(GPIO.input(32) == False):
##        time.sleep(Brakes_Delay)
##        Sensor_Brake_R = GPIO.input(32)
##        print("Engage Brakes")
##    Ind_Sensor_Brake_R     =   True
##    
##def Tripped_Sensor_Brake_L():
##    print("Left Brakes Disengaged")
##    Ind_Sensor_Brake_L     =   False
##    while(GPIO.input(13) == False):
##        time.sleep(Brakes_Delay)
##        Sensor_Brake_L = GPIO.input(13)
##        print("Engage Brakes")
##    Ind_Sensor_Brake_L     =   True
##
##def Tripped_Sensor_Press_R():
##    print("Right Pressure Sensor Disengaged")
##    Ind_Sensor_Press_R     =   False
##    while(GPIO.input(12) == False):
##        time.sleep(Press_Delay)
##        Sensor_Press_R = GPIO.input(12)
##        print("Your Ground is uneven")
##        #Add screen to show a new paws retract option
##    print("Right Sensor is Good")
##    Ind_Sensor_Press_R     =   True
##        
##def Tripped_Sensor_Press_L():
##    print("Left Pressure Sensor Disengaged")
##    Ind_Sensor_Press_L     =   False
##    while(GPIO.input(7) == False):
##        time.sleep(Press_Delay)
##        Sensor_Press_L = GPIO.input(7)
##        print("Your Ground is uneven")
##        #Add screen to show a new paws retract option
##    print("Right Sensor is Good")
##    Ind_Sensor_Press_L     =   True

##GPIO.add_event_detect(32, GPIO.FALLING, callback=Tripped_Sensor_Brake_R, bouncetime=300)
##GPIO.add_event_detect(13, GPIO.FALLING, callback=Tripped_Sensor_Brake_L, bouncetime=300)
##GPIO.add_event_detect(12, GPIO.FALLING, callback=Tripped_Sensor_Press_R, bouncetime=300)
##GPIO.add_event_detect(7, GPIO.FALLING, callback=Tripped_Sensor_Press_R, bouncetime=300)
    #may need to add event detection during each Goal Mode Process, and deleting at the end of each function using the following
    #GPIO.remove_event_detect(port_number)


##def Brake_Safety_Check():
##    print("no Brakes")
##
##def Press_Safety_Check():
##    print("No Pressure Check")

def Brake_Safety_Check():
    #Written for Normally Open circuit, when sensor reads True the Brakes are engaged
    global Sensor_Brake_R
    global Sensor_Brake_L
    
    print("Initializing Brake Safety Check")
    while(GPIO.input(32) == False or GPIO.input(13) == False):
        print("Brakes are not engaged")
        time.sleep(Brakes_Delay)
        Sensor_Brake_R = GPIO.input(32)
        Sensor_Brake_L = GPIO.input(18)
    print("Brakes Engaged")

def Press_Safety_Check():
    #Written for Normally Open circuit, when sensor reads True the Paws are engaged
    global Sensor_Press_R
    global Sensor_Press_L
    print("Intializing Press Safety Check:")

    while(GPIO.input(12) == False or GPIO.input(7) == False):
        print("Paws are not engaged")
        time.sleep(Press_Delay)
        Sensor_Brake_R = GPIO.input(12)
        Sensor_Brake_L = GPIO.input(7)
    print("Paws Engaged")

def Press_Timer():
    print("Starting the Paw")
    global Paws_Retract_Delay
    global Ind_Paws_Extend
    global Sensor_Press_R
    global Sensor_Press_L
    Sec = 0
    timeLoop = True
    time.sleep(Paws_Extend_Delay)
    #timeLoop = start
    while(GPIO.input(12) == False or GPIO.input(7) == False):
        print("Paws are not engaged")
        Sensor_Brake_R = GPIO.input(12)
        Sensor_Brake_L = GPIO.input(7)
        Sec += 1
        print(str(Sec) + " Secconds of Paw Extention")
        time.sleep(Press_Delay)
    print("Loop Time Finished")
    Paws_Retract_Delay = Sec + 4
    print("New Paws Retract Delay: " + str(Paws_Retract_Delay))
    print("Paws Engaged")
    time.sleep(2) #extra time to ensure paws extend completely

def Display_Initial():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    global Label_Message

##    Button_Mobile.destroy()
##    Button_Stationary.destroy()
##    Button_H_Rails.destroy()
##    Button_Paws.destroy()
##    Button_D_Rails.destroy()
##    Button_Seat.destroy()
##    Button_EStop.destroy()

    print("Setting Ivory on Initializer")
    Button_EStop = Button(modeselect, text=' Skip->Standby ', font = Font1, command = Go_Skip_Standby, bg='#FFFFF2', height = 1, width = 15)
    Button_EStop.grid(row=0,column=0)
    Button_Mobile = Button(modeselect, text=' Mobile ', font = Font1, command = Set_Mobile, bg='#FF00FF', height = 1, width = 15)
    Button_Mobile.grid(row=0,column=1)
    Button_H_Rails = Button(modeselect, text=' Rails FWD ', font = Font1, command = Set_HRails_Extended, bg='#FF00FF', height = 1, width = 15)
    Button_H_Rails.grid(row=0,column=2)
    Button_Paws = Button(modeselect, text=' Paws ', font = Font1, command = Set_Stabilize_Paws, bg='#FF00FF', height = 1, width = 15)
    Button_Paws.grid(row=1,column=0)
    Button_D_Rails = Button(modeselect, text=' Rails Up ', font = Font1, command = Set_DRails_Extended, bg='#FF00FF', height = 1, width = 15)
    Button_D_Rails.grid(row=1,column=1)
    Button_Seat = Button(modeselect, text=' Seat ', font = Font1, command = Set_Seat_Lift, bg='#FF00FF', height = 1, width = 15)
    Button_Seat.grid(row=1,column=2)

    print("ready for use")

#Changes buttons to labels and color to IVORY
def Display_Screenlock():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    global Label_Message

    Button_Mobile.destroy()
    Button_H_Rails.destroy()
    Button_Paws.destroy()
    Button_D_Rails.destroy()
    Button_Seat.destroy()
    Button_EStop.destroy()
    
    Button_Mobile = Label(modeselect, text=' Mobile ', font = Font1, bg='#D3D3D3', height = 1, width = 15)
    Button_Mobile.grid(row=0,column=1)
    Button_H_Rails = Label(modeselect, text=' Rails FWD ', font = Font1, bg='#D3D3D3', height = 1, width = 15)
    Button_H_Rails.grid(row=0,column=2)
    Button_Paws = Label(modeselect, text=' Paws ', font = Font1, bg='#D3D3D3', height = 1, width = 15)
    Button_Paws.grid(row=1,column=0)
    Button_D_Rails = Label(modeselect, text=' Rails Up ', font = Font1, bg='#D3D3D3', height = 1, width = 15)
    Button_D_Rails.grid(row=1,column=1)
    Button_Seat = Label(modeselect, text=' Seat ', font = Font1, bg='#D3D3D3', height = 1, width = 15)
    Button_Seat.grid(row=1,column=2)
    Button_EStop = Label(modeselect, text=' E Stop ', font = Font1, bg='#D3D3D3', height = 1, width = 15)
    Button_EStop.grid(row=0,column=0)

    #When the interrupt is functional the ESTOP button will need to remain a Button
    print("Screen is Locked")
    
    if (Ind_Seat_Rise == True or Ind_Seat_Retract == True):
        Button_Seat.destroy()
        Button_Seat = Label(modeselect, text=' Seat ', font = Font1, bg='#87CEFA', height = 1, width = 15)
        Button_Seat.grid(row=1,column=2)
        print("Seat is Moving")
    
    if (Ind_DRails_Extend == True or Ind_DRails_Retract == True):
        Button_D_Rails.destroy()
        Button_D_Rails = Label(modeselect, text=' Rails Up ', font = Font1, bg='#87CEFA', height = 1, width = 15)
        Button_D_Rails.grid(row=1,column=1)
        print("DRails are Moving")
    
    if (Ind_Paws_Extend == True or Ind_Paws_Retract == True):
        Button_Paws.destroy()
        Button_Paws = Label(modeselect, text=' Paws ', font = Font1, bg='#87CEFA', height = 1, width = 15)	
        Button_Paws.grid(row=1,column=0)
        print("Paws are Moving")
    
    if Ind_HRails_Extend == True:
        Button_H_Rails.destroy()
        Button_H_Rails = Label(modeselect, text=' Rails FWD ', font = Font1, bg='#87CEFA', height = 1, width = 15)
        Button_H_Rails.grid(row=0,column=2)
        print("HRails are Moving")
    


##    print("Checking for Goal Changing to GREEN")
##    if Goal_Mode == "Mobile":
##        Button_Mobile.destroy()
##        Button_Mobile = Label(modeselect, text= ' Mobile ', font = Font1, bg='#008000', height = 1, width = 10)
##        Button_Mobile.grid(row=0,column=0)
##    elif  Goal_Mode == "Stationary":
##        Button_Stationary.destroy()
##        Button_Stationary = Label(modeselect, text=' Stationary ', font = Font1, bg='#008000', height = 1, width = 10)
##        Button_Stationary.grid(row=0,column=1)
##    elif  Goal_Mode == "Rails_Forward":
##        Button_H_Rails.destroy()
##        Button_H_Rails = Label(modeselect, text=' Rails FWD ', font = Font1, bg='#008000', height = 1, width = 10)
##        Button_H_Rails.grid(row=0,column=2)
##    elif  Goal_Mode == "Stabilized":
##        Button_Paws.destroy()
##        Button_Paws = Label(modeselect, text=' Paws ', font = Font1, bg='#008000', height = 1, width = 10)
##        Button_Paws.grid(row=1,column=0)
##    elif  Goal_Mode == "Rails_Up":
##        Button_D_Rails.destroy()
##        Button_D_Rails = Label(modeselect, text=' D_Rails ', font = Font1, bg='#008000', height = 1, width = 10)
##        Button_D_Rails.grid(row=1,column=1)
##    elif  Goal_Mode == "Seat_Lifted":
##        Button_Seat.destroy()
##        Button_Seat = Label(modeselect, text=' Seat ', font = Font1, bg='#008000', height = 1, width = 10)
##        Button_Seat.grid(row=1,column=2)
##    else:
##        print("Error in Goal to GREEN")

    if  Previous_Mode == "Mobile":
        Button_Mobile.destroy()
        Button_Mobile = Label(modeselect, text= ' Mobile ', font = Font1, bg='#B22222', height = 1, width = 15)
        Button_Mobile.grid(row=0,column=1)
    elif  Previous_Mode == "Rails_Forward":
        Button_H_Rails.destroy()
        Button_H_Rails = Label(modeselect, text=' Rails FWD ', font = Font1, bg='#B22222', height = 1, width = 15)
        Button_H_Rails.grid(row=0,column=2)
    elif  Previous_Mode == "Stabilized":
        Button_Paws.destroy()
        Button_Paws = Label(modeselect, text=' Paws ', font = Font1, bg='#B22222', height = 1, width = 15)
        Button_Paws.grid(row=1,column=0)
    elif  Previous_Mode == "Rails_Up":
        Button_D_Rails.destroy()
        Button_D_Rails = Label(modeselect, text=' Rails Up ', font = Font1, bg='#B22222', height = 1, width = 15)
        Button_D_Rails.grid(row=1,column=1)
    elif  Previous_Mode == "Seat_Lifted":
        Button_Seat.destroy()
        Button_Seat = Label(modeselect, text=' Seat ', font = Font1, bg='#B22222', height = 1, width = 15)
        Button_Seat.grid(row=1,column=2)
    else:
        print ("Error in Previous to RED")
    print("Standing By")


def Display_Standby():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    global Label_Message

    Button_Mobile.destroy()
    Button_H_Rails.destroy()
    Button_Paws.destroy()
    Button_D_Rails.destroy()
    Button_Seat.destroy()
    Button_EStop.destroy()

    Button_EStop = Button(modeselect, text=' Emergency Stop ', font = Font1, command = Go_EStop, bg='#FFFF00', height = 1, width = 15)
    Button_EStop.grid(row=0,column=0)
    Button_Mobile = Button(modeselect, text=' Mobile ', font = Font1, command = Go_Mobile, bg='#FFFFF0', height = 1, width = 15)
    Button_Mobile.grid(row=0,column=1)
    Button_H_Rails = Button(modeselect, text=' Rails FWD ', font = Font1, command = Go_HRails_Extend, bg='#FFFFF0', height = 1, width = 15)
    Button_H_Rails.grid(row=0,column=2)
    Button_Paws = Button(modeselect, text=' Paws ', font = Font1, command = Go_Stabilize_Paws, bg='#FFFFF0', height = 1, width = 15)
    Button_Paws.grid(row=1,column=0)
    Button_D_Rails = Button(modeselect, text=' Rails Up ', font = Font1, command = Go_DRails_Extend, bg='#FFFFF0', height = 1, width = 15)
    Button_D_Rails.grid(row=1,column=1)
    Button_Seat = Button(modeselect, text=' Seat ', font = Font1, command = Go_Seat_Lift, bg='#FFFFF0', height = 1, width = 15)
    Button_Seat.grid(row=1,column=2)

    
    if (Ind_Seat_Rise == True or Ind_Seat_Retract == True):
        Button_Seat.destroy()
        Button_Seat = Button(modeselect, text=' Seat ', font = Font1, command = Go_Seat_Lift, bg='#87CEFA', height = 1, width = 15)
        Button_Seat.grid(row=1,column=2)
        print("Seat is Moving")
    
    if (Ind_DRails_Extend == True or Ind_DRails_Retract == True):
        Button_D_Rails.destroy()
        Button_D_Rails = Button(modeselect, text=' Rails Up ', font = Font1, command = Go_DRails_Extend, bg='#87CEFA', height = 1, width = 15)
        Button_D_Rails.grid(row=1,column=1)
        print("DRails are Moving")
    
    if (Ind_Paws_Extend == True or Ind_Paws_Retract == True):
        Button_Paws.destroy()
        Button_Paws = Button(modeselect, text=' Paws ', font = Font1, command = Go_Stabilize_Paws, bg='#87CEFA', height = 1, width = 15)	
        Button_Paws.grid(row=1,column=0)
        print("Paws are Moving")
    
    if Ind_HRails_Extend == True:
        Button_H_Rails.destroy()
        Button_H_Rails = Button(modeselect, text=' Rails FWD ', font = Font1, command = Go_HRails_Extend, bg='#87CEFA', height = 1, width = 15)
        Button_H_Rails.grid(row=0,column=2)
        print("HRails are Moving")

##    print("Checking for Goal Changing to GREEN")
##    if Goal_Mode == "Mobile":
##        Button_Mobile.destroy()
##        Button_Mobile = Button(modeselect, text= ' Mobile ', font = Font1, command = Go_Mobile, bg='#008000', height = 1, width = 10)
##        Button_Mobile.grid(row=0,column=0)
##    elif  Goal_Mode == "Stationary":
##        Button_Stationary.destroy()
##        Button_Stationary = Button(modeselect, text=' Stationary ', font = Font1, command = Go_Stationary, bg='#008000', height = 1, width = 10)
##        Button_Stationary.grid(row=0,column=1)
##    elif  Goal_Mode == "Rails_Forward":
##        Button_H_Rails.destroy()
##        Button_H_Rails = Button(modeselect, text=' Rails FWD ', font = Font1, command = Go_HRails_Extend, bg='#008000', height = 1, width = 10)
##        Button_H_Rails.grid(row=0,column=2)
##    elif  Goal_Mode == "Stabilized":
##        Button_Paws.destroy()
##        Button_Paws = Button(modeselect, text=' Paws ', font = Font1, command = Go_Stabilize_Paws, bg='#008000', height = 1, width = 10)
##        Button_Paws.grid(row=1,column=0)
##    elif  Goal_Mode == "Rails_Up":
##        Button_D_Rails.destroy()
##        Button_D_Rails = Button(modeselect, text=' Rails Up ', font = Font1, command = Go_DRails_Extend, bg='#008000', height = 1, width = 10)
##        Button_D_Rails.grid(row=1,column=1)
##    elif  Goal_Mode == "Seat_Lifted":
##        Button_Seat.destroy()
##        Button_Seat = Button(modeselect, text=' Seat ', font = Font1, command = Go_Seat_Lift, bg='#008000', height = 1, width = 10)
##        Button_Seat.grid(row=1,column=2)
##    else:
##        print("Error in Goal to GREEN")

    if  Previous_Mode == "Mobile":
        Button_Mobile.destroy()
        Button_Mobile = Button(modeselect, text= ' Mobile ', font = Font1, command = Go_Mobile, bg='#B22222', height = 1, width = 15)
        Button_Mobile.grid(row=0,column=1)
    elif  Previous_Mode == "Rails_Forward":
        Button_H_Rails.destroy()
        Button_H_Rails = Button(modeselect, text=' Rails FWD ', font = Font1, command = Go_HRails_Extend, bg='#B22222', height = 1, width = 15)
        Button_H_Rails.grid(row=0,column=2)
    elif  Previous_Mode == "Stabilized":
        Button_Paws.destroy()
        Button_Paws = Button(modeselect, text=' Paws ', font = Font1, command = Go_Stabilize_Paws, bg='#B22222', height = 1, width = 15)
        Button_Paws.grid(row=1,column=0)
    elif  Previous_Mode == "Rails_Up":
        Button_D_Rails.destroy()
        Button_D_Rails = Button(modeselect, text=' Rails Up ', font = Font1, command = Go_DRails_Extend, bg='#B22222', height = 1, width = 15)
        Button_D_Rails.grid(row=1,column=1)
    elif  Previous_Mode == "Seat_Lifted":
        Button_Seat.destroy()
        Button_Seat = Button(modeselect, text=' Seat ', font = Font1, command = Go_Seat_Lift, bg='#B22222', height = 1, width = 15)
        Button_Seat.grid(row=1,column=2)
    else:
        print ("Error in Previous to RED")
    print("Standing By")
        
#MAIN PROGRAM LOGICS, needs addition of Sensory inputs from Pressure Sensor and testing of Brake Sensor Program####################################################
def Go_Mobile():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay
    global Remainder_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop

    Goal_Mode = "Mobile"
    Display_Screenlock()
    if  Previous_Mode == "Mobile":
        print("Redundant Command")

    elif  Previous_Mode == "Stationary":
        print("Release your Brakes")

    elif  Previous_Mode == "Rails_Forward":
        Actuate_HRails_Retract()
        print("Release your Brakes")

    elif  Previous_Mode == "Stabilized":
        Actuate_Paws_Retract()
        Actuate_HRails_Retract()
        print("Release your Brakes")

    elif  Previous_Mode == "Rails_Up":
        Actuate_DRails_Retract()
        Actuate_Paws_Retract()
        Actuate_HRails_Retract()
        print("Release your Brakes")

    elif  Previous_Mode == "Seat_Lifted":
        Actuate_Seat_Retract()
        Actuate_DRails_Retract()
        Actuate_Paws_Retract()
        Actuate_HRails_Retract()
        print("Release your Brakes")

    else:
        print ("Error in Case Selection")

    print("Returning to Standby")
    Goal_Mode = ""
    Previous_Mode = "Mobile"
    Display_Standby()

def Go_Stationary():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop

    Goal_Mode = "Stationary"
    Display_Screenlock()

    if  Previous_Mode == "Mobile":
        print("Engage your Brakes")

    elif  Previous_Mode == "Stationary":
        print("Redundant Command")

    elif  Previous_Mode == "Rails_Forward":
        Actuate_HRails_Retract()
        print("Release your Brakes")

    elif  Previous_Mode == "Stabilized":
        Actuate_Paws_Retract()
        Actuate_HRails_Retract()
        print("Release your Brakes")

    elif  Previous_Mode == "Rails_Up":
        Actuate_DRails_Retract()
        Actuate_Paws_Retract()
        Actuate_HRails_Retract()
        print("Release your Brakes")

    elif  Previous_Mode == "Seat_Lifted":
        Actuate_Seat_Retract()
        Actuate_DRails_Retract()
        Actuate_Paws_Retract()
        Actuate_HRails_Retract()
        print("Release your Brakes")

    else:
        print ("Error in Case Selection")

    print("Returning to Standby")
    Goal_Mode = ""
    Previous_Mode = "Stationary"
    Display_Standby()

def Go_HRails_Extend():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop

    Goal_Mode = "H_Rails"
    Display_Screenlock()

    if Previous_Mode == "Mobile":
        print("Engage your Brakes")
        Actuate_HRails_Extend()

    elif Previous_Mode == "Stationary":
        Actuate_HRails_Extend()

    elif  Previous_Mode == "Rails_Forward":
        print("Redundant Command")

    elif  Previous_Mode == "Stabilized":
        Actuate_Paws_Retract()
        
    elif  Previous_Mode == "Rails_Up":
        Actuate_DRails_Retract()
        Actuate_Paws_Retract()

    elif  Previous_Mode == "Seat_Lifted":
        Actuate_Seat_Retract()
        Actuate_DRails_Retract()
        Actuate_Paws_Retract()

    else:
        print ("Error in Case Selection")

    print("Returning to Standby")
    Goal_Mode = ""
    Previous_Mode = "Rails_Forward"
    Display_Standby()

def Go_Stabilize_Paws():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop

    Goal_Mode = "Paws"
    Display_Screenlock()

    if  Previous_Mode == "Mobile":
        Actuate_HRails_Extend()
        Actuate_Paws_Extend()

    elif  Previous_Mode == "Stationary":
        Actuate_HRails_Extend()
        Actuate_Paws_Extend()

    elif  Previous_Mode == "Rails_Forward":
        Actuate_Paws_Extend()

    elif  Previous_Mode == "Stabilized":
        print("Redundant Command")

    elif  Previous_Mode == "Rails_Up":
        Actuate_DRails_Retract()

    elif  Previous_Mode == "Seat_Lifted":
        Actuate_Seat_Retract()
        Actuate_DRails_Retract()

    else:
        print ("Error in Case Selection")

    print("Returning to Standby")
    Goal_Mode = ""
    Previous_Mode = "Stabilized"
    Display_Standby()

def Go_DRails_Extend():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop

    Goal_Mode = "D_Rails"
    Display_Screenlock()
    Display_State = "Lockout"
    Display_Green_Goal()

    if  Previous_Mode == "Mobile":
        Actuate_HRails_Extend()
        Actuate_Paws_Extend()
        Actuate_DRails_Extend()

    elif  Previous_Mode == "Stationary":
        Actuate_HRails_Extend()
        Actuate_Paws_Extend()
        Actuate_DRails_Extend()
        
    elif  Previous_Mode == "Rails_Forward":
        Actuate_Paws_Extend()
        Actuate_DRails_Extend()
        
    elif  Previous_Mode == "Stabilized":
        Actuate_DRails_Extend()

    elif  Previous_Mode == "Rails_Up":
        print("Redundant Command")

    elif  Previous_Mode == "Seat_Lifted":
        Actuate_Seat_Retract()

    else:
        print ("Error in Case Selection")

    print("Returning to Standby")
    Goal_Mode = ""
    Previous_Mode = "Rails_Up"
    Display_Standby()

def Go_Seat_Lift():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop

    Goal_Mode = "Seat"
    Display_Screenlock()

    if  Previous_Mode == "Mobile":
        Actuate_HRails_Extend()
        Actuate_Paws_Extend()
        Actuate_DRails_Extend()
        Actuate_Seat_Extend()

    elif  Previous_Mode == "Stationary":
        Actuate_HRails_Extend()
        Actuate_Paws_Extend()
        Actuate_DRails_Extend()
        Actuate_Seat_Extend()

    elif  Previous_Mode == "Rails_Forward":
        Actuate_Paws_Extend()
        Actuate_DRails_Extend()
        Actuate_Seat_Extend()

    elif  Previous_Mode == "Stabilized":
        Actuate_DRails_Extend()
        Actuate_Seat_Extend()

    elif  Previous_Mode == "Rails_Up":
        Actuate_Seat_Extend()

    elif  Previous_Mode == "Seat_Lifted":
        print("Redundant Command")

    else:
        print ("Error in Case Selection")

    print("Returning to Standby")
    Goal_Mode = ""
    Previous_Mode = "Seat_Lifted"
    Display_Standby()

def Go_EStop():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    #This Program Forces the Initializer Screen
    if Ind_Seat_Rise 	== True:
        Resume_Mode = "Seat_Rise"
    elif Ind_Seat_Retract 	== True:
        Resume_Mode = "Seat_Retract"
    elif Ind_DRails_Extend 	== True:
        Resume_Mode = "DRails_Extend"
    elif Ind_DRails_Retract 	== True:
        Resume_Mode = "DRails_Retract"
    elif Ind_Paws_Extend 	== True:
        Resume_Mode = "Paws_Extend"
    elif Ind_Paws_Retract 	== True:
        Resume_Mode = "Paws_Retract"
    elif Ind_HRails_Extend 	== True:
        Resume_Mode = "HRails_Extend"
    elif Ind_HRails_Retract 	== True:
        Resume_Mode = "HRails_Retract"
    else:
        print("Error in EStop Current Mode Selector")
    Goal_Mode = ""
    Previous_Mode = ""
    Actuators_Off()
    Current_Mode = Resume_Mode
    Display_Initial()

##def Set_Mobile():
##    global Previous_Mode
##    Display_Screenlock()
##    Actuate_Seat_Retract()
##    Actuate_DRails_Retract()
##    Actuate_Paws_Retract()
##    Actuate_HRails_Retract()
##
##    Previous_Mode = "Mobile"
##    print(Previous_Mode)
##    Display_Standby()
##
##def Set_Stationary():
##    global Previous_Mode
##    Display_Screenlock()
##    
##    Actuate_Seat_Retract()
##    Actuate_DRails_Retract()
##    Actuate_Paws_Retract()
##    Actuate_HRails_Retract()
##
##    Previous_Mode = "Stationary"
##    print(Previous_Mode)
##    Display_Standby()
##
##def Set_HRails_Extended():
##    global Previous_Mode
##    Display_Screenlock()
##    
##    Actuate_Seat_Retract()
##    Actuate_DRails_Retract()
##    Actuate_Paws_Retract()
##    Actuate_HRails_Retract()
##    
##    Actuate_HRails_Extend()
##
##    Previous_Mode = "Rails_Forward"
##    print(Previous_Mode)
##    Display_Standby()
##
##def Set_Stabilize_Paws():
##    global Previous_Mode
##    Display_Screenlock()
##
##    Actuate_Seat_Retract()
##    Actuate_DRails_Retract()
##    Actuate_Paws_Retract()
##    Actuate_HRails_Retract()
##
##    Actuate_HRails_Extend()
##    Actuate_Paws_Extend()
##
##
##    Previous_Mode = "Stabilized"
##    print(Previous_Mode)
##    Display_Standby()
##
##def Set_DRails_Extended():
##    global Previous_Mode
##    Display_Screenlock()
##    
##    Actuate_Seat_Retract()
##    Actuate_Paws_Retract()
##    Actuate_HRails_Retract()
##
##    Actuate_HRails_Extend()
##    Actuate_Paws_Extend()
##    Actuate_DRails_Extend()
##    
##    Previous_Mode = "Rails_Up"
##    print(Previous_Mode)
##    Display_Standby()
##
##def Set_Seat_Lift():
##    global Previous_Mode
##    Display_Screenlock()
##
##    Actuate_Paws_Retract()
##    Actuate_HRails_Retract()
##
##    Actuate_HRails_Extend()
##    Actuate_Paws_Extend()
##    Actuate_DRails_Extend()
##    Actuate_Seat_Extend()
##    
##    Previous_Mode = 'Seat_Lifted'
##    print(Previous_Mode)
##    Display_Standby()

def Go_Nothing():
    print("this is a Dummy Button")

def Go_Skip_Standby():
    print("Skipping to Standby")
    Display_Standby()

def Actuate_Seat_Extend():
    global Ind_Seat_Rise
    global Seat_Rise
    global Seat_Rise_Delay
    Brake_Safety_Check()
    Press_Safety_Check()

    Display_Standby()
    print("Actuating Seat Lift")
    Ind_Seat_Rise 	= True
    GPIO.output(Seat_Rise, GPIO.LOW)
    time.sleep(Seat_Rise_Delay)
    Actuators_Off()
    Display_Standby()
    
def Actuate_Seat_Retract():
    global Seat_Retract
    global Ind_Seat_Retract
    global Seat_Retract_Delay
    Brake_Safety_Check()
    Press_Safety_Check()

    Display_Standby()
    print("Actuating Seat Lift")
    Ind_Seat_Retract 	= True
    GPIO.output(Seat_Retract, GPIO.LOW)
    time.sleep(Seat_Retract_Delay)
    Actuators_Off()
    Display_Standby()
    
def Actuate_DRails_Extend():
    global Ind_DRails_Extend
    global DRails_Extend
    global DRails_Extend_Delay
    Brake_Safety_Check()
    Press_Safety_Check()

    Display_Standby()
    print("Actuating Diagonal Rails")
    Ind_DRails_Extend 	= True
    GPIO.output(DRails_Extend, GPIO.LOW)
    time.sleep(DRails_Extend_Delay)
    Actuators_Off()
    Display_Standby()
        
def Actuate_DRails_Retract():
    global Ind_DRails_Retract
    global DRails_Retract
    global DRails_Retract_Delay
    Brake_Safety_Check()
    Press_Safety_Check()

    Display_Standby()
    print("Actuating Diagonal Rails")
    Ind_DRails_Retract 	= True
    GPIO.output(DRails_Retract, GPIO.LOW)
    time.sleep(DRails_Retract_Delay)
    Actuators_Off()
    Display_Standby()
        
def Actuate_Paws_Extend():
    global Ind_Paws_Extend
    global Paws_Extend
    global Paws_Extend_Delay
    Brake_Safety_Check()
    print("Actuating Paws")

    Display_Standby()
    Ind_Paws_Extend 	= True
    GPIO.output(Paws_Extend, GPIO.LOW)
    Press_Timer()
    Actuators_Off()
    Display_Standby()
        
def Actuate_Paws_Retract():
    global Ind_Paws_Retract
    global Paws_Retract
    global Paws_Retract_Delay
    Brake_Safety_Check()
    print("Actuating Paws")
    
    Display_Standby()
    Ind_Paws_Retract 	= True
    GPIO.output(Paws_Retract, GPIO.LOW)
    time.sleep(Paws_Retract_Delay)
    Actuators_Off()
    Display_Standby()

def Actuate_HRails_Extend():
    global Ind_HRails_Extend
    global HRails_Extend
    global HRails_Extend_Delay
    Brake_Safety_Check()
    print("Actuating Horizontal Rails")

    Display_Standby()
    Ind_HRails_Extend 	= True
    GPIO.output(HRails_Extend, GPIO.LOW)
    time.sleep(HRails_Extend_Delay)
    Actuators_Off()
    Display_Standby()
        
def Actuate_HRails_Retract():
    global Ind_HRails_Retract
    global HRails_Retract
    global HRails_Retract_Delay
    Brake_Safety_Check()
    print("Actuating Horizontal Rails")

    Display_Standby()
    Ind_HRails_Retract 	= True
    GPIO.output(HRails_Retract, GPIO.LOW)
    time.sleep(HRails_Retract_Delay)
    Actuators_Off()
    Display_Standby()

#Set Programs refer to the Display_Initial Screen

def Set_Mobile():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    Previous_Mode = 'Mobile'
    print("Hands Off")
    
    Ind_Seat_Retract 	= True
    GPIO.output(Seat_Retract, GPIO.LOW)
    time.sleep(Seat_Retract_Delay)
    Actuators_Off()

    Ind_DRails_Retract 	= True
    GPIO.output(DRails_Retract, GPIO.LOW)
    time.sleep(DRails_Retract_Delay)
    Actuators_Off()

    Ind_Paws_Retract 	= True
    GPIO.output(Paws_Retract, GPIO.LOW)
    time.sleep(Paws_Retract_Delay)
    Actuators_Off()

    Ind_HRails_Retract 	= True
    GPIO.output(HRails_Retract, GPIO.LOW)
    time.sleep(HRails_Retract_Delay)
    Actuators_Off()
    print("Release your Brakes")
    print("setting Previous Mode")
    Previous_Mode = 'Mobile'
    print(Previous_Mode)
    Display_Standby()


    
def Set_Stationary():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    Previous_Mode = 'Stationary'
    print("Hands Off")
    
    Ind_Seat_Retract 	= True
    GPIO.output(Seat_Retract, GPIO.LOW)
    time.sleep(Seat_Retract_Delay)
    Actuators_Off()

    Ind_DRails_Retract 	= True
    GPIO.output(DRails_Retract, GPIO.LOW)
    time.sleep(DRails_Retract_Delay)
    Actuators_Off()

    Ind_Paws_Retract 	= True
    GPIO.output(Paws_Retract, GPIO.LOW)
    time.sleep(Paws_Retract_Delay)
    Actuators_Off()

    Ind_HRails_Retract 	= True
    GPIO.output(HRails_Retract, GPIO.LOW)
    time.sleep(HRails_Retract_Delay)
    Actuators_Off()
    print("setting Previous Mode")
    Previous_Mode = 'Stationary'
    print(Previous_Mode)
    Display_Standby()


def Set_HRails_Extended():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    Previous_Mode = 'Rails_Forward'
    print("Hands Off")
    
    Ind_Seat_Retract 	= True
    GPIO.output(Seat_Retract, GPIO.LOW)
    time.sleep(Seat_Retract_Delay)
    Actuators_Off()

    Ind_DRails_Retract 	= True
    GPIO.output(DRails_Retract, GPIO.LOW)
    time.sleep(DRails_Retract_Delay)
    Actuators_Off()

    Ind_Paws_Retract 	= True
    GPIO.output(Paws_Retract, GPIO.LOW)
    time.sleep(Paws_Retract_Delay)
    Actuators_Off()

    Ind_HRails_Retract 	= True
    GPIO.output(HRails_Retract, GPIO.LOW)
    time.sleep(HRails_Retract_Delay)
    Actuators_Off()
    
    Ind_HRails_Extend 	= True
    GPIO.output(HRails_Extend, GPIO.LOW)
    time.sleep(HRails_Extend_Delay)
    Actuators_Off()
    print("setting Previous Mode")
    Previous_Mode = 'Rails_Forward'
    print(Previous_Mode)
    Display_Standby()


def Set_Stabilize_Paws():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    Previous_Mode = 'Stabilized'
    print("Hands Off")
    
    Ind_Seat_Retract 	= True
    GPIO.output(Seat_Retract, GPIO.LOW)
    time.sleep(Seat_Retract_Delay)
    Actuators_Off()

    Ind_DRails_Retract 	= True
    GPIO.output(DRails_Retract, GPIO.LOW)
    time.sleep(DRails_Retract_Delay)
    Actuators_Off()

    Ind_Paws_Retract 	= True
    GPIO.output(Paws_Retract, GPIO.LOW)
    time.sleep(Paws_Retract_Delay)
    Actuators_Off()

    Ind_HRails_Retract 	= True
    GPIO.output(HRails_Retract, GPIO.LOW)
    time.sleep(HRails_Retract_Delay)
    Actuators_Off()
    
    Ind_HRails_Extend 	= True
    GPIO.output(HRails_Extend, GPIO.LOW)
    time.sleep(HRails_Extend_Delay)
    Actuators_Off()

    Ind_Paws_Extend 	= True
    GPIO.output(Paws_Extend, GPIO.LOW)
    time.sleep(Paws_Extend_Delay)
    Press_Safety_Check()
    Actuators_Off()
    print("setting Previous Mode")
    Previous_Mode = 'Stabilized'
    print(Previous_Mode)
    Display_Standby()


def Set_DRails_Extended():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    Previous_Mode = 'Rails_Up'

    Ind_Seat_Retract 	= True
    GPIO.output(Seat_Retract, GPIO.LOW)
    time.sleep(Seat_Retract_Delay)
    Actuators_Off()

    Ind_Paws_Retract 	= True
    GPIO.output(Paws_Retract, GPIO.LOW)
    time.sleep(Paws_Retract_Delay)
    Actuators_Off()

    Ind_HRails_Retract 	= True
    GPIO.output(HRails_Retract, GPIO.LOW)
    time.sleep(HRails_Retract_Delay)
    Actuators_Off()

    Ind_HRails_Extend 	= True
    GPIO.output(HRails_Extend, GPIO.LOW)
    time.sleep(HRails_Extend_Delay)
    Actuators_Off()

    Ind_Paws_Extend 	= True
    GPIO.output(Paws_Extend, GPIO.LOW)
    time.sleep(Paws_Extend_Delay)
    Press_Safety_Check()
    Actuators_Off()

    Ind_DRails_Extend 	= True
    GPIO.output(DRails_Extend, GPIO.LOW)
    time.sleep(DRails_Extend_Delay)
    Actuators_Off()
    print("setting Previous Mode")
    Previous_Mode = 'Rails_Up'
    print(Previous_Mode)
    Display_Standby()

    
def Set_Seat_Lift():
    global Previous_Mode
    global Resume_Mode
    global Goal_Mode
    global Display_State

    global Seat_Rise
    global Seat_Retract
    global DRails_Extend
    global DRails_Retract
    global Paws_Extend
    global Paws_Retract
    global HRails_Extend
    global HRails_Retract

    global Seat_Rise_Delay
    global Seat_Retract_Delay
    global DRails_Extend_Delay
    global DRails_Retract_Delay
    global Paws_Extend_Delay
    global Paws_Retract_Delay
    global HRails_Extend_Delay
    global HRails_Retract_Delay

    global Button_Mobile
    global Button_Stationary
    global Button_H_Rails
    global Button_Paws
    global Button_D_Rails
    global Button_Seat
    global Button_EStop
    Previous_Mode = 'Seat_Lifted'

    Ind_Paws_Retract 	= True
    GPIO.output(Paws_Retract, GPIO.LOW)
    time.sleep(Paws_Retract_Delay)
    Actuators_Off()

    Ind_HRails_Retract 	= True
    GPIO.output(HRails_Retract, GPIO.LOW)
    time.sleep(HRails_Retract_Delay)
    Actuators_Off()

    Ind_HRails_Extend 	= True
    GPIO.output(HRails_Extend, GPIO.LOW)
    time.sleep(HRails_Extend_Delay)
    Actuators_Off()

    Ind_Paws_Extend 	= True
    GPIO.output(Paws_Extend, GPIO.LOW)
    time.sleep(Paws_Extend_Delay)
    Press_Safety_Check()
    Actuators_Off()

    Ind_DRails_Extend 	= True
    GPIO.output(DRails_Extend, GPIO.LOW)
    time.sleep(DRails_Extend_Delay)
    Actuators_Off()

    Ind_Seat_Rise 	= True
    GPIO.output(Seat_Rise, GPIO.LOW)
    time.sleep(Seat_Rise_Delay)
    Actuators_Off()
    print("setting Previous Mode")
    Previous_Mode = 'Seat_Lifted'
    print(Previous_Mode)
    Display_Standby()

Display_Initial()
root.mainloop()

