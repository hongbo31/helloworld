------------------------test001----------------------------
from selenium import webdriver
from common.basepage import BasePage
from common.driver import Driver
from common.elementRead import ElementsRead
from page.homepage import HomePage
from common.log import Log
import unittest
from time import sleep

# test001:欢迎语是否推送,以及内容的正确性
class Test001(unittest.TestCase):
    def __init__(self):
        # 实例化一个HomePage对象,定义为Test001的属性
        self.test001 = HomePage()

    def setUp(self):
        self.log = Log()
        self.log.info("测试开始")

    def run_001(self):
        # 打开企业英文生产对外前台页面，并且最大化，website = 4时为打开生产对外前台
        self.test001.link_home_page(4)

        # 获取登录状态（text = login为未登录，text = logout 为已登录）
        status_login = self.test001.driver.element_is_exist("login")
        
        status_logout = self.test001.driver.element_is_exist("logout")

        # 获取欢迎语内容
        sleep(3)
        
        self.test001.switch_to_iframe(0)
        
        self.test001.switch_to_iframe("hwIframe2")

        welcome_content_login = self.test001.driver.find_element("loginWelcome").text

        print(welcome_content_login)

        welcome_content_logout = self.test001.driver.find_element("logoutWelcome").text

        if status_login == True:
            try:
                assertIn("Log in", welcome_content_login)
                print("未登录，欢迎语内容正确")
            except Exception:
                print("未登录，欢迎语内容错误")

        elif status_login == False:
            try:
                assertNotIn("Log in", welcome_content_logout)
                print("登录，欢迎语内容正确")
            except Exception:
                print("登录，欢迎语内容错误")

    def tearDown(self):
        self.log.info('结束测试')
        sleep(2)
        self.close_browser()




        
        
        
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
    
    
    --------------------------basepage------------------------------
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


------------------------driver---------------------------
from selenium import webdriver
from common.elementRead import ElementsRead
from selenium.webdriver.common.action_chains import ActionChains

import time


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

    #打开浏览器，输入url，然后最大化
    def open_browser(self, url):
        self.current_driver.maximize_window()
        self.current_driver.get(url)
        return self.current_driver

    def getwindow_size(self):
        self.current_driver.get_window_size()

    def setwindow_size(self, width, height, windowHandle='current'):
        self.current_driver.set_window_size(width, height, windowHandle)

    def close_browser(self):
        self.current_driver.quit()

    def find_element(self, name=None):
        pathType = self.elementRead.getPathType(name)
        pathValue = self.elementRead.getPathValue(name)
        if pathType.lower() == 'id':
            return self.current_driver.find_element_by_id(pathValue)
        elif pathType.lower() == 'class_name':
            return self.current_driver.find_element_by_class_name(pathValue)
        elif pathType.lower() == 'name':
            return self.current_driver.find_element_by_name(pathValue)
        elif pathType.lower() == 'css_selector':
            return self.current_driver.find_element_by_css_selector(pathValue)
        elif pathType.lower() == 'xpath':
            return self.current_driver.find_element_by_xpath(pathValue)
        elif pathType.lower() == 'link_text':
            return self.current_driver.find_element_by_link_text(pathValue)
        elif pathType.lower() == 'tag_name':
            return self.current_driver.find_element_by_tag_name(pathValue)
        elif pathType.lower() == 'partial_link_text':
            return self.current_driver.find_element_by_partial_link_text(pathValue)

    def find_elements(self, name=None):
        pathType = self.elementRead.getPathType(name)
        pathValue = self.elementRead.getPathValue(name)
        if pathType == 'id':
            return self.current_driver.find_elements_by_id(pathValue)
        elif pathType == 'class_name':
            return self.current_driver.find_elements_by_class_name(pathValue)
        elif pathType == 'name':
            return self.current_driver.find_elements_by_name(pathValue)
        elif pathType == 'css_selector':
            return self.current_driver.find_elements_by_css_selector(pathValue)
        elif pathType == 'xpath':
            return self.current_driver.find_elements_by_xpath(pathValue)
        elif pathType == 'link_text':
            return self.current_driver.find_elements_by_link_text(pathValue)
        elif pathType == 'tag_name':
            return self.current_driver.find_elements_by_tag_name(pathValue)
        elif pathType == 'partial_link_text':
            return self.current_driver.find_elements_by_partial_link_text(pathValue)

    def element_is_exist(self, name):
        if len(self.find_elements(name)) == 0:
            return False
        return True

    def alter(self):
        return self.current_driver.switch_to.alert()

    def is_alter_present(self):
        try:
            self.current_driver.switch_to.alert()
            return True
        except Exception as e:
            print(e)
            return False


    # 鼠标右键
    def right_click(self, element_name=None):
        ac = ActionChains(self.current_driver)
        return ac.context_click(self.find_element(element_name)).perform()

    def forward(self):
        self.current_driver.forward()

    def back(self):
        self.current_driver.back()
