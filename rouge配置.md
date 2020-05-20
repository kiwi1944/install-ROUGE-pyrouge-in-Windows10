# 配置过程

## 0.写在前面

我在使用 pyrouge 时为了配置相关环境费了很大的功夫，网上与此相关的内容很少，我也是检索了 stackoverflow 、CSDN 以及各种博客上的资料才最终配置成功，所以对自己的配置过程做了以下记录。如果想了解更多关于 ROUGE 的内容，可以参考 Kavita Ganesan 老师的主页（https://kavita-ganesan.com/），我主要参考了以下两篇文章的内容：

https://kavita-ganesan.com/what-is-rouge-and-how-it-works-for-evaluation-of-summaries/#.XrVKxSC-uQI

https://kavita-ganesan.com/rouge-howto/#.XrVK9SC-uQI

如果你可以使用 java，而且已经安装了 java 1.8及以上版本，ROUGE 2.0可能会更适合你，具体的安装方法可以参考下面这个链接：

https://kavita-ganesan.com/rouge2-usage-documentation/

如果你在以下步骤操作中遇到了我没有提到过的问题，或者是已经全部完成了操作步骤，但还是无法正常使用 pyrouge，请尝试寻找资料独自摸索，我对此了解也不是非常深入，并不能保证以下步骤一定能帮你安装好 ROUGE。下面的内容可能有一些安装包下载非常缓慢，或者是链接失效，可以联系我（huangyixuannwpu@126.com）获取，也可以通过这个邮箱和我交流关于 ROUGE 配置的问题。

另外，python 上其实已经有不少关于 ROUGE 的包可以使用了，但是用时候需要先读入句子，并不是非常方便。如果你以后长期需要使用 ROUGE，建议你最好根据以下步骤尝试安装，如果你只是偶尔使用，且工程量不大，那么也有一些好用的包推荐给你。

### rouge

https://pypi.org/project/rouge/

链接内有这个包的详细介绍，用起来比较简单，有多种评测模型。但是貌似只能进行 hypothesis 和 reference 一对一的评测。

### py-rouge

https://pypi.org/project/py-rouge/

链接内对这个包进行了详细介绍，具体用法可见里面的 example，可以同时对多个 hypothesis 和多个 reference 进行评测，输出结果也非常全面。

## 1.所用平台、构架等：

1.win10

2.python3.7（anaconda）

3.jupyter notebook

4.strawberry-perl-5.30.2.1-32bit

5.ROUGE-1.5.5


## 2.安装 pyrouge 包

打开 powershell，输入语句

```
pip install pyrouge
```

## 3.安装 ROUGE-1.5.5

可以从这个链接下载：

https://github.com/andersjo/pyrouge/tree/master/tools/ROUGE-1.5.5

打开此链接后，只需要下载本页面内的全部内容即可。

## 4.设置 pyrouge 路径

有两个方法，任选其一即可。

### 方法1
在系统C盘建立文件夹 ROUGE，直接将步骤3中下载的内容（名为 ROUGE-1.5.5 的文件夹）放入其中即可。

### 方法2
首先，在系统任意位置放置步骤3中下载的内容（名为 ROUGE-1.5.5 的文件夹），比如放置在 C:\pyrouge-master\tools 文件夹下。

然后，找到一个名为 pyrouge_set_rouge_path 的文件（没有后缀），一般在 python 安装路径的 Scripts 文件夹内，比如在 C:\Anaconda\envs\py35\Scripts 内。

最后，打开 powershell，运行以下命令：

```
python C:\Anaconda\envs\py35\Scripts\pyrouge_set_rouge_path  C:\pyrouge-master\tools\ROUGE-1.5.5
```
## 5.进行前半部分内容的测试

打开 jupyter notebook，输入以下代码并运行，应该不会产生报错。

```python3
from pyrouge import Rouge155
r = Rouge155()
```
## 6.安装 perl

打开链接：http://strawberryperl.com/

在主页 Recommended version 中下载相应的安装包（.msi 后缀）即可，可以根据电脑位数选择，64位电脑安装32位安装包也可以成功（推荐下载最新版本，新版与旧版后续操作可能有部分差别）。

下载成功后双击安装即可，记得选择合适的安装路径并做记录。

安装成功后，找到安装文件夹内 perl.exe 所在位置，并将其所在文件夹添加到系统的 path 路径中。

比如， perl.exe 安装在了 D:\Program_Files\perl\perl\bin\perl.exe ，则应当把 D:\Program_Files\perl\perl\bin 添加到系统的path路径中。

