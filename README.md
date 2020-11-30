# alarm_pi_oders_you_to_get_up
# raspberrypi/alarm.py
# raspberrypi alarm clock
# 2014, Ismail Uddin
# www.scienceexposure.com

import time
import RPi.GPIO as GPIO
from playsound import playsound
from sense_hat import SenseHat

sense = SenseHat()# erstellt ein eines sensehat object

response = raw_input("Please input the time for the alarm in format HHMM: \n")

print("Alarm has been set for %s hrs" % response)

awake = 0
extratime = 5

try:
    while True:
        curr_time = int(time.strftime("%H%M"))  # continuesly checking time
        time.sleep(0.2)  # break from checking to avoid overheating of chip

        # alarm goes off at set time
        if curr_time == alarm:
            awake = 1
            while awake <= 1:  # alarm goes off
                # put and play song here
                if shake_methode == True:  # computer is shaken
                    awake = 2
                if magnet_methode == True:  # computer notices magent
                    awake = 3

        if awake == 2:  #computer has been shaken
            while count == 1:
                alarm += extratime  # extratime of minutes more sleep
                print('\n you recieved '+(str(extratime))+' extra minutes!!')
                # put verbal annoucement here
                count = 2
                if curr_time == alarm and count == 2:  # alarm starts again
                    # song here
                    if magnet_methode == True:  # door opens and person got up!!!
                        awake = 3

        if awake == 3:
            GPIO.cleanup()
            print("good morning")
            GPIO.cleanup()
            break
finally:
    print("have a nice day")
