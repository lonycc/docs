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

win32gui.PostMessage(win32gui.findWindow(classname, titlename), win32con.WM_CLOSE, 0, 0) #关闭窗口
```

参考资料:

[win32gui](https://www.programcreek.com/python/index/322/win32gui) 

[win32api](https://www.programcreek.com/python/index/188/win32api)

[win32con](https://www.programcreek.com/python/index/475/win32con)