if __name__ == '__main__':
    dr = Driver(1)
    dr.open_browser('http://www.multilotto.com/en')
    time.sleep(2)
    #dr.find_element("百度搜索输入框").send_keys("haha")
    print(dr.is_alter_present())
    dr.find_element("首页弹窗确认").click()
    time.sleep(10)
    print(dr.is_alter_present())
    dr.find_element("首页图标").click()
    #dr.close_brwser()
    
    --------------------------elementread--------------------
    import xml.etree.cElementTree as MyElementTree


class ElementsRead:

    def __init__(self):
        xml_file_path = 'D:\\MLUIproject-master\\testFile\\config.xml'  # 文件路径
        self.tree = MyElementTree.parse(xml_file_path)
        self.root = self.tree.getroot()

    # 获取所有元素name，type，value，返回一个二维列表
    def get_all_elements(self):
        elements = []
        for children in self.root.findall('element'):
            name = children.find('name').text
            path_type = children.find('pathType').text
            path_value = children.find('pathValue').text
            t = [name, path_type, path_value]
            elements.append(t)
        return elements

        # 获取所有元素的name， 返回一个list
    def get_all_elements_name(self):
        temp_list = ElementsRead().get_all_elements()
        name_list = []
        for i in temp_list:
            name = i[0]
            name_list.append(name)
        return name_list

        # 根据元素的name，返回元素的定位方式path，如果元素不存在则返回none
    def getPathType(self, name):
        path_type_list = ElementsRead().get_all_elements()
        for i in path_type_list:
            if i[0] == name:
                return i[1]

        # 根据xml文件的name， 返回元素定位的value，如果元素不存在则返回none
    def getPathValue(self, name):
        path_value_list = ElementsRead().get_all_elements()
        for i in path_value_list:
            if i[0] == name:
                return i[2]


if __name__ == '__main__':
    r1 = ElementsRead()
    print(r1.get_all_elements())
    print(r1.get_all_elements_name())

--------------config.xml--------------------
<?xml version="1.0" encoding="UTF-8"?>
<page>
    <element>
        <name>百度搜索输入框</name>
        <pathType>id</pathType>
        <pathValue>kw</pathValue>
    </element>
    <element page = "首页">
        <name>首页图标</name>
        <pathType>class_name</pathType>
        <pathValue>logo-cont</pathValue>
    </element>
    <element page = "首页">
        <name>首页弹窗</name>
        <pathType>xpath</pathType>
        <pathValue>.//*[@id='modal']</pathValue>
    </element>
    <element page = "首页">
        <name>首页年龄弹窗确认</name>
        <pathType>xpath</pathType>
        <pathValue>.//*[@id='modal']/div[2]/a</pathValue>
    </element>
    <element page ="首页">
        <name>首页signin按钮</name>
        <pathType>class_name</pathType>
        <pathValue>go-login</pathValue>
    </element>
    <element page ="首页">
        <name>首页signup按钮</name>
        <pathType>class_name</pathType>
        <pathValue>go-sign</pathValue>
    </element>
    <element page ="登录页">
        <name>用户名输入框</name>
        <pathType>name</pathType>
        <pathValue>email</pathValue>
    </element>
    <element page = "登录页">
        <name>密码输入框</name>
        <pathType>name</pathType>
        <pathValue>password</pathValue>
    </element>
     <element page = "登录页">
        <name>登录提交按钮</name>
        <pathType>xpath</pathType>
        <pathValue>.//*[@id='wrapper']/div[2]/div[2]/div[2]/div[7]</pathValue>
    </element>
    <element page = "enterprise 测试前台">
        <name>enterprise access</name>
        <pathType>xpath</pathType>
        <pathValue>//*[@id="datacom"]/a[1]</pathValue>
    </element>
    <element page = "enterprise 测试前台">
        <name>all</name>
        <pathType>xpath</pathType>
        <pathValue>/html/body/div/dl[8]/dt/a</pathValue>
    </element>
    <element page = "enterprise 测试前台">
        <name>login</name>
        <pathType>id</pathType>
        <pathValue>loginBtn</pathValue>
    </element>
    <element page = "enterprise 测试前台">
        <name>logout</name>
        <pathType>id</pathType>
        <pathValue>logoutBtn</pathValue>
    </element>
     <element page = "enterprise 测试前台">
        <name>loginWelcome</name>
        <pathType>xpath</pathType>
        <pathValue>//*[@id="outputArea"]/dl[1]/dd/div/div</pathValue>
    </element>
     <element page = "enterprise 测试前台">
        <name>logoutWelcome</name>
        <pathType>xpath</pathType>
        <pathValue>//*[@id="outputArea"]/dl[1]/dd/div/p</pathValue>
    </element>
</page>
