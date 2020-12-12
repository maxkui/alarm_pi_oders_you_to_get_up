#!/usr/bin/python
# alarm_pi_oders_you_to_get_up
# raspberrypi/alarm.py
# raspberrypi alarm clock
# 2014, Ismail Uddin
# www.scienceexposure.com
import time
from time import strftime
import vlc
import random
from sense_hat import SenseHat
import multiprocessing
from multiprocessing import Process, Queue


#p3=multiprocessing.Process(target=blink)


sense = SenseHat()
sense.set_rotation(90) #possible values are 0, 90, 180, 270
sense.set_imu_config(False, True, False)  # only Accelerometer is enabled

count = 1  # this variable is used later on
time_start_song = int  # this variable is used later
wait = 1
awake = 0  # another variable that is used later

alarm = int(input("Please input the time for the alarm in format HHMM: \n"))
print("Alarm has been set for %s hrs" % alarm)
wakeup_songs = ["DontStopMeNow.mp3", "Lavieenrose-LouisArmstrong.mp3",
                "MGMT-Kids.mp3", "StandByMe,BenEKing,1961.mp3",
                "buddy.mp3", "campfire.mp3", "simple.mp3", "starglightlounge.mp3",
                "DancingIntheMoonlight.mp3", "04SchützeMich.mp3",
                "01FrankSinatra-MyWay.mp3", "BrookeFraser-SomethingInTheWater[OfficialVideoHD].mp3",
                "IDreamedaDream.mp3", "LouisArmstrongAndHisHotFive-StruttinWithSomeBarbecue.mp3",
                "LouisArmstrong-WhatAWonderfulWorld(HQ Audio).mp3",
                "RIQK3ilhVqm9.128.mp3"]

# media = vlc.MediaPlayer("/home/pi/songs_and_sounds/" + random.choice(wakeup_songs))

alarm_sounds = ["alarmclockbell.mp3", "alarmnormal.mp3",
                "antiqueclock.mp3", "grandfatherclockstrike.mp3",
                "rooster.mp3", "cartoonpartyhoorn.mp3"]
# media = vlc.MediaPlayer("/home/pi/songs_and_sounds/alarm/" + random.choice(alarm_sounds))
celebration_sounds = ["arenacrowdcheer.mp3", "cartoonboing.mp3",
                      "cartoonpartyhorn.mp3", "clappingcrowdstudio03.mp3", "kidscheering.mp3",
                      "recitalcrowdapplause.mp3", "MyMovie13.mp3"]
# media = vlc.MediaPlayer("/home/pi/songs_and_sounds/alarm/" + random.choice(celebration_sounds))
# these lines detect interruptions/signals from the buttons simulateneously
# as the program is running, which ensures that a button activation/press is detected at all times

#------------------------------------- Functions for alarm -------------------------------------#
def shakeit(q):
    #methode die bemerkt wenn geschüttelt wird
    while True:
        counter=0
        average=0
        givemethepower = 0
        while counter != 5:
            orientation1=[]
            orientation2=[]
            sum1=0
            sum2=0
            Grenzwert = 15 #der Grenzwerte der totalten Änderung der Orientation über den Zeitinterval
            orientation = sense.get_orientation()
            # Erste Liste mit Orientierungswerten wird erstellt
            p = int(round(orientation["pitch"], 0))
            orientation1.append(p)
            r = int(round(orientation["roll"], 0))
            orientation1.append(r)
            y = int(round(orientation["yaw"], 0))
            orientation1.append(y)
            time.sleep(0.3) #Zeitintervall zwischen Messungen
            orientation = sense.get_orientation()
            # Zweite Liste mit Orientierungswerten wird erstellt
            p2 = int(round(orientation["pitch"], 0))
            orientation2.append(p2)
            r2 = int(round(orientation["roll"], 0))
            orientation2.append(r2)
            y2 = int(round(orientation["yaw"], 0))
            orientation2.append(y2)
            for index in range(0, len(orientation1)):
                sum1 += orientation1[index] #den totalen wert der Orientierungsmessung
            for n in range(0, len(orientation2)):
                sum2 += orientation2[n] #den totalen Wert der zweiten orientierungsmessung
            if abs(sum2-sum1) <= (Grenzwert): #wenn der Wecker steht
                givemethepower=0
            else:
                givemethepower=1
            average+=givemethepower
            counter+=1
        if average > 2:
            q.put(True)
        else:
            q.put(False)


