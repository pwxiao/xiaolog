# opencv学习


## 图像基本读取与显示
```python
导入cv2和matplotlib库
import cv2 as cv
from matplotlib import pyplot as plt

读取图像
img = cv.imread("2.png",0)

显示图像
plt.imshow(img,cmap='Accent',interpolation='bicubic')
plt.xticks([]),plt.yticks([])
plt.show()

等待键盘输入
k = cv.waitKey(0)

判断键盘输入
if k==27: # Esc键
cv.destroyAllWindows()
elif k==ord('s') & 0xFF: # s键
# 保存图像
cv.imwrite("ani.png",img)
print("保存成功")
```

## 调用摄像头
```python
导入cv2库
import cv2 as cv

打开电脑摄像头
cap = cv.VideoCapture(0)

循环读取摄像头数据并展示
while True:
# 读取摄像头数据
ret,fram = cap.read()
# 对读取的数据进行翻转
flip = cv.flip(fram,1)
# 在窗口中展示读取的数据
cv.imshow('frame',fram)
# 等待用户输入
# k = cv.waitKey(0)
# if k==27:
# cv.destroyAllWindows()

释放摄像头资源
cap.release()

关闭所有窗口
cv.destroyAllWindows()
```