## 7.安装 XML::DOM 插件

打开 powershell，输入以下命令：

```
cpan XML::DOM
```

## 8.重新生成 .db 文件

打开步骤4中放置的 ROUGE-1.5.5 文件夹，在其中的 data 文件夹内找到 WordNet-2.0.exc.db 文件，并删除。

然后在此文件夹内，不要选中任何文件或文件夹，按住shift键的同时点击鼠标右键，再点击“在此处打开 powershell 窗口”。打开后输入以下命令（可能会因为阅读界面被用了两行或以上显示，直接复制即可，只有一行，只有一句命令）：

```
perl WordNet-2.0-Exceptions/buildExeptionDB.pl ./WordNet-2.0-Exceptions ./smart_common_words.txt ./WordNet-2.0.exc.db
```

之后会发现原来的地方又生成了一个 WordNet-2.0.exc.db 文件。

## 9.修改 pyrouge 包中的内容

首先要找到你的 pyrouge 的安装包位置，一般在 python 安装位置的 ./Lib/site-packages 文件夹内。

找到 pyrouge 文件夹后，打开其中的 Rouge155.py 文件，找到下面这个函数（大多查看器可以用 ctrl+F 快捷键查找，我配置时，这个函数在319行）：

```python3
def evaluate(self, system_id=1, rouge_args=None)
```

然后找到该函数内的下面这条语句（这句话可能被分成两行写，请仔细查找）：

```python3
self.log.info("Running ROUGE with command {}".format(" ".join(command)))
```

在其之前加上这句代码：

```python3
command.insert(0, 'perl ')
```

## 10.ROUGE 测试

到上一步结束，已经基本完成了 ROUGE 的配置，接下来需要进行测试，以确保安装成功。

测试时不建议使用 pyrouge.test，建议按照以下步骤进行。

在你的电脑中任选文件夹（some_folder）建立以下内容：

```
some_folder:
│   rouge.py
│
├───model_summaries
│       text.A.001.txt
│
└───system_summaries
        text.001.txt
```

rouge.py 是你的python代码，其中内容为：

```python3
from pyrouge import Rouge155
r = Rouge155()

r.system_dir = 'system_summaries'
r.model_dir = 'model_summaries'
r.system_filename_pattern = 'text.(\d+).txt'
r.model_filename_pattern = 'text.[A-Z].#ID#.txt'

output = r.convert_and_evaluate()
print(output)
output_dict = r.output_to_dict(output)
```

text.A.001.txt 是你在测试中所设定的人工摘要，如果你想和我得到同样的 ROUGE 值以验证安装正确，可以在此文件中写入以下内容：

```
preprocess my summaries, then run ROUGE
```

text.001.txt 是你在测试中设定的自动生成摘要，同样，你也可以写入以下内容：

```
preprocess my summaries, then run ROUGE
```

接下来是输出内容：

