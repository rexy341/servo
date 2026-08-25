```py
from cvzone.HandTrackingModule import HandDetector
from cvzone.HandTrackingModule import cv2
import pyfirmata2

cap = cv2.VideoCapture(0)
detector = HandDetector(maxHands=2, detectionCon=0.2)
board = pyfirmata2.Arduino('COM6')
print("myserial",board)

servo_thumb = board.get_pin('d:9:s') # create a Servo object and attach it to pin 9
servo_index = board.get_pin('d:10:s')
servo_middle = board.get_pin('d:11:s')
servo_ring = board.get_pin('d:12:s')
servo_pinky = board.get_pin('d:13:s')
fingers1=[]

while True:
    hands={}
    success, img = cap.read()
    hands, img = detector.findHands(img)
    
    if hands:
        hand1 = hands[0]
        lmList = hand1["lmList"]
        fingers1 = detector.fingersUp(hand1)
        print(fingers1)
        if len (hands)>1 :
            hand2 = hands[1]
            fingers2 = detector.fingersUp(hand2)
            if (fingers1[0]==1 and fingers1[1]==fingers1[2]==fingers1[3]==fingers1[4]== 0 and fingers2[1]==fingers2[2]==1 and fingers2[0]==fingers2[3]==fingers2[4]== 0) or (fingers2[0]==1 and fingers2[1]==fingers2[2]==fingers2[3]==fingers2[4]== 0 and fingers1[1]==fingers1[2]==1 and fingers1[0]==fingers1[3]==fingers1[4]== 0):
               # Terminate program if certain hand configuration is detected
               break
    cv2.imshow("tracker", img)
    
    
    if fingers1[0]==1:
       servo_thumb.write(180)
    else :
        servo_thumb.write(0)
            
    if fingers1[1]==1:
       servo_index.write(180)
    else :
        servo_index.write(0)    
        
    if fingers1[2]==1:
       servo_middle.write(180)
    else :
        servo_middle.write(0)
    
    if fingers1[3]==1:
       servo_ring.write(180)
    else :
        servo_ring.write(0)
        
    if fingers1[4]==1:
       servo_pinky.write(180)
    else :
        servo_pinky.write(0)
            
    cv2.waitKey(2000)  
print("Bye Bye")
cap.release()
cv2.destroyAllWindows()       
# Close the serial connection and release the camera
#board.close()
```
