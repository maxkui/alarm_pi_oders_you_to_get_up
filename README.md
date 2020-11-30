# alarm_pi_oders_you_to_get_up
# raspberrypi/alarm.py
# raspberrypi alarm clock
# 2014, Ismail Uddin
# www.scienceexposure.com

import time
import RPi.GPIO as GPIO
from playsound import playsound
import siren

GPIO.setmode(GPIO.BCM)
GPIO.setup(25, GPIO.IN, pull_up_down=GPIO.PUD_UP)

response = raw_input("Please input the time for the alarm in format HHMM: \n")

print("Alarm has been set for %s hrs" % response)

awake = 0

try:
    while True:
        # Continually get's time as an integer number
        curr_time = int(time.strftime("%H%M"))

        # alarm goes off at set time
        if curr_time == alarm:
            siren.siren_start()
            playsound('nameoffile')

            # put song here
            # put in siren.py

        # button is hit
        if GPIO.input(23) == 0 and awake == 1:
            alarm += 7
            awake = 0

        # if alarm not snoozed and button not hit, then alarm
        # is changed to current time to ensure that the alarm
        # continues until door opens
        elif curr_time != alarm and awake == 1:
            alarm = curr_time
