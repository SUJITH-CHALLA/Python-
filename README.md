# Python-

import numpy as np
import cv2

# Load image
img = cv2.imread('picture/51c.jpg')

# Convert to grayscale and threshold
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
thresh = cv2.threshold(gray, 220, 255, cv2.THRESH_BINARY_INV)[1] 

# Find contours and filter for rectangle
cnts = cv2.findContours(thresh, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
cnts = cnts[0] if len(cnts) == 2 else cnts[1]
for c in cnts:
    peri = cv2.arcLength(c, True)
    approx = cv2.approxPolyDP(c, 0.04 * peri, True)
    if len(approx) == 4:
        x,y,w,h = cv2.boundingRect(approx)
        if abs(w - h) <= 3:
            cv2.rectangle(img, (x, y), (x + w, y + h), (36,255,12), 2)

cv2.imshow('Image', img)
cv2.waitKey(0)
