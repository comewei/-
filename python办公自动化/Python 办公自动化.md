## Python 办公自动化

### Task 01



遇到以下bug（文件中zip存在，路径也没有错）：

```
BadZipFile: File is not a zip file
```

原因：之前声明的压缩文件对象没有关闭



- 作业一

  - ```
    import os
    import shutil
    
    def copy_filetype(path, type):
        for folderName, subFolders,fileNames in os.walk(path):
            for filename in fileNames:
            #print('File Inside ' + folderName+':'+filename)
                if filename.endswith(type):
                    # 复制的目录一定要先存在，不然生成无拓展名的文件
                    shutil.copy(os.path.join(folderName, filename),'/home/xxxx/Task01')
    # 给出遍历地址
    path='/root/Datawhale/'
    #
    type='txt'
    copy_filetype(path, type)
    
    # reference:https://blog.csdn.net/zzl49689981/article/details/117898291
    ```

  - ```
    def remove_big_flies(path, size):
        for dirpaths, subFolders,fileNames in os.walk(path):
            for fileName in fileNames:
                    if os.path.getsize(os.path.join(dirpaths, fileName))>= size:
                        flag = input('{} 是否删除(y/n)'.format(fileName))
                        if flag=='y':
                            #print(dirpaths)
                            os.unlink(os.path.join(dirpaths,fileName))
                        else:
                            pass
                        
    path = '/root/Datawhale/'
    size = 10  * 1024
    remove_big_flies(path, size)
    ```

  - 作业三不太会

- 总结：
  - 这一次任务大概学习了整整半天，还有很多不太会的地方。对于Python读写文件有了进一步的了解，会一些基本的读写操作命令，但是有些作业还是完成的不太好。多多练习，多多实践。



### Task 02

这一章节还算简单，主要对Excel的读取，通过导入包建立对象进行赋值。这个有个比较有趣的点就是使用的python版本不能读取中文字符串文件名。加深了对于迭代的了解，使用索引和迭代项。

作业打算使用读取多个格子的方式来，但是没能成功，不能直接在cell使用[]索引

作业：

```
from openpyxl import load_workbook
from openpyxl.styles import Font, Side, Border
workbook = load_workbook('new_test.xlsx')
sheet = workbook.active
buy_mount = sheet["F"]
row_lst = []
for cell in buy_mount:
    if isinstance(cell.value, int) and cell.value > 5:
        print(cell.row)
        row_lst.append(cell.row)
side = Side(style='thin', color='FF000000')
border = Border(left=side, right=side, top=side, bottom=side)
font = Font(bold=True, color='FF0000')
for row in row_lst:
    for cell in sheet[row]:
        cell.font = font
        cell.border = border
workbook.save('new_test.xlsx')
```





### Task 03

这次学习的是python-word,主要还是生成对象，很多和word里面操作的属性类似。这里在excel到word中，利用了上节课的知识点，在设置的时候，使用了函数，没有直接嵌入循环，比较简洁。

作业：

