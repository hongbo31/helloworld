from selenium import webdriver

from common.basepage import BasePage

from common.driver import Driver

from common.elementRead import ElementsRead

from page.homepage import HomePage

from time import sleep

# test001:欢迎语是否推送,以及内容的正确性
# 实例化一个对象
test001 = HomePage()
# 打开企业英文测试前台页面，并且最大化
test001.link_home_page(1)
# 获取登录状态（text = login为未登录，text = logout 为已登录）
status_login = test001.driver.element_is_exist("login")

status_logout = test001.driver.element_is_exist("logout")

# 获取欢迎语内容
welcome_content_login = test001.driver.find_element("loginWelcome").text

welcome_content_logout = test001.driver.find_element("logoutWelcome").text

if status_login == True:

    try:
    
        assert "Log in" in welcome_content_login.text
        
        print("未登录，欢迎语内容正确")
        
    except Exception:
    
        print("未登录，欢迎语内容错误")

elif status_login == False:

    try:
    
        assert "Log in" not in welcome_content_logout.text
        
        print("登录，欢迎语内容正确")
    except Exception:
    
        print("登录，欢迎语内容错误")
        
        
        
        ------------------------homepage---------------------------
        
        from common.basepage import BasePage
from common.driver import Driver
from time import sleep
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
        self.driver.find_element("all").click()





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
    sleep(2)
    print("b")
    homePage.close_current_page()
    print("c")
