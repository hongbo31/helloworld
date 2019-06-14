-------------basepage页面内容----------------

from common.driver import Driver


class BasePage:

    def __init__(self, web=1):
        self.current_page = None
        self.driver = Driver(web)

    def link_page(self,url):
        self.driver.current_driver.get(url)
        self.driver.current_driver.maximize_window()

    # 页面前进
    def forward_page(self):
        self.driver.forward()

    # 页面回退
    def back_page(self):
        self.driver.back()

    # 返回当前页面的url
    def get_current_url(self):
        return self.driver.current_driver.current_url

    # 返回当前页面的title
    def get_current_title(self):
        return self.driver.current_driver.title

    # 获取当前窗口的handle
    def get_current_window_handle(self):
        return self.driver.current_driver.current_window_handles

    # 获取所有窗口的handle
    def get_windows_handles(self):
        return self.driver.current_driver.window_handles

    # 跳转到iframe页面， 可传参数name，id，如果iframe没有这个属性的话，可以用index或者element来定位
    def switch_to_iframe(self,index=0):
        self.driver.current_driver.switch_to.frame()

    # 回到原来的iframe页面
    def switch_to_back_default_iframe(self):
        self.driver.current_driver.switch_to.default_content()

    # 首页出现18岁年龄确认弹窗，关闭弹窗
    """
    def close_age_alert(self):
        self.driver.find_element("首页年龄弹窗确认").click()
    """
    #关闭浏览器
    def close_current_page(self):
        self.driver.close_browser()

if __name__ == '__main__':
    enrobotpage = BasePage()
    enrobotpage.driver.open_browser("http://www.multilotto.com/en")
    enrobotpage.driver.find_element("首页弹窗确认").click()
    enrobotpage.link_page()


--------------------homepage---------------页面内容---------------------


from common.basepage import BasePage
from common.driver import Driver

class HomePage(BasePage):

    # 进入网站首页，传入站点名称即可
    def link_home_page(self, website=1):
        if website ==1:
            self.link_page("http://rnd-suptest.huawei.com/enrobot")  # 企业英文测试前台
        elif website == 2:
            self.link_page("http://support-trial.huawei.com/enrobot")  # 企业英文蓝环境前台
        elif website == 3:
            self.link_page("http://support.huawei.com/enrobot")  # 企业英文生产前台
        elif website == 4:
            self.link_page("http://support.huawei.com/iknow")  # 企业英文生产前台（对外）


    # 点击首页右下角的all，刷新为全部分类
    def refresh_category(self):
        self.driver.current_driver.switch_to.frame(0)
        self.driver.current_driver.find_element("all").click()

"""
    # 从首页进入登录页面
    def link_signin_page(self):
        self.driver.find_element("首页signin按钮").click()

    def link_signup_page(self):
        self.driver.find_element("首页signup按钮").click()
"""

if __name__ == '__main__':

    homePage = HomePage()
    
    homePage.link_home_page(1)
    
    print("a")
    
    homePage.refresh_category()
    
    print("b")
    
    homePage.close_current_page()
    
    print("c")

--------------------------报错--------------------------------

C:\Python36\python.exe D:/MLUIproject-master/page/homepage.py
a
Traceback (most recent call last):
  File "D:/MLUIproject-master/page/homepage.py", line 37, in <module>
    homePage.refresh_category()
  File "D:/MLUIproject-master/page/homepage.py", line 22, in refresh_category
    self.driver.current_driver.find_element("all").click()
  File "C:\Python36\lib\site-packages\selenium\webdriver\remote\webdriver.py", line 978, in find_element
    'value': value})['value']
  File "C:\Python36\lib\site-packages\selenium\webdriver\remote\webdriver.py", line 321, in execute
    self.error_handler.check_response(response)
  File "C:\Python36\lib\site-packages\selenium\webdriver\remote\errorhandler.py", line 242, in check_response
    raise exception_class(message, screen, stacktrace)
selenium.common.exceptions.WebDriverException: Message: invalid argument: 'value' must be a string
  (Session info: chrome=74.0.3729.169)
  (Driver info: chromedriver=2.37.544315 (730aa6a5fdba159ac9f4c1e8cbc59bf1b5ce12b7),platform=Windows NT 6.1.7601 SP1 x86_64)


Process finished with exit code 1



-------------------------current driver--------------------

class Driver:

    def __init__(self, web=1):
    
        self.elementRead = ElementsRead()
        
        if web == 1:
        
            self.current_driver = webdriver.Chrome()
            
        elif web == 2:
        
            self.current_driver = webdriver.Ie()
            
        elif web == 3:
        
            self.current_driver = webdriver.Firefox()
            
        self.action = ActionChains(self.current_driver)
