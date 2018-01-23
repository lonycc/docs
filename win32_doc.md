# win32相关

> windows系统上一些列操作库.

使用示例:

```
import win32gui
import win32api
import win32con

classname = None
titlename = 'xxx'

hwnd = win32gui.FindWindow(classname, titlename)  #获取窗口句柄
left, top, right, bottom = win32gui.GetWindowRect(hwnd)  #获取窗口左上角和右下角坐标

title = win32gui.GetWindowText(hwnd)  #获取某个窗口句柄的title
clsname = win32gui.GetClassName(hwnd)  #获取某个窗口句柄的class

hwnd1 = win32gui.FindWindowEx(hwnd, None, clsname, None)  #获取父句柄hwnd类名为clsname的子句柄

win32api.SetCursorPos([30, 150]) #鼠标定位到(30, 50)


win32api.mouse_event(win32con.MOUSEEVENTF_LEFTUP | win32con.MOUSEEVENTF_LEFTDOWN, 0, 0, 0, 0) #执行左键单击, 若需要双击则延时几毫秒再点击一次即可
win32api.mouse_event(win32con.MOUSEEVENTF_RIGHTUP | win32con.MOUSEEVENTF_RIGHTDOWN, 0, 0, 0, 0)

win32api.keybd_event(13, 0, 0, 0) #发送回车
# win32api.keybd_event(13, 0, win32con.KEYEVENTF_KEYUP, 0)

win32gui.PostMessage(win32gui.FindWindow(classname, titlename), win32con.WM_CLOSE, 0, 0) #关闭窗口

win32gui.ShowWindow(hld, win32con.SW_RESTORE) #恢复显示窗口句柄

win32gui.SetForegroundWindow(hld) #将句柄hld置于当前活动窗口

import ctypes
from PIL import Image, ImageGrab

class RECT(ctypes.Structure):
    _fields_ = [('left', ctypes.c_long),
            ('top', ctypes.c_long),
            ('right', ctypes.c_long),
            ('bottom', ctypes.c_long)]
    def __str__(self):
        return str((self.left, self.top, self.right, self.bottom))

def analyze_current_screen_text(label, directory="."):
    """当前窗口句柄截屏"""
    hld = win32gui.FindWindow(None, label)
    if hld > 0:
        screenshot_filename = "screenshot.png"
        save_text_area = os.path.join(directory, "text_area.png")
        capture_screen(hld,screenshot_filename, directory)  #截屏
    else:
        print('咦，你没打开'+label+'吧!')


def capture_screen(hld, filename="screenshot.png", directory="."):
    win32gui.ShowWindow(hld, win32con.SW_RESTORE)
    shell = win32com.client.Dispatch("WScript.Shell")
    shell.SendKeys('%')
    win32gui.SetForegroundWindow(hld)
    time.sleep(1)
    rect = RECT()
    ctypes.windll.user32.GetWindowRect(hld, ctypes.byref(rect))
    rangle = (rect.left,rect.top,rect.right,rect.bottom)
    im = ImageGrab.grab(rangle)
    im.save(os.path.join(directory, filename))
```

参考资料:

[win32gui](https://www.programcreek.com/python/index/322/win32gui) 

[win32api](https://www.programcreek.com/python/index/188/win32api)

[win32con](https://www.programcreek.com/python/index/475/win32con)

[win32com.client](https://www.programcreek.com/python/index/697/win32com.client)
