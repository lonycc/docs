# 图片处理包PIL/Pillow

```
from PIL import Image, ImageFilter, ImageDraw, ImageFont, ImageEnhance, ImageGrab
import os
import sys
import re
from pyocr import pyocr
from pytesseract import image_to_string

class ImageUtil(object):
    
    def __init__(self, imagefile):
        self.imagefile = imagefile
    
    # 根据文件后缀判断是否图片, 可以用python-magic包来判断
    def isImage(self, image):
        if re.match('(.*?)(\.jpg|\.png|\.gif|\.jpeg|\.bmp)', image):
            return True
        return False
        
    # 是否色情图片, 将图片转为YCbCr颜色空间
    def isPornImage(self):
        if isImage(self.imagefile):
            img = Image.open(self.imagefile).convert('YCbCr')
            w, h = img.size
            data = img.getdata()
            cnt = 0
            for i, ycbcr in enumerate(data):
                y, cb, cr = ycbcr
                if 86 <= cb <= 117 and 140 <= cr <= 168:
                    cnt += 1
            print('%s %s a porn image.' %(self.imagefile, 'is' if cnt > w * h * 0.3 else 'is not'))
        else:
            print(self.imagefile + ' is not image')
    
    # 删除图片
    def delImage(self, filepath):
        if os.path.exists(filepath):
            filelist = os.listdir(filepath)
            for file in filelist:
                if isImage(file):
                    img = Image.open(filepath + file)
                    w, h = img.size
                    size = os.path.getsize(filepath + file)
                    if w < 500 or h < 500 or size < 30240:
                        os.remove(filepath + file)
        else:
            print(filepath + ' is not existed')
    
    # pyocr包识别文字         
    def extractChars(self, pic):
        tools = pyocr.get_avaliable_tools()[:]
        if len(tools) == 0:
            print()
            sys.exit(1)
        print('Using %s' % (tools[0].get_name()))
        tools[0].image_to_string(Image.open(pic), lang='fra', builder=TextBuilder())
    
    # ocr识别图片中的文字
    def extractChars2(self, pic):
        return image_to_string(Image.open(pic))
    
    # 屏幕抓图
    def capture_screen(self):
        im = ImageGrab.grab() #全屏抓取
        # im = ImageGrab.grab((100, 100, 1100, 600)) #抓取指定区域左上(100, 100), 右下(1100, 600)
        # im = ImageGrab.grabclipboard() #抓取当前剪贴板的快照
        # isinstance(im, Image)  #im是否是Image实例
        width, height = im.size
        mode = im.mode  #模式, RGB
        format = im.format #MIMEtype
        # im.show()  #显示图片
        im.save('/path/to/demo.png')  #保存到文件里
    
    # 图片处理
    def operate(self):
        im = Image.open(self.imagefile)
        box = (100, 100, 200, 200)
        region = im.crop(box) #截取指定坐标的矩形区域
        region = region.transpose(Image.ROTATE_180) #距去区域旋转180度
        im.paste(region, box) #将截取的区域粘贴到原图上
```
