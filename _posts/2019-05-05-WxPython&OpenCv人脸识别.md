---
layout:     post
title: WxPython&OpenCv人脸识别
subtitle: WxPython&OpenCv人脸识别简单结合
date: 2019-05-05
author: Bend
header-img: img/post-bg-debug.png
catalog: true
tags:
    - WxPython
    - OpenCv
    - 人脸识别
---
>五一很忙呀,更文频率应该会继续下降,被博客图床问题搞得有点烦呀.

这是一个将 WxPython 和 OpenCv 结合的小程序

```Python
import cv2
import wx


COVER = 'camera_close.png'


class Face_detect(wx.Frame):

    def __init__(self, parent, title):
        # 框架初始化
        wx.Frame.__init__(self, parent, title=title, size=(640, 480))
        self.SetIcon(wx.Icon(name='face.png', type=wx.BITMAP_TYPE_PNG))
        self.panel = wx.Panel(self, size=(640, 480))
        self.Center()
        self.image_cover = wx.Image(COVER, wx.BITMAP_TYPE_ANY).Scale(350, 300)
        self.bmp = wx.StaticBitmap(self.panel, -1, wx.Bitmap(self.image_cover))
        start_button = wx.Button(self.panel, label='开始')
        close_button = wx.Button(self.panel, label='结束')
        self.Bind(wx.EVT_BUTTON, self.learning_face, start_button)
        self.Bind(wx.EVT_BUTTON, self.close_face, close_button)
        self.vbox = wx.BoxSizer(wx.VERTICAL)
        self.hbox1 = wx.BoxSizer(wx.HORIZONTAL)
        self.hbox2 = wx.BoxSizer(wx.HORIZONTAL)
        self.hbox1.Add(self.bmp, proportion=1, flag=wx.EXPAND | wx.ALL)
        self.vbox.Add(self.hbox1, flag=wx.EXPAND | wx.LEFT | wx.RIGHT | wx.TOP, border=10)
        self.hbox2.Add(start_button, proportion=0, flag=wx.LEFT | wx.BOTTOM, border=5)
        self.hbox2.Add(close_button, proportion=0, flag=wx.LEFT | wx.BOTTOM, border=5)
        self.vbox.Add(self.hbox2, flag=wx.ALIGN_RIGHT | wx.RIGHT, border=10)
        self.panel.SetSizer(self.vbox)
        self.vbox.Fit(self)

        # 界面自动调整窗口适应内容

    def _learning_face(self, event):
        # 建cv2摄像头对象，这里使用电脑自带摄像头，如果接了外部摄像头，则自动切换到外部摄像头
        # self.cap = cv2.VideoCapture("E:\Bend\Videos\VideoProc\_Chandelier_ SIA _ Allie Sherlock cover.mp4")
        self.cap = cv2.VideoCapture(0)
        # cap.isOpened（） 返回true/false 检查初始化是否成功
        while (self.cap.isOpened()):
                # cap.read()
                # 返回两个值：
                #    一个布尔值true/false，用来判断读取视频是否成功/是否到视频末尾
                #    图像对象，图像的三维矩阵
            flag, self.im_rd = self.cap.read()
            self.im_rd = cv2.flip(self.im_rd, 1)
            im_rd2 = self.face_detect_demo(self.im_rd)

            self.k = cv2.waitKey(10)
            if (self.k == ord('q')):
                break
                # 使用opencv会在新的窗口中显示
                # cv2.imshow("camera", im_rd)
                # 现将opencv截取的一帧图片BGR转换为RGB，然后将图片显示在UI的框架中

            height, width = im_rd2.shape[:2]
            image1 = cv2.cvtColor(im_rd2, cv2.COLOR_BGR2RGB)
            pic = wx.Bitmap.FromBuffer(width, height, image1)
            # 显示图片在panel上
            self.bmp.SetBitmap(pic)
            self.vbox.Fit(self)
            # 释放摄像头

            # 删除建立的窗口
            # cv2.destroyAllWindows()

        # self.cap.release()

    def face_detect_demo(self, frame):
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

        face_detector = cv2.CascadeClassifier(r"C:\Users\Bend\AppData\Local\Programs\Python\Python36\Lib\site-packages\cv2\data\haarcascade_frontalface_alt.xml")

        faces = face_detector.detectMultiScale(gray, 1.1, 2)
        for x, y, w, h in faces:
            cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 0, 255), 2)

        return frame

    def learning_face(self, event):
        """使用多线程，子线程运行后台的程序，主线程更新前台的UI，这样不会互相影响"""
        import _thread
        # 创建子线程，按钮调用这个方法，
        _thread.start_new_thread(self._learning_face, (event,))

    def close_face(self, event):
        """关闭摄像头"""
        self.cap.release()
        self.vbox.Fit(self)


class main_app(wx.App):
    # OnInit 方法在主事件循环开始前被wxPython系统调用，是wxpython独有的
    def OnInit(self):
        self.frame = Face_detect(parent=None, title="Face-Detect")
        self.frame.Show(True)
        return True


if __name__ == "__main__":
    app = main_app()
    app.MainLoop()

```

下面是程序截图
![Snipaste_2019-04-26_13-10-53.jpg](https://ddd.cat/images/2019/05/05/Snipaste_2019-04-26_13-10-53.jpg)

码文仓促,未来应该会继续更新~~