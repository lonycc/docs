# mitmproxy

安装方式：`pip install proxymitm`

功能说明：

> 中间人攻击, 可用于代理抓包.

使用方式:

`mitmproxy -p [port] -s [script.py]`

手机或电脑设置代理, 确保在同一局域网内. 然后浏览器访问`http://mitm.it`, 安装相应的证书.

下面是微信小游戏[王者头脑]的一个demo, 代码来源于某V2网友.

```
import re
import json
from mitmproxy import ctx
from urllib.parse import quote
import string
import requests

def response(flow):
    path = flow.request.path
    if path == '/question/bat/findQuiz':
        content = flow.response.text
        data = json.loads(content)
        question = data['data']['quiz']
        options = data['data']['options']
        ctx.log.info('question : %s, options : %s'%(question, options))
        options = ask(question, options)
        data['data']['options'] = options
        flow.response.text = json.dumps(data)

def ask(question, options):
    url = quote('http://www.baidu.com/s?wd=' + question, safe = string.printable)
    content = requests.get(url).text
    answer = []
    for option in options:
        count = content.count(option)
        ctx.log.info('option : %s, count : %s'%(option, count))
        answer.append(option + ' [' + str(count) + ']')
    return answer
```

参考资料:[docs](http://docs.mitmproxy.org/en/latest/mitmproxy.html)
