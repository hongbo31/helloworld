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
status = test001.driver.element_is_exist("login")
if status == True:
    try:
        assert "Log in" in status.text:
        print("未登录，欢迎语内容正确")
    except:
        print("未登录，欢迎语内容错误")

elif status == False:
    try:
        assert "Log in" not in status.text:
        print("未登录，欢迎语内容正确")
    except:
        print("未登录，欢迎语内容错误")
