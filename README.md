# alarm_pi_oders_you_to_get_up
# raspberrypi/alarm.py
# raspberrypi alarm clock
# 2014, Ismail Uddin
# www.scienceexposure.com

#!/usr/bin/python
import time
from time import strftime
import vlc
import random
from sense_hat import SenseHat

sense = SenseHat()
sense.set_rotation(180) #possible values are 0, 90, 180, 270
sense.set_imu_config(False, False, True)  # only Accelerometer is enabled

count = 1  # this variable is used later on
time_start_song = int  # this variable is used later
wait = 1
awake = 0  # another variable that is used later

alarm = int(input("Please input the time for the alarm in format HHMM: \n"))
print("Alarm has been set for %s hrs" % alarm)

wakeup_songs = ["Don't Stop Me Now.mp3", "La vie en rose - Louis Armstrong.mp3",
                "MGMT- Kids.mp3", "Stand By Me, Ben E King, 1961.mp3",
                "buddy.mp3", "campfire.mp3", "simple.mp3", "starglight lounge.mp3",
                "Dancing In the Moonlight.mp3", "04 Schütze Mich.mp3",
                "01 Frank Sinatra - My Way.mp3", "Brooke Fraser - Something In The Water [Official Video HD].mp3",
                "I Dreamed a Dream.mp3", "Louis Armstrong And His Hot Five - Struttin' With Some Barbecue.mp3",
                "Louis Armstrong - What A Wonderful World (HQ Audio).mp3",
                "RIQK3ilhVqm9.128.mp3"]

# media = vlc.MediaPlayer("/home/pi/songs_and_sounds/" + random.choice(wakeup_songs))

alarm_sounds = ["alarm clock bell.mp3", "alarm normal.mp3",
                "antique clock.mp3", "grandfather clock strike.mp3",
                "rooster.mp3", "cartoon party hoorn.mp3"]
# media = vlc.MediaPlayer("/home/pi/songs_and_sounds/alarm/" + random.choice(alarm_sounds))
celebration_sounds = ["arena crowd cheer.mp3", "cartoon boing.mp3",
                      "cartoon party horn.mp3", "clapping crowd studio 03.mp3", "kids cheering.mp3",
                      "recital crowd applause.mp3", "My Movie 13.mp3"]
# media = vlc.MediaPlayer("/home/pi/songs_and_sounds/alarm/" + random.choice(celebration_sounds))
GPIO.add_event_detect(wand_button, GPIO.RISING, callback=my_callback, bouncetime=200)
GPIO.add_event_detect(MAGNET_GPIO, GPIO.FALLING, bouncetime=200)
# these lines detect interruptions/signals from the buttons simulateneously
# as the program is running, which ensures that a button activation/press is detected at all times

#------------------------------------- Functions for alarm -------------------------------------#
def shakeit():
    #methode die bemerkt wenn geschüttelt wird

def coolthebeer(): #methode die bemerkt wenn wecker im Kühlschrank
    from sense_hat import SenseHat
    import time

    while True:
    sense = SenseHat()
    sense.clear()

    temp = sense.get_temperature()      
    print(temp)
    time.sleep(5)                     #Temperatur wird alle 5s gemessen
    if temp <= 10:                 #temperatur kleiner als 10 Grad, line 175 soll ausgelöst werden
        coolthebeer() = true

def heiligenummber(farbliste):
    maximsnummer = random.randrange(0, len(farbliste), 1)
    if maximsnummer == 0:
        maximsnummer = 3
    return (maximsnummer)
def countdown():
    for i in reversed(range(0, 10)):
        sense.show_letter(str(i), text_colour=[255, 255, 255], back_colour=[0, 0, 0])
        time.sleep(1)
    sense.clear()
    return
