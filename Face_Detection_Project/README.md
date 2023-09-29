# ISE-Capstone-Design
## Video
https://youtu.be/NNpn7n3vXiM

## Face detecting using from the picture
`
import cv2

face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')

img = cv2.imread('test2.jpg')

gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

faces = face_cascade.detectMultiScale(gray, 1.1, 4)

for (x, y, w, h) in faces:
    cv2.rectangle(img, (x, y), (x+w, y+h), (255,0,0), 2)

cv2.imshow('img', img)
cv2.waitKey()
`
## Face detecting using from real-time video
`
import cv2

face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')

cap = cv2.VideoCapture(0)
while True:
    _, img = cap.read(0)

    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    faces = face_cascade.detectMultiScale(gray, 1.1, 4)

    for (x, y, w, h) in faces:
        cv2.rectangle(img, (x, y), (x+w, y+h), (255,0,0), 2)

    cv2.imshow('img', img)

    k = cv2.waitKey(30) & 0xff
    if k ==27:
        break

cap.release()
`
