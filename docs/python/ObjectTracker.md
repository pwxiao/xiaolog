# 物体追踪算法


!!! info "使用须知"
    在使用这段代码前，需要确保设备安装了python库：imutils
    否则请输入`pip`命令
    ```bash
    pip install imutils
    ```

## 遇到的坑
初次运行是出现了以下错误
```python
AttributeError: module 'cv2' has no attribute 'TrackerBoosting_create'
# 或者
AttributeError: module 'cv2' has no attribute 'TrackerTLD_create'
# 或者
AttributeError: module 'cv2' has no attribute 'TrackerMedianFlow_create'
# 或者
AttributeError: module 'cv2' has no attribute 'TrackerMOSSE_create'
```
这是由于opencv版本包括4.5.1以上，不支持这些函数，网上的方法多是提出安装`opencv-contrib-python`,我试过了但无效。最终解决方案是：
将以下代码
```python
		"boosting": cv2.legacy.TrackerBoosting_create,
		"mil": cv2.TrackerMIL_create,
		"tld": cv2.TrackerTLD_create,
		"medianflow": cv2.TrackerMedianFlow_create,
		"mosse": cv2.TrackerMOSSE_create
```
修改成
```python
		"boosting": cv2.legacy.TrackerBoosting_create,
		"mil": cv2.legacy.TrackerMIL_create,
		"tld": cv2.legacy.TrackerTLD_create,
		"medianflow": cv2.legacy.TrackerMedianFlow_create,
		"mosse": cv2.legacy.TrackerMOSSE_create
```
修改后运行成功！


以下是物体追踪算法的全部代码：

```python
from imutils.video import VideoStream
from imutils.video import FPS
import argparse
import imutils
import time
import cv2
import numpy as np

# 构造参数解析器
ap = argparse.ArgumentParser()
ap.add_argument("-v", "--video", type=str,
	help="path to input video file")
ap.add_argument("-t", "--tracker", type=str, default="kcf",
	help="OpenCV object tracker type")
args = vars(ap.parse_args())

# 获取opencv版本信息
(major, minor) = cv2.__version__.split(".")[:2]
# 创建物体追踪器 直接使用Tracker_create，版本3.2及其之前直接使用
if int(major) == 3 and int(minor) < 3:
	tracker = cv2.Tracker_create(args["tracker"].upper())


else:
	OPENCV_OBJECT_TRACKERS = {
		"csrt": cv2.TrackerCSRT_create,
		"kcf": cv2.TrackerKCF_create,
		"boosting": cv2.legacy.TrackerBoosting_create,
		"mil": cv2.legacy.TrackerMIL_create,
		"tld": cv2.legacy.TrackerTLD_create,
		"medianflow": cv2.legacy.TrackerMedianFlow_create,
		"mosse": cv2.legacy.TrackerMOSSE_create
	}

	tracker = OPENCV_OBJECT_TRACKERS[args["tracker"]]()


initBB = None


if not args.get("video", False):
	print("[INFO] starting video stream...")
	vs = VideoStream(src=0).start()
	time.sleep(1.0)

else:
	vs = cv2.VideoCapture(args["video"])

fps = None


def video_mirror_output(video):
    new_img=np.zeros_like(video)
    h,w=video.shape[0],video.shape[1]
    for row in range(h):
        for i in range(w):
            new_img[row,i] = video[row,w-i-1]
    return new_img


while True:
	frame = vs.read()
	#翻转
	frame = cv2.flip(frame,1)
	frame = frame[1] if args.get("video", False) else frame
	# 检车是否 读取到视频流结尾
	if frame is None:
		break
	# 调整窗体大小，提高处理效率
	frame = imutils.resize(frame, width=500)
	(H, W) = frame.shape[:2]

	# 检查当前是否在追踪物体
	if initBB is not None:

		(success, box) = tracker.update(frame)

		if success:
			(x, y, w, h) = [int(v) for v in box]
			cv2.rectangle(frame, (x, y), (x + w, y + h),
				(0, 255, 0), 2)

		fps.update()
		fps.stop()

		info = [
			("Tracker", args["tracker"]),
			("Success", "Yes" if success else "No"),
			("FPS", "{:.2f}".format(fps.fps())),
		]

		for (i, (k, v)) in enumerate(info):
			text = "{}: {}".format(k, v)
			cv2.putText(frame, text, (10, H - ((i * 20) + 20)),
				cv2.FONT_HERSHEY_SIMPLEX, 0.6, (0, 0, 255), 2)


	cv2.imshow("Frame", frame)
	key = cv2.waitKey(1) & 0xFF

	if key == ord("s"):

		initBB = cv2.selectROI("Frame", frame, fromCenter=False,
			showCrosshair=True)

		tracker.init(frame, initBB)
		fps = FPS().start()


	elif key == ord("q"):
		break

if not args.get("video", False):
	vs.stop()

else:
	vs.release()

cv2.destroyAllWindows()
```