```
# 导入库
from docx import Document
from docx.shared import RGBColor, Pt, Inches, Cm
from docx.enum.text import WD_PARAGRAPH_ALIGNMENT
from docx.oxml.ns import qn
from docx.shared import RGBColor,Pt

def produce_doc(i, r0, r1, r2, r3):

    doc = Document()

    heading_1 = doc.add_heading("邀请函", level = 0)
    heading_1.alignment = WD_PARAGRAPH_ALIGNMENT.CENTER

    # 新增段落
    paragraph_1 = doc.add_paragraph()

    # 设置段落的格式
    '''
    设置段落格式：首行缩进0.75cm，居左，段后距离1.0英寸,1.5倍行距。
    '''
    paragraph_1.paragraph_format.first_line_indent = Cm(0.75)
    paragraph_1.paragraph_format.alignment = WD_PARAGRAPH_ALIGNMENT.LEFT
    paragraph_1.paragraph_format.space_after = Inches(1.0)
    paragraph_1.paragraph_format.line_spacing = 1.5
    paragraph_1.add_run("尊敬%s的公司%s%s，您好" %(r0, r1, r2))
    paragraph_1.add_run('just for testing').font.underline = True

    paragraph_2 = doc.add_paragraph()
    paragraph_2.add_run('邀请您参加活动！')


    paragraph_3 = doc.add_paragraph()
    paragraph_3.paragraph_format.alignment = WD_PARAGRAPH_ALIGNMENT.RIGHT
    paragraph_3.add_run('邀请时间：%s' %r3)
    doc.save('test%s.docx' %i)
    
    
import openpyxl

wb = openpyxl.load_workbook('excelToword.xlsx')
sheet = wb.active
cells = sheet['A1:D5']
i = 0
for row in sheet.iter_rows(min_row = 2, max_row = 5,
                           min_col = 1, max_col =4):
    i = i + 1
    produce_doc(i, row[0].value, row[1].value, row[2].value, row[3].value)
```



### Task-04

这里使用pdf__writer进行读写的时候，出现`UnicodeEncodeError: 'latin-1' codec can't encode characters in position 8-9: ordinal not in range(256)`  bug,发现之后。想通过在`with open(save_path, "wb") as out:`代码加入encoding = 'utf-8'   但是不成功，之后发现原文直接修改源代码去了。牛逼。

感觉通过几次Task任务的模仿，对于文件的读写操作更加熟练了。这次Task04对我而言，提取原文件的表格功能最为友好，可以很方便的进行操作。组合上一次的excel生成word，这次使用pdf加入会更为有趣。

