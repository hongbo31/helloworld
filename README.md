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
        self.driver.current_driver.switch_to.frame(0).find_element("all").click()

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
    
给我重点看看refresh_category这个方法写的问题在哪，找不到all这个元素，报错是下面：
C:\Python36\python.exe D:/MLUIproject-master/page/homepage.py
a
Traceback (most recent call last):
  File "D:/MLUIproject-master/page/homepage.py", line 36, in <module>
    homePage.refresh_category()
  File "D:/MLUIproject-master/page/homepage.py", line 21, in refresh_category
    self.driver.current_driver.switch_to.frame(0).find_element("all").click()
AttributeError: 'NoneType' object has no attribute 'find_element'

Process finished with exit code 1


-------------------------------------------------------------------------改过之后--------------------------------------------

 # 点击首页右下角的all，刷新为全部分类
    def refresh_category(self):
        self.driver.current_driver.switch_to.frame(0)
        self.driver.current_driver.find_element("all").click()
        
        -----------------------------------------报错-------------------------------
        
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
    
    -------------------------------------all 的配置---------------------------------
    
    <element page = "enterprise 测试前台">
        <name>all</name>
        <pathType>xpath</pathType>
        <pathValue>/html/body/div/dl[8]/dt/a</pathValue>
    </element>


def refresh_category(self):
        self.driver.current_driver.switch_to.frame(0)
        
        self.driver.current_driver.switch_to.frame("hwIframe2")
        
        self.driver.current_driver.switch_to.frame(0)
        
        self.driver.current_driver.find_element("all").click()
