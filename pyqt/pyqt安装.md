# 1.安装Pyqt5相应的包

> `pip install sip`
> `pip install pyqt5`
> `pip install pyqt5-tools`

# 2.配置PyCharm

>![qt-designer](http://reblmr14h.hb-bkt.clouddn.com/note/20220706162249.png)
>
> - 添加“Qt Designer”：用于可视化设计Qt界面。
>![20220706163419](http://reblmr14h.hb-bkt.clouddn.com/note/20220706163419.png)
>
> ```json
> Name： Qt-Designer
> Program：在Anaconda创建的环境目录中搜索"designer.exe" 。（例如："D:\Anaconda3\envs\torch\Library\bin\designer.exe"）
> Arguments：不填
> working directory：$FileDir$
> ```
>
> - 添加“pyuic”：用于将".ui"文件转换为".py"文件。
> ![20220706163641](http://reblmr14h.hb-bkt.clouddn.com/note/20220706163641.png)
> 
> ```json
> Name： PyUIC
> Program：在Anaconda创建的环境目录中搜索"pyuic5.exe" 。（例如："D:\Anaconda3\envs\torch\Scripts\pyuic5.exe"）
> Arguments：$FileName$ -o $FileNameWithoutExtension$_rc.py
> working directory：$FileDir$
> ```

# 3.运行相应的工具

>![20220706164144](http://reblmr14h.hb-bkt.clouddn.com/note/20220706164144.png)