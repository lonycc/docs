# matplotlib是一个2D绘图工具

```
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import numpy as np
from PIL import Image

plt.style.use('ggplot')  #绘图样式
fig = plt.figure()  #创建一个图形
data = np.random.randn(50) #随机整数矩阵
# data = np.array(Image.open('demo.jpg')) #图片像素矩阵
im = plt.imshow(data, animated=True) #图形填充数据
im.set_array(data) #填充新的数据

# 点击事件, 获取点击坐标
def onclick(event):
    print('button=%d, x=%d, y=%d, xdata=%f, ydata=%f' % (event.button, event.x, event.y, event.xdata, event.ydata))

cid = fig.canvas.mpl_connect('button_press_event', onclick) #事件连接, 返回一个连接id
fig.canvas.mpl_disconnect(cid) #断开链接

# 通过重复调用一个方法来形成动画效果
ani = animation.FuncAnimation(fig, update, interval=50, blit=True) #其中update是方法名

plt.show() #打开图形
```

[docs](https://matplotlib.org/users/index.html)