def coolthebeer(): #methode die bemerkt wenn wecker im Kühlschrank
    while True:
        sense.clear()
        isthebeercold=bool
        temp1 = sense.get_temperature()
        time.sleep(5)      #Temperatur wird alle 5s gemessen
        temp2=sense.get_temperature()
        if (temp2-temp1) >=-5:                 #temperatur kleiner als 10 Grad, line 175 soll ausgelöst werden
            m.put(True)
        else:
            m.put(False)

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
    while counter != 20:
        sense.set_pixels(disco)
        time.sleep(0.2)
        counter +=1
    return

def escalation():
    g = (0, 255, 0)
    y = (255, 255, 0)
    b = (0, 0, 255)
    r = (255, 0, 0)
    w = (255, 255, 255)
    n = (0, 0, 0)
    p = (255, 105, 180)
    farbliste = [g, y, b, r, w, p]
    while True:
        sense.clear(farbliste[heiligenummber(farbliste)])
        time.sleep(0.1)
    return

def discoisdead():
    #methode die LEDs wieder ausschaltet
    sense.clear()
    return
#------------------------------------- main skript -------------------------------------#
# when alarm goes off, opportunity to get 7 extra minutes is given
alarmsound = vlc.MediaPlayer("/home/pi/songs_and_sounds/alarm/" + random.choice(alarm_sounds))
song = vlc.MediaPlayer("/home/pi/songs_and_sounds/" + random.choice(wakeup_songs))
try:
    while True:
        curr_time = int(time.strftime("%H%M"))  # continuesly checking time
        time.sleep(0.2)  # break from checking to avoid overheating of chip

        if curr_time == alarm and wait == 1:# alarm goes off at set time
            countdown() #ein Countdown wird angezeigt
            awake = 1
            wait = 2
            alarmsound = vlc.MediaPlayer("/home/pi/songs_and_sounds/alarm/" + random.choice(alarm_sounds))
            alarmsound.play()  # a random selected song from the alarm_sound list is played
            while awake <= 1:  # alarm goes off
                #getthediscostarted() #eine krasse flickerlichtattacke wird gestartet
                if __name__ == '__main__':
                    q=Queue()
                    pblink = multiprocessing.Process(target=escalation)
                    pshake =multiprocessing.Process(target=shakeit, args=(q,))
                    pblink.start()
                    pshake.start()
                    if q.get()==True:
                        print("I'm about to shake it hard") # the alarm is shaken
                        alarmsound.stop() # the alarmsound activated earlier stops playing
                        discoisdead()
                        sound = vlc.MediaPlayer(
                                "/home/pi/songs_and_sounds/special celebration/" + random.choice(celebration_sounds))
                        sound.play()  #say something funny, like we love to wake you up
                        awake = 2
                    pblink.terminate()
                    pshake.terminate()

        if awake == 2:  # the alarm was shaken
            pblink.terminate()
            pshake.terminate()
            while count == 1:
                alarm += 7  # seven minutes more sleep
                print('\nYou recieved 7 extra minutes!!') #prints are used to control the process in the terminal on the remote desktop
                # put annoucement here
                time_start_song = alarm
                time_start_song -= 4  # this variable starts a song 4 minutes before the second alarm, making getting up more comfortable
                count = 2
        if curr_time == time_start_song:
            while count == 2:  # the count variable is also used to exit the while loop after one round, otherwise multiple songs play
                song.play()
                count = 3
        if curr_time == alarm and count == 3:  # alarm starts again
            #getthediscostarted()
            song.stop()
            alarmsound = vlc.MediaPlayer("/home/pi/songs_and_sounds/alarm/" + random.choice(alarm_sounds))
            alarmsound.play()
            count = 4
            if __name__ == '__main__':
                    m=Queue()
                    pblink =multiprocessing.Process(target=escalation)
                    pcold =multiprocessing.Process(target=coolthebeer, args=(m,))
                    pblink.start()
                    pcold.start()
                    if m.get()==True: # door opens and person got up!!!
                        song = vlc.MediaPlayer("/home/pi/songs_and_sounds/" + random.choice(wakeup_songs))
                        song.stop()
                        awake = 3
                        print("Cool it babe!")

        if awake == 3:
            sounds = vlc.MediaPlayer(
                "/home/pi/songs_and_sounds/alarm/special celebration/" + random.choice(celebration_sounds))
            sounds.play()
            pblink.terminate()
            pcold.terminate()
            discoisdead()
            song.stop()
            alarmsound.stop()
            sounds.stop()
            print("good morning")
            sense.clear()
            break
finally:
    print("have a nice day")
    sense.clear()
