该模块提供了一种使用依赖于操作系统函数的可移植方法。如果想读或写一个文件，参考<code>open()</code>；如果想操作路径，参考<code>os</code>.path模块；如果想读取命令行中所有文件的所有行，参考<code>fileinput</code>模块。如果要创建临时文件和目录，参考tempfile模块。高级文件和目录处理则参考<code>shutil</code>模块。
 

注意函数的可用性：
* Python所有内置的依赖于操作系统的模块设计原则是：如果有相同的函数功能可用，则使用同一接口。例如，函数<code>os.stat(path)</code>以同一格式返回路径的<code>stat</code>信息(源于POSIX接口)。
* 在os模块中，特定于某一操作系统的扩展仍然可用，但是使用它们对于可移植性是个挑战。
* “Availability: Unix”(即：适用于Unix)标注意味着该函数在Unix系列操作系统上普遍可用。它并不额外说明在某一特定操作系统上存在。
* 如果没有单独标注，所有标示为“Availability: Unix”的函数也支持Mac OS X(基于Unix内核编译)。

> 注意：如果文件名或目录不可用或无法访问，或者其他参数虽然具有正确的类型，但并不被操作系统所接受，该模块所有函数将会抛出<code>OSError</code>。

####*exception* os.error

    内置OSError异常的别名。

####os.name
    依赖于操作系统模块的名称。以下名称已经注册: 'posix', 'nt', 'os2', 'ce', 'java', 'riscos'。 

> 参考：<code>sys.platform</code>的粒度更细。<code>os.uname()</code>提供依赖于系统的版本信息。
<code>platform</code>模块提供对系统身份识别更细粒度的检查。

##15.1.1 进程参数
这些函数和数据项基于当前进程和用户提供信息和进行操作。
####os.environ
一个代表环境变量的<code>mapping</code>映射对象。例如，<code>environ['HOME']</code>是(某些平台上)home目录的路径，相当于C中的getenv("HOME")。
这个mapping在<code>os</code>模块首次引入时获取，通常在Python启动处理<code>site.py</code>时。对环境变量的修改在这之后不会体现到<code>os.environ</code>上，除非直接修改<code>os.environ</code>。
若当前平台支持<code>putenv()</code>函数，该mapping可以用来修改环境变量。当修改mapping时，<code>putenv()</code>会被自动调用。

> 注意：直接调用<code>putenv()</code>不会修改<code>os.environ</code>，所以最好修改<code>os.environ</code>

> 注意：在FreeBSD和Mac OS X等某些平台上，设置<code>environ</code>可以会导致内存泄露。参考<code>putenv()</code>的系统手册。

若不支持<code>putenv()</code>函数，该mapping的修改副本可能会传入相应的进程创建函数中，从而使子进程使用修改后的环境变量。

若当前平台支持<code>unsetenv()</code>函数，可以在该mapping中删除环境变量。当从<code>os.environ</code>中删除项，即<code>pop()</code>或<code>clear()</code>被调用时，<code>unsetenv()</code>会被自动调用。

*2.6版本修改*：调用<code>os.environ.clear()</code>和<code>os.environ.pop()</code>时也会清除环境变量。

#### os.chdir(path)
####os.fchdir(fd)
####os.getcwd()
这些函数在*__文件和目录__*中描述。

####os.ctermid()
根据进程的控制终端返回其文件名。
可用平台：Unix

####os.getegid()
返回当前进程的有效组ID。与执行该文件的当前进程的"set id"位相对应。
可用平台：Unix



 

 