# splinter

> 一个开源的web测试工具, 可以用它控制浏览器模拟人工操作.

安装方式：`pip install splinter`

使用示例：

```
from splinter import Browser

with Browser(driver_name='chrome', execute_path='/path/to/chromedriver', headless=False, user_agent='Mozilla') as browser:
  browser.driver.set_window_size(1024, 768) #设置页面尺寸
  browser.visit('https://www.baidu.com')  #打开百度
  browser.fill('wd', 'splinter')  #查询构造?wd=splinter
  button = browser.find_by_id('su')  #<tag id="su">
  // browser.find_by_text(u'查询')  #<tag>查询</tag>
  // browser.find_by_name('btn').first   #<tag name="btn">
  // browser.find_by_css('h1')
  // browser.find_by_xpath('//h1')
  // browser.find_by_tag('h1')  #<h1>xx</h1>
  // browser.find_by_value('query')  #<tag value="query">
  
  button.click()  #执行点击
  if browser.is_text_present('splinter.readthedocs.io'):  #搜索结果中是否存在<tag>splinter.readthedocs.io</tag>
    print('good')
    
  browser.cookies.add({'whatever': 'and ever'}) #新增cookie
  browser.cookies.all() #获取所有cookie
  browser.cookies.deleta('name') #删除key为name的cookie
  browser.cookies.delete('whatever', 'wherever') #删除多个cookie
  browser.cookies.delete() #删除所有cookie
  
  browser.reload()  #重新加载当前页面
  browser.back()  #回到后一个
  browser.forward()  #回到前一个
  browser.url  #当前url
  browser.title #当前页面title
  browser.html  #当前页面html
  browser.status_code  #返回状态码
  
  browser.execute_script("$('body').empty()")  #执行js代码

  browser.windows              # all open windows
  browser.windows[0]           # the first window
  browser.windows[window_name] # the window_name window
  browser.windows.current      # the current window
  browser.windows.current = browser.windows[3]  # set current window to window 3

  window = browser.windows[0]
  window.is_current            # boolean - whether window is current active window
  window.is_current = True     # set this window to be current window
  window.next                  # the next window
  window.prev                  # the previous window
  window.close()               # close this window
  window.close_others()        # close all windows except this one
  
  browser.quit()  #退出程序, 关闭浏览器
```

参考资料[源码](https://github.com/cobrateam/splinter) [docs](http://splinter.readthedocs.io/en/latest/index.html)
