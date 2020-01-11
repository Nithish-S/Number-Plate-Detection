# Number-Plate-Detection
Number Plate detection using opencv.
#...........................Functions.......................#

import cv2
import pytesseract

ori_img= cv2.imread('C:\\Users\\user\\PycharmProjects\\AGV\\number plate\\test img\\1.jpg')
img= cv2.resize(ori_img,(800,800))


def pyploter(img):
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)                      # cov color 2 gray img
    blur = cv2.GaussianBlur(gray,(5,5),0)                            # making it blur to avoid noise..
    canny_edge = cv2.Canny(blur,50,150)                              # finding edges with high change in intensity
    return canny_edge


cannyedge = pyploter(img)
img1 = img.copy()
contours, hierarchy = cv2.findContours(cannyedge, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
contours = sorted(contours, key=cv2.contourArea, reverse= True)[:30]
cimg = cv2.drawContours(img1,contours,-1,(34,244,70),1)
plate = None
h=3
for c in contours:
    perimeter = cv2.arcLength(c,True)
    aprox = cv2.approxPolyDP(c,0.01*perimeter, True)

    if len(aprox) == 4:
        plate = aprox
        print(plate)
        x,y,w,h = cv2.boundingRect(c)
        new_img = img[y:y+h,x:x+w]
        n_img = cv2.drawContours(img, [plate], -1, (34, 244, 70), 5)
        cv2.imshow("Number plate", n_img)
        cv2.imwrite('C:\\Users\\user\\PycharmProjects\\AGV\\number plate\\result\\'+'e.jpg',new_img)

        break

cv2.imshow("CannyEdge", cannyedge)
cv2.imshow("Counters",cimg)


#crop =cv2.imread('C:\\Users\\user\\PycharmProjects\\AGV\\number plate\\resulte.jpg')
#text = pytesseract.image_to_string(crop)
#print(text)
cv2.waitKey(0)
