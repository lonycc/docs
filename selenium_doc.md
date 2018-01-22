# selenium

> 一个web自动化测试工具.

安装方式: `pip install selenium`

使用示例：

```
from selenium import webdriver
from selenium.common.exceptions import NoSuchElementException
from selenium.webdriver.common.keys import Keys
import time

browser = webdriver.Chrome()
browser.get('http://www.alexa.cn/southcn.com')
#browser.maximize_window()  #最大化窗口
#browser.set_window_size(480,800)  #固定宽和高
#browser.back()     #后退
#browser.forward()  #前进
#browser.find_element_by_name('username').send_keys('selenium')
#browser.find_element_by_id('key').click()
#browser.find_element_by_tag_name('input')
#browser.find_element_by_css_selector('#kw')
#browser.find_element_by_css_selector('a[name="fuck"]')
#browser.find_element_by_xpath('//input[@id="kw"]')
#browser.find_element_by_link_text('click me').click()
#browser.find_element_by_partial_link_text('click').click()

worldrank = browser.find_element_by_xpath('//ul[@class="info1"]/li[1]/font').text
week = browser.find_element_by_xpath('//ul[@class="info1"]/li[3]/font').text
month = browser.find_element_by_xpath('//ul[@class="info1"]/li[5]/font').text
season = browser.find_element_by_xpath('//ul[@class="info1"]/li[7]/font').text

title = browser.title  #当前标题

#time.sleep(1)
#browser.close()  #关闭当前窗口

browser.quit()   #关闭全部窗口

from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver import ActionChains

# selenium 测试
def test_selenium(url):
    service_args=[]
    service_args.append('--load-images=no')  ##关闭图片加载
    service_args.append('--disk-cache=yes')  ##开启缓存
    service_args.append('--ignore-ssl-errors=true') ##忽略https错误

    driver = webdriver.PhantomJS('/usr/local/bin/phantomjs', service_args=service_args)
    driver.implicitly_wait(10)        ##设置超时时间
    driver.set_page_load_timeout(10)  ##设置超时时间
    driver.get(url)
    driver.find_element_by_id('password').clear() #清空
    driver.find_element_by_id('password').send_keys('password') #填充
    driver.find_element_by_xpath('//input[@name="user"]').send_keys('admin')
    driver.find_element_by_id('submit').click()
    #driver.find_element_by_xpath('//form[@name="lzform"]').submit()
    #cookies = driver.get_cookies() #获取cookies
    #driver.switch_to.alert() 
    #driver.accept()
    #driver.dismiss()
    #driver.execute_script('一段js代码') #自定义弹窗, 可以将其dispaly设置为none
    #driver.switch_to.iframe('frame_name')

    try:
        dr = WebDriverWait(driver, 5)
        dr.until(lambda the_driver:the_driver.find_element_by_xpath('//h6[@id="menu_title"]').is_displayed())
        #driver.get_screenshot_as_file('show.png')
        print(driver.current_url)
    except:
        print( '登录失败')
        sys.exit(0)
    '''
    loadmore = driver.find_element_by_xpath('//a[@id="load-more"]')
    actions = ActionChains(driver)
    actions.move_to_element(loadmore)
    actions.click(loadmore)
    actions.perform()
    '''        
    #element.send_keys(Keys.RETURN)
    #print(driver.current_url, driver.page_source)
    driver.quit()
```

参考资料[docs](http://selenium-python-zh.readthedocs.io/en/latest/)
