# 处理监听键盘鼠标等动作的包

`pip install keyboard`

`pip install pyhooked`  #不再更新

pyhooked使用demo:

```
from pyhooked import Hook, KeyboardEvent

def handle_events(args):
    if isinstance(args, KeyboardEvent):
        if args.current_key == hot_key and args.event_type == 'key down':
            main()
        elif args.current_key == 'Q' and args.event_type == 'key down':
            hk.stop()
            print('退出啦~')

hk = Hook() #初始化
hk.handler = handle_events  #绑定事件句柄
hk.hook() #监听
```

keyboard使用demo: [keyboard](https://github.com/boppreh/keyboard/tree/master/examples)
