该模块实现了一些有用的路径函数。读写文件参考<code>open()</code>，访问文件系统参考[<code>os</code>](os.md)模块。
> 注意：在Windows上，大多数函数无法有效支持UNC路径，<code>splitunc()</code>和<code>ismount()</code>可以正确处理它们。

与Unix Shell不同，Python不做任何自动路径扩展。应用需要类shell的路径扩展功能时，可以调用函数<code>expanduser()</code>和<code>expandvars()</code>(也可参考<code>glob</code>模块)。
> 注意：由于不同操作系统有不同的路径命名传统，在标准库中有该模块的不同版本。当前<code>os.path</code>模块是适用于Python所运行操作系统上的路径模块，因而适用于本地路径。尽管如此，如果想要操作不同格式的路径，可以引入基于不同OS的模块。它们具有相同的接口：
>
* <code>posixpath</code>: Unix风格路径  
* <code>ntpath</code>: Windows路径  
* <code>macpath</code>: 旧式MacOS路径  
* <code>os2emxpath</code>: OS/2 EMX路径  

####os.path.abspath(path)
返回*path*的标准化绝对路径。在多数平台上，相当于调用函数<code>normpath()</code>: normpath(join(os.getcwd(), path))。  
1.5.2版本新增

####os.path.basename(path)
返回*path*的basename。这是函数<code>split()</code>返回值对中的第二个元素。该函数的结果与Unix的**basename**程序不同，传入'/foo/bar/'时，**basename**返回'bar'，<code>basename()</code>函数返回空值('')。

####os.path.commonprefix(list)
返回*list*中所有路径前缀的最长值(按字符计算)。若*list*为空，返回空值('')。由于一次处理一个字符，因而可能返回一个不可用路径。

####os.path.dirname(path)
返回*path*的目录名。这是函数<code>split()</code>返回值对中的第一个元素。


####os.path.exists(path)
若*path*引用路径存在，返回True。坏软链返回False。在一些平台上，若未授权在所请求文件上执行<code>os.stat()</code>函数，即使*path*实际存在，该函数仍然返回False。

####os.path.lexists(path)
若*path*引用路径存在，返回True。坏软链返回True。在一些平台上，相同于缺少<code>os.lstat()</code>的<code>exists()</code>。  
2.4版本新增

