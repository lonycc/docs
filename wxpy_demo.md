##微信机器人, 基于网页网微信的操作接口

> http://wxpy.readthedocs.io/zh/latest/

```
from wxpy import *
bot = Bot()
friends = bot.friends()
friends.stats()
friends.stats_text()

friend = friends.search('xx')[0]
friend.send('hello wechat')
friend.send_image(r'f:/xx.jpg')
friend.send_file(r'f:/xx.zip')
friend.send_video(r'f:/xx.mov')
group = bot.groups().search('大前端')[0]
for xx in group:
	print(xx, xx.name)
mps = bot.mps().search('xx') #公众号
```
