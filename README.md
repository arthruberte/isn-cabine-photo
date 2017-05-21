# isn-cabine-photo

from picamera import PiCamera 
from time import sleep
import time
from grovepi import *
import subprocess
import os

stat = os.statvfs('cabine photo.py')
espace_min = 2500000
espace_restant = stat.f_frsize * stat.f_bfree 
led = 4
switch = 0

pinMode(led, "OUTPUT")

camera = PiCamera()

camera.resolution = (2592, 1944)
camera.start_preview()
while True:
  if espace_restant < espace_min:
    print('plus de place')
    break
  for i in range(11):
    camera.annotate_text = "Souhaitez vous conserver cette photo ?"
    camera.annotate_text_size = 50
    switch = not switch
    digitalWrite(led, switch)
    sleep(1)
    print(10-1)
    if i==10:
      print('Souriez !!')
   camera.capture('/home/pi/Desktop/image0.jpg')
   subprocess.call(["gpicview","/home/pi/Desktop/image0.jpg"])
   camera.stop_preview()
