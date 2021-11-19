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





> 参考

[Python-Datawhale-Github官方指定](https://github.com/datawhalechina/team-learning-program/blob/master/OfficeAutomation/Task01%20%E6%96%87%E4%BB%B6%E8%87%AA%E5%8A%A8%E5%8C%96%E4%B8%8E%E9%82%AE%E4%BB%B6%E5%A4%84%E7%90%86.md)

[文件与文件系统拓展](https://github.com/datawhalechina/team-learning-program/blob/master/PythonLanguage/17.%20%E6%96%87%E4%BB%B6%E4%B8%8E%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F.md)
