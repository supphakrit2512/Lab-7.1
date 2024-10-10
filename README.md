# Lab-7.1
import RPi.GPIO as GPIO
import time
from RPLCD.i2c import CharLCD

SW1 = 27
SW2 = 17

members = [
    ("นิธาน ชูสุวรรณ", "1166304620030"),
    ("ศุภกฤษณ์ ปราบแก้ว", "1166304620436"),
]

GPIO.setmode(GPIO.BCM)
GPIO.setup(SW1, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(SW2, GPIO.IN, pull_up_down=GPIO.PUD_UP)

lcd = CharLCD('PCF8574', 0x27)
lcd.clear()

current_member_index = 0

try:
    while True:
        if GPIO.wait_for_edge(SW1, GPIO.FALLING):
            name, student_id = members[current_member_index]
            lcd.clear()
            lcd.write_string(f"{name}\n{student_id}")

            current_member_index += 1
            if current_member_index >= len(members):
                current_member_index = 0

            time.sleep(0.3)

        elif GPIO.wait_for_edge(SW2, GPIO.FALLING):
            print("Bye")
            lcd.clear()
            lcd.write_string("Bye")
            time.sleep(2)
            break

except KeyboardInterrupt:
    pass

finally:
    GPIO.cleanup()
    lcd.clear()
    print("\nBye..")
