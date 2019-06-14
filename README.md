# helloworld
hello world!!!!
import selenium
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
        return website


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
    homePage.refresh_category()

    homePage.close_current_page()

C:\Python36\python.exe D:/MLUIproject-master/page/homepage.py
Traceback (most recent call last):
  File "D:/MLUIproject-master/page/homepage.py", line 2, in <module>
    from common.basepage import BasePage
  File "D:\MLUIproject-master\common\basepage.py", line 2, in <module>
    from page.homepage import HomePage
  File "D:\MLUIproject-master\page\homepage.py", line 2, in <module>
    from common.basepage import BasePage
ImportError: cannot import name 'BasePage'

Process finished with exit code 1

22222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222
from common.driver import Driver


class BasePage:

    def __init__(self, web=1):
        self.current_page = None
        self.driver = Driver(web)

    def link_page(self,url):
        self.driver.current_driver.get(url)
        self.driver.current_driver.maximize_window()
