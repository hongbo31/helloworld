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