[参考水印](https://mp.weixin.qq.com/s/_oJA6lbsdMlRRsBf6DPxsg)



### Task-05

> Requests 简介

Requests可以很方便的对网络数据爬取，爬虫过程中也是最常用的发起第三方库。

在jypyter中安装：

```
!pip install requests
```

| re.status_code | 响应HTTP状态码       |
| :------------- | -------------------- |
| re.text        | 响应内容的字符串形式 |
| re.content     | 响应内容的二进制形式 |
| re.encoding    | 响应内容的编码       |

- > ​	访问百度

  - 通过访问requests来访问网址，可以通过上述`re.status_code` 得到相应码——这里涉及到部分计算机网络的知识（不熟悉的可以看看网络的状态码代表）

  - ```python
    import requests
    # 发出请求
    re = requests.get("https://www.baidu.com")
    # 查看响应码  如果为200，则代表响应成功
    print(re.status_code)
    
    # 查看一下响应的字符串格式
    print(re.text)
    ```

  - res.text返回的是服务器响应内容的字符串格式，也就是文本内容

- > 下载txt文件

  - 使用爬虫爬取`孔乙己`的文章，[网址](https://apiv3.shanbay.com/codetime/articles/mnvdu)

  - ```python
    import requests
    # 发出请求
    re = requests.get("https://apiv3.shanbay.com/codetime/articles/mnvdu")
    # 查看响应码
    print('the requests status_code is %s' %re.status_code)
    
    with open('鲁迅文章.txt', 'w') as file:
        # 把数据的字符串格式传入文件中
        print('正在爬取小说')
        file.write(re.text)
    ```

  - 打开本地目录验证是否爬取成功

- > 下载图片

  - 图片、视频、音频等比文本复杂的文件，我们会使用到requests的另一个属性`re.content`

  - ```python
    import requests
    # 发出请求
    re = requests.get("https://img-blog.csdnimg.cn/20210424184053989.PNG")
    # 查看响应码
    print('the requests status_code is %s' %re.status_code)
    
    with open('DataWhale.png', 'wb') as file:
        # 把数据的字符串格式传入文件中
        print('正在爬取图片')
        file.write(re.content)
    ```

  - 注意`re.encoding` 爬取内容的编码形似，常见的编码方式有ASCII、GBK、UTF-8等。如果和文件编码不同的方式去解码，我们会得到一些乱码。

> HTML解析宇提取

学习过HTML的同学，应该对于HTML的组成不陌生。可以参考网络上的HTML了解并且结合



> BeautifulSoup简洁

- BeautifulSoup在爬虫过程中的经常使用

- 在`jupyter`中安装：

  - ```python
    !pip install bs4
    ```

- 解析豆瓣读书TOP250

  - 网址：https://book.douban.com/top250

  - ```python
    import os
    import sys
    import requests
    from bs4 import BeautifulSoup
    
    """
     运行过程中，如果出现编码错误，可以使用sys来修改编码方式
     比如修改为'gb18030'
     sys.stdout = io.TextIOWrapper(sys.stdout.buffer,encoding='gb18030')
     """
    headers = {
    'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36'
    }
    # 需要爬取的网址
    res = requests.get('https://book.douban.com/top250', headers = headers)
    soup = BeautifulSoup(res.text, 'lxml')
    print(soup)
    ```
    
  - headers 的内容是对访问网址的设定，我们可以在Google点击F12进入开发者选项中查看headers里面的内容。![image-20211127143818001](E:\研究生\工作\Datawhale\study-in-graduate\python办公自动化\Python 办公自动化.assets\image-20211127143818001.png)
  
  - 注：
  
    - python 打印信息会有限制，我们将打印编码改成gb18030
  
    - headers代表请求头，==如果没有请求头会被服务器判断为爬虫而拒绝提供服务==
  
    - 使用 BeautifulSoup(res.text, lxmlr’) 语句将网页源代码的字符串形式解析成了 BeautifulSoup 对象 
  
    - 提取信息方面，BeautifulSoup 为我们提供了一些方法
  
      - | find()     | 返回符合条件的首个数据 |
        | ---------- | ---------------------- |
        | find_all() | 返回符合条件的所有数据 |
  
  - 观察 豆瓣读书TOP250网址，查看相关的元素
  
  - ```python
    import os
    import sys
    import requests
    from bs4 import BeautifulSoup
    
    """
     运行过程中，如果出现编码错误，可以使用sys来修改编码方式
     比如修改为'gb18030'
     sys.stdout = io.TextIOWrapper(sys.stdout.buffer,encoding='gb18030')
     """
    headers = {
    'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36'
    }
    # 需要爬取的网址
    res = requests.get('https://book.douban.com/top250', headers = headers)
    soup = BeautifulSoup(res.text, 'lxml')
    print(soup)
    
    # 使用find()   find_all()查找目标地址的标签<a></a>
    print(soup.find('a'))
    print(soup.find_all('a'))
    
    # 定义 html 中 div开头， 同时为 id 'doubanapp-tip'
    soup.find('div', 'doubanapp-tip')
    # 定义 span  开头，  class 为 'rating_nums'标签
    soup.find('span', 'rating_nums')
    ```



> 项目实践——爬取自如租房信息

爬取自如公寓官网：https://wh.ziroom.com/z/z/

点击官网的页码，可以得到：每一个页码网址都是后面  p  加入  数字，比如第一页为p1  第50页为p50

![image-20211127195606845](E:\研究生\工作\Datawhale\study-in-graduate\python办公自动化\Python 办公自动化.assets\image-20211127195606845.png)

点击其中一个房子看看：[创意天地保利心语一期朝南次卧合租租房价格信息_武汉洪山租房价格信息-自如网 (ziroom.com)](https://wh.ziroom.com/x/807075978.html) 访问房屋的网址，然后发现自己需要的信息，比如价格、面积、朝向、户型等。

点击`F12`进入开发者选项，定位需要元素的位置（比如价格）：

![image-20211127200241009](E:\研究生\工作\Datawhale\study-in-graduate\python办公自动化\Python 办公自动化.assets\image-20211127200241009.png)

开始操作：

- 导入需要爬虫的包：

- ```python
  import requests
  from bs4 import BeautifulSoup
  import random
  import time
  import csv
  ```

- 注意这里有一个问题，如果headers中的样式只有一个，那很容易被`反爬虫`识别

- ```python
  #这里增加了很多user_agent
  #能一定程度能保护爬虫
  user_agent = [
  "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50
  (KHTML, like Gecko) Version/5.1 Safari/534.50",
  "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-us) AppleWebKit/534.50 (KHTML,
  like Gecko) Version/5.1 Safari/534.50",
  "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:38.0) Gecko/20100101 Firefox/38.0",
  "Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; .NET
  CLR 2.0.50727; .NET CLR 3.0.30729; .NET CLR 3.5.30729; InfoPath.3; rv:11.0) like
  Gecko",
  "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)",
  "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0)",
  "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)",
  "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)",
  "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv:2.0.1) Gecko/20100101
  Firefox/4.0.1",
  "Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
  "Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131
  Version/11.11",
  "Opera/9.80 (Windows NT 6.1; U; en) Presto/2.8.131 Version/11.11",
  "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_0) AppleWebKit/535.11 (KHTML,
  like Gecko) Chrome/17.0.963.56 Safari/535.11",
  "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Maxthon 2.0)",
  "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; TencentTraveler 4.0)"
  "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)",
  "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; The World)",
  "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SE 2.X
  MetaSr 1.0; SE 2.X MetaSr 1.0; .NET CLR 2.0.50727; SE 2.X MetaSr 1.0)",
  "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; 360SE)",
  "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Avant Browser)"]
  ```

- 现在可以开始爬取数据了

- 根据需要的数据爬取保存在csv文件中，导入`csv`包，并简单的了解了`csv`包的用法

  1. 获得房屋的数字标签，进行定位

     多次点击不同的房子，发现房屋的名称的属性均为   <a href="//wh.ziroom.com/x/808092210.html" target="_blank">合租·阳城景园4居室-南卧</a>

     ```html
     <a href="//wh.ziroom.com/x/808092210.html" target="_blank"></a>
     ```

  2. ```python
     # 爬取自如前50页需要数据
     def get_info1():
       csvheader = ['名称', '面积', '朝向', '户型', '楼层', '是否有楼梯', '绿化']
       with open('wuhan_ziru.csv', 'a+', newline='') as csvfile:
         writer = csv.writer(csvfile)
         writer.writerow(csvheader)
         for i in range(1, 50):
           print('目前正在爬取第%s页' %i)
           # 这个是干嘛的呀,懂了
           timelist = [1, 2, 3]
           print(' 反爬取 再等等  ')
           time.sleep(random.choice(timelist))  # 不要给对方服务器太大压力，哈哈哈哈哈
           
     
           url  = 'https://wh.ziroom.com/z/p%s/' %i
           headers = {'User-Agent': random.choice(user_agent)}
           r = requests.get(url, headers = headers)
           r.encoding = r.apparent_encoding
           soup = BeautifulSoup(r.text, 'lxml')
           # 定位需要查找信息的元素上一级
           all_info = soup.find_all('div', class_='info-box')
           print(' GAN 给老子爬')
           for info in all_info:
             href = info.find('a')
             if href !=None:
               href = 'https:' + href['href']
               try:
                 print('正在爬取%s' %href)
                 house_info = get_house_info(href)
               except:
                 print('难顶，进不去啊%s' %href)
     ```

  3. 点击进入房间的细节信息：

     ==观察需要的信息哪里==

     ![image-20211127205612361](E:\研究生\工作\Datawhale\study-in-graduate\python办公自动化\Python 办公自动化.assets\image-20211127205612361.png)

     ```html
     <div class="Z_home_info">
                 <div class="Z_home_b clearfix">
     									                    <dl class="">
                             <dd>11㎡</dd>
                             <dt>使用面积</dt>
                         </dl>
     									                    <dl class="">
                             <dd>朝北</dd>
                             <dt>朝向</dt>
                         </dl>
     									                    <dl class="">
                             <dd>3室1厅</dd>
                             <dt>户型</dt>
                         </dl>
     															            </div>
                 <div class="tip-tempbox" id="tip-tempbox">
                     <div class="distance_tip" style="right: -168.075px; bottom: 0.450012px; display: none;">
                         <i class="allow" style="left: 143px; bottom: -5px;"></i>
     					房源详情页标注的房源到地铁站距离实际为该房源所在小区到地铁站的步行距离。当小区面积较大时，距离有一定误差，标注距离仅供参考，请以实际看房为准。                </div>
                 </div>
     
                 <ul class="Z_home_o" style="height: auto; padding-bottom: 52px;">
     				                    <li>
                             <span class="la">位置</span>
                             <span class="va">
                             <span class="ad">小区距6号线唐家墩站步行约430米</span>
     							<i class="address_query"></i>                    </span>
                         </li>
     																																														                    <li>
                             <span class="la">楼层</span><span class="va">37/46</span>
                         </li>
     									                    <li>
                             <span class="la">电梯</span><span class="va">有</span>
                         </li>
     									                    <li>
                             <span class="la">年代</span><span class="va">2016年建成</span>
                         </li>
     									                    <li>
                             <span class="la">门锁</span><span class="va">智能门锁</span>
                         </li>
     									                    <li>
                             <span class="la">绿化</span><span class="va">25%</span>
                         </li>
     				
                 </ul>
     
             </div>
     ```

     ```python
     def get_house_info1(href):
       # 得到房屋信息
       time.sleep(1)
       headers = {'User-Agent': random.choice(user_agent)}
       response = requests.get(url=href, headers=headers)
       soup = BeautifulSoup(response, 'lxml')
       # <h1 class="Z_name"><i class="status "></i>自如友家·顶琇国际城·3居室-03卧</h1>
       name = soup.find('h1', class_='Z_name')
       sinfo = soup.find('div', class_='Z_home_b clearfix').find_all('dd')
       area = sinfo[0].text
       orien = sinfo[1].text
       area_type = sinfo[2].text
       dinfo = soup.find('ul', class_='Z_home_o').find_all('li')
       loucen = dinfo[1].find('span', class_='va').text
       dianti = dinfo[2].find('span', class_='va').text
       lvhua = dinfo[5].find('span', class_='va').text
       room_info =[name, area, orien, area_type, loucen, dianti, lvhua]
       return room_info
     #['名称', '面积', '朝向', '户型', '楼层', '是否有楼梯', '绿化']
     ```

  4. 爬取不成功，可能设置了反爬取机制。但是其中的思想还是可以学到的，真的不错



> 实践项目2：36kr信息抓取与邮件发送

- 之前的学习较为轻松，就是没有成功爬取数据。找到需要收集的数据，进行数据html的定位,通过列表对象存储信息在csv文件中，收集信息

- 这次实践2是对动态信息进行监控，一旦信息触发条件，把信息转投到手机上，从而让自己更快感知。具体路径： python爬虫-->通过邮件A发送-->服务器--->通过邮件B接收  

- 36kr网站：[快讯_融资_互联网_资本_科技_合并_最新快讯_36氪 (36kr.com)](https://36kr.com/newsflashes)

- QQ POP3服务器在==邮件设置==中开启

- 开始爬取

  1. ```python
     # 导入包
     import requests
     import random
     from bs4 import BeautifulSoup
     import smtplib # 发送邮件模块
     from email.mime.text import MIMEText # 定义邮件内容
     from email.header import Header # 定义邮件标题
     ```

  2. 比如现在网站第一个是

     ```html
     <a class="item-title" rel="noopener noreferrer" target="_blank" href="/newsflashes/1502982432095106" sensors_operation_list="page_flow">中信证券：围绕“三个低位”布局蓝筹</a>
     ```

  3. ```python
     # 发送邮件
     def send_email(context):
       # 通过QQ邮箱来发送
       title = '36kr message'
       subject = title
       msg = MIMEText(context, 'html', 'utf-8')
       msg['Subject'] = Header(subject, 'utf-8')
       msg['From'] = sender
       msg['To'] = receive
       # SSL协议端口号要使用465
       smtp = smtplib.SMTP_SSL(smtpserver, 465) # 这里是服务器端口！
       # HELO 向服务器标识用户身份
       smtp.helo(smtpserver)
       # 服务器返回结果确认
       smtp.ehlo(smtpserver)
       # 登录邮箱服务器用户名和密码
       smtp.login(user, password)
       smtp.sendmail(sender, receive, msg.as_string())
       smtp.quit()
     ```

  4. ```python
     # 这里要注意端口问题，因为这里QQ POP3和计算机网络中设定端口不一致
     smtpserver = 'smtp.qq.com'
     # 发送邮箱的用户名密码
     user = '871321767@qq.com'
     password = 'xxxx'
     sender = '871321767@qq.com'
     receive = 'xxxxxx@qq.com'
     
     user_agent = [
     "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
     "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
     "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:38.0) Gecko/20100101 Firefox/38.0","Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; .NET CLR 2.0.50727; .NET CLR 3.0.30729; .NET CLR 3.5.30729; InfoPath.3; rv:11.0) like Gecko",
     "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)",
     "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0)",
     "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)",
     "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)",
     "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
     "Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
     "Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131 Version/11.11",
     "Opera/9.80 (Windows NT 6.1; U; en) Presto/2.8.131 Version/11.11",
     "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_0) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.56 Safari/535.11",
     "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Maxthon 2.0)",
     "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; TencentTraveler 4.0)",
     "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)",
     "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; The World)",
     "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SE 2.X MetaSr 1.0; SE 2.X MetaSr 1.0; .NET CLR 2.0.50727; SE 2.X MetaSr 1.0)",
     "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; 360SE)",
     "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Avant Browser)"]
     ```

  5. ```python
     def main():
       print('正在爬取数据')
       url = 'https://36kr.com/newsflashes'
       headers = {'User-Agent': random.choice(user_agent)}
       response = requests.get(url, headers=headers)
       response=response.content.decode('utf-8', 'ignore')
       soup = BeautifulSoup(response, 'lxml')
       news = soup.find_all('a', class_="item-title")
       news_list = []
       for i in news:
         # 这里可以 .text 吗 等下试一下
         # 测试了一下，也可以
         title = i.get_text()
         href = 'https://36kr.com' + i['href']
         news_list.append(title + '<br>' + href)
       info='<br></br>'.join(news_list)
       print(info)
       send_email(info)
     main()
     #print(info)
     ```

- 感悟：完成了Python-Whale的打卡活动，这次活动触动比较大的是很多组员的笔记做的很不错。也正是这样的一个机遇迫使我想要改变之前做笔记的一些方式，希望可以做的更好。这次Task05总的来说，我之前是科班的原因，学的比较轻松，但是对于什么时候需要爬取，在html哪里声明对象还不是很熟练。本来想爬取自如的价格，但是发现它是图片，点击进去看了一下，发现是按照位置来选择的，最后也没有去尝试。这次的电子邮件发送比较实用，很不错。 也十分感谢DataWhale这个平台。算是对我python编程对我的一个提升。工作人员很负责任，多谢工作人员。

> 参考

[Python-Datawhale-Github官方指定](https://github.com/datawhalechina/team-learning-program/blob/master/OfficeAutomation/Task01%20%E6%96%87%E4%BB%B6%E8%87%AA%E5%8A%A8%E5%8C%96%E4%B8%8E%E9%82%AE%E4%BB%B6%E5%A4%84%E7%90%86.md)

[文件与文件系统拓展](https://github.com/datawhalechina/team-learning-program/blob/master/PythonLanguage/17.%20%E6%96%87%E4%BB%B6%E4%B8%8E%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F.md)