def getthediscostarted():
    #methode die die LEDS des Pi-Hats als visuelles Weckerelement benutzt
    # r g b
    g = (0, 255, 0)
    y = (255, 255, 0)
    b = (0, 0, 255)
    r = (255, 0, 0)
    w = (255, 255, 255)
    n = (0, 0, 0)
    p = (255, 105, 180)
    counter = 0
    farbliste = [g, y, b, r, w, p]

    farbe1 = farbliste[heiligenummber(farbliste)]
    farbe2 = farbliste[heiligenummber(farbliste)]
    farbe3 = farbliste[heiligenummber(farbliste)]
    farbe4 = farbliste[heiligenummber(farbliste)]

    disco = [
        farbe1, farbe1, farbe1, farbe1, farbe2, farbe2, farbe2, farbe2,
        farbe1, farbe1, farbe1, farbe1, farbe2, farbe2, farbe2, farbe2,
        farbe1, farbe1, farbe1, farbe1, farbe2, farbe2, farbe2, farbe2,
        farbe1, farbe1, farbe1, farbe1, farbe2, farbe2, farbe2, farbe2,
        farbe3, farbe3, farbe3, farbe3, farbe4, farbe4, farbe4, farbe4,
        farbe3, farbe3, farbe3, farbe3, farbe4, farbe4, farbe4, farbe4,
        farbe3, farbe3, farbe3, farbe3, farbe4, farbe4, farbe4, farbe4,
        farbe3, farbe3, farbe3, farbe3, farbe4, farbe4, farbe4, farbe4,
    ]
    while True:
        while counter != 6:
            sense.set_pixels(disco)
            time.sleep(0.2)
            counter+=1
        while counter >=6 and !=30:
            sense.clear(farbliste[heiligenummber(farbliste)])
            time.sleep(0.1)
            counter +=1

def discoisdead():
    #methode die LEDs wieder ausschaltet
    sense.clear()
#------------------------------------- main skript -------------------------------------#
# when alarm goes off, opportunity to get 7 extra minutes is given
try:
    while True:
        curr_time = int(time.strftime("%H%M"))  # continuesly checking time
        time.sleep(0.2)  # break from checking to avoid overheating of chip

        if shakeit == True and curr_time != alarm:
            print('too early, sleep more')
            wait = 1

        if coolthebeer == True and curr_time != alarm:
            print('too early, sleep more')
            wait = 1

        if curr_time == alarm and wait == 1:# alarm goes off at set time
            countdown() #ein Countdown wird angezeigt
            awake = 1
            wait = 2
            alarmsound = vlc.MediaPlayer("/home/pi/songs_and_sounds/alarm/" + random.choice(alarm_sounds))
            alarmsound.play()  # a random selected song from the alarm_sound list is played
            while awake <= 1:  # alarm goes off
                getthediscostarted() #eine krasse flickerlichtattacke wird gestartet
                if shakeit() == True and wait == 2:  # the alarm is shaken
                    alarmsound.stop()  # the alarmsound activated earlier stops playing
                    discoisdead()
                    sound = vlc.MediaPlayer(
                        "/home/pi/songs_and_sounds/special celebration/" + random.choice(celebration_sounds))
                    sound.play()  #say something funny, like we love to wake you up
                    awake = 2
                if coolthebeer()==True:  # if the alarm is in the fridge, the alarm stops immediately
                    discoisdead()
                    awake = 3

        if awake == 2:  # the alarm was shaken
            while count == 1:
                alarm += 7  # seven minutes more sleep
                print('\nYou recieved 7 extra minutes!!') #prints are used to control the process in the terminal on the remote desktop
                # put annoucement here
                time_start_song = alarm
                time_start_song -= 4  # this variable starts a song 4 minutes before the second alarm, making getting up more comfortable
                count = 2
        if curr_time == time_start_song:
            while count == 2:  # the count variable is also used to exit the while loop after one round, otherwise multiple songs play
                song = vlc.MediaPlayer("/home/pi/songs_and_sounds/" + random.choice(wakeup_songs))
                song.play()
                count = 3
        if curr_time == alarm and count == 3:  # alarm starts again
            getthediscostarted()
            song.stop()
            while count == 4:
                alarmsound = vlc.MediaPlayer("/home/pi/songs_and_sounds/alarm/" + random.choice(alarm_sounds))
                alarmsound.play()
                count = 4

        if coolthebeer()==True:  # door opens and person got up!!!
            discoisdead()
            song.stop()
            awake = 3

        if awake == 3:
            sounds = vlc.MediaPlayer(
                "/home/pi/songs_and_sounds/alarm/special celebration/" + random.choice(celebration_sounds))
            sounds.play()
            discoisdead()
            song.stop()
            alarmsound.stop()
            sounds.stop()
            print("good morning")
            GPIO.cleanup()
            break
finally:
    print("have a nice day")
    GPIO.cleanup()
