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





> 参考

[Python-Datawhale-Github官方指定](https://github.com/datawhalechina/team-learning-program/blob/master/OfficeAutomation/Task01%20%E6%96%87%E4%BB%B6%E8%87%AA%E5%8A%A8%E5%8C%96%E4%B8%8E%E9%82%AE%E4%BB%B6%E5%A4%84%E7%90%86.md)

[文件与文件系统拓展](https://github.com/datawhalechina/team-learning-program/blob/master/PythonLanguage/17.%20%E6%96%87%E4%BB%B6%E4%B8%8E%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F.md)