```python3
2020-05-13 18:41:29,856 [MainThread  ] [INFO ]  Writing summaries.
2020-05-13 18:41:29,858 [MainThread  ] [INFO ]  Processing summaries. Saving system files to C:\Users\13093\AppData\Local\Temp\tmpss82_6zk\system and model files to C:\Users\13093\AppData\Local\Temp\tmpss82_6zk\model.
2020-05-13 18:41:29,859 [MainThread  ] [INFO ]  Processing files in system_summaries.
2020-05-13 18:41:29,860 [MainThread  ] [INFO ]  Processing text.001.txt.
2020-05-13 18:41:29,862 [MainThread  ] [INFO ]  Saved processed files to C:\Users\13093\AppData\Local\Temp\tmpss82_6zk\system.
2020-05-13 18:41:29,863 [MainThread  ] [INFO ]  Processing files in model_summaries.
2020-05-13 18:41:29,863 [MainThread  ] [INFO ]  Processing text.A.D001.txt.
2020-05-13 18:41:29,865 [MainThread  ] [INFO ]  Processing text.B.D001.txt.
2020-05-13 18:41:29,866 [MainThread  ] [INFO ]  Saved processed files to C:\Users\13093\AppData\Local\Temp\tmpss82_6zk\model.
2020-05-13 18:41:29,868 [MainThread  ] [INFO ]  Written ROUGE configuration to C:\Users\13093\AppData\Local\Temp\tmpk83mr63s\rouge_conf.xml
2020-05-13 18:41:29,869 [MainThread  ] [INFO ]  Running ROUGE with command perl  C:\ROUGE\ROUGE-1.5.5\ROUGE-1.5.5.pl -e C:\ROUGE\ROUGE-1.5.5\data -c 95 -2 -1 -U -r 1000 -n 4 -w 1.2 -a -m C:\Users\13093\AppData\Local\Temp\tmpk83mr63s\rouge_conf.xml
---------------------------------------------
1 ROUGE-1 Average_R: 1.00000 (95%-conf.int. 1.00000 - 1.00000)
1 ROUGE-1 Average_P: 0.42857 (95%-conf.int. 0.42857 - 0.42857)
1 ROUGE-1 Average_F: 0.60000 (95%-conf.int. 0.60000 - 0.60000)
---------------------------------------------
1 ROUGE-2 Average_R: 0.80000 (95%-conf.int. 0.80000 - 0.80000)
1 ROUGE-2 Average_P: 0.30769 (95%-conf.int. 0.30769 - 0.30769)
1 ROUGE-2 Average_F: 0.44444 (95%-conf.int. 0.44444 - 0.44444)
---------------------------------------------
1 ROUGE-3 Average_R: 0.50000 (95%-conf.int. 0.50000 - 0.50000)
1 ROUGE-3 Average_P: 0.16667 (95%-conf.int. 0.16667 - 0.16667)
1 ROUGE-3 Average_F: 0.25000 (95%-conf.int. 0.25000 - 0.25000)
---------------------------------------------
1 ROUGE-4 Average_R: 0.00000 (95%-conf.int. 0.00000 - 0.00000)
1 ROUGE-4 Average_P: 0.00000 (95%-conf.int. 0.00000 - 0.00000)
1 ROUGE-4 Average_F: 0.00000 (95%-conf.int. 0.00000 - 0.00000)
---------------------------------------------
1 ROUGE-L Average_R: 1.00000 (95%-conf.int. 1.00000 - 1.00000)
1 ROUGE-L Average_P: 0.42857 (95%-conf.int. 0.42857 - 0.42857)
1 ROUGE-L Average_F: 0.60000 (95%-conf.int. 0.60000 - 0.60000)
---------------------------------------------
1 ROUGE-W-1.2 Average_R: 0.69883 (95%-conf.int. 0.69883 - 0.69883)
1 ROUGE-W-1.2 Average_P: 0.42857 (95%-conf.int. 0.42857 - 0.42857)
1 ROUGE-W-1.2 Average_F: 0.53131 (95%-conf.int. 0.53131 - 0.53131)
---------------------------------------------
1 ROUGE-S* Average_R: 1.00000 (95%-conf.int. 1.00000 - 1.00000)
1 ROUGE-S* Average_P: 0.16484 (95%-conf.int. 0.16484 - 0.16484)
1 ROUGE-S* Average_F: 0.28303 (95%-conf.int. 0.28303 - 0.28303)
---------------------------------------------
1 ROUGE-SU* Average_R: 1.00000 (95%-conf.int. 1.00000 - 1.00000)
1 ROUGE-SU* Average_P: 0.19231 (95%-conf.int. 0.19231 - 0.19231)
1 ROUGE-SU* Average_F: 0.32258 (95%-conf.int. 0.32258 - 0.32258)
```

## 11.一些常见问题

### FileNotFoundError

如果在最后的测试中，你的输出结果与我的输出结果极其相似，而且在相应的文件夹内可以找到正确的 xml 文件，但是在 ROUGE 值输出前报错：

```
FileNotFoundError: [WinError 2] 系统找不到指定的文件。
```

你可以尝试重新启动计算机来解决这个问题。

如果重启之后还是不能解决，可以继续尝试下面的方法：

按下 win+X 键，点击 windows powershell 管理员 (A)，输入以下命令：

```
sfc /scannow
```

等待完成之后再进行尝试。

### perl.exe - System Error

```
The program can't start because \*.dll is missing from your computer. Try reinstalling the program to fix this problem.
```

一个可能的解决方案是，从你的 perl 的安装位置中，./c/bin 文件夹内找到对应的 .dll 文件，将其复制到 ./perl/bin 文件夹内。

### db file error

如果你遇到了如下错误：

```
Cannot open exception db file for reading: C:\Anaconda\pyrouge-master\tools\ROUGE-1.5.5\data/WordNet-2.0.exc.db
```

很有可能是你没有进行或没有成功进行 db 文件的重新生成，请根据之前的参考内容重新操作。

### OSError: [WinError 193]

如果你遇到了如下错误：

```
OSError: [WinError 193] %1 is not a valid Win32 application
```

很有可能是你缺少了以上所写的一些步骤，请重新检查一遍。




