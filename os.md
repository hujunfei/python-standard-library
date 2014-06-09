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
适用平台：Unix

####os.getegid()
返回当前进程的有效组ID。与执行该文件的当前进程的"set id"位相对应。  
适用平台：Unix

####os.geteuid()
返回当前进程的有效用户ID。  
适用平台：Unix

####os.getgid()
返回当前进程的真实组ID  
适用平台：Unix

####os.getgroups()
返回当前进程相关的组ID列表  
适用平台：Unix

> 注意：在Mac OS X上，<code>getgroups()</code>的行为与其他Unix平台不同。若Python解释器在10.5及之前的版本上构建的，<code>getgroups()</code>返回当前用户进程相关联的有效组ID列表；这个列表受限于系统定义的项数，一般是16，可以通过授权条件下调用<code>setgroups()</code>来修改。  
若在10.5以上版本上构建，则<code>getgroups()</code>返回与进程的有效用户ID相关联的用户当前可访问组列表(？有点不通顺，不明其意)；在进程的生命周期，组可访问列表可能会改变，调用<code>setgroups()</code>不会影响该列表，长度也不会受限于16。布署目标机器的版本值<code>MACOSX_DEPLOYMENT_TARGET</code>可以通过<code>sysconfig.get_config_var()</code>来获取。

####os.initgroups(username, gid)
调用系统函数<code>initgroups()</code>初始化用户组访问列表，其中的用户组为指定<code>username</code>所属组，可指定组ID。  
适用平台：Unix  
2.7版本新增

####os.getlogin()
返回进程所在控制终端的登录用户名。若需要更多用途，可以使用环境变量<code>LOGNAME</code>，或<code>pwd.getpwuid(os.getuid())[0]</code>来获取当前有效用户ID的登录名。  
适用平台：Unix

####os.getpgid(pid)
返回进程pid的组ID。若pid为0，则返回当前进程的组ID。
适用平台：Unix

####os.getpgrp() 
返回当前进程组ID。
适用平台：Unix

####os.getpid()
返回当前进程ID。
适用平台：Unix, Windows

####os.getppid()
返回父进程ID
适用平台：Unix

####os.getresuid()
返回一个元组(ruid, euid, suid)，表示当前进程的真实、有效和保存用户ID。
适用平台：Unix
2.7新增

####os.getresgid()
返回一个元组(rgid, egid, sgid)，表示当前进程的真实、有效和保存用户组ID。
适用平台：Unix
2.7新增

####os.getuid()
返回当前进程的真实用户ID。
适用平台：Unix

####os.getenv(varname[, value])
若环境变量*varname*存在，则返回变量值，否则返回*value*。默认*value*为<code>None</code>
适用平台：Unix的多数版本，Windows

####os.putenv(varname, value)
设置环境变量*varname*为字符串*value*。修改会影响由<code>os.system()</code>, <code>popen()</code>或<code>fork()</code>以及<code>execv()</code>。
适用平台：Unix的多数版本，Windows
> 注意：在FreeBSD和Mac OS X等平台上，设置<code>environ</code>可能导致内存泄露。参考相应平台的系统手册。

若当前平台支持<code>putenv()</code>函数，调用<code>os.environ()</code>自动转换为调用相应的<code>putenv()</code>函数。尽管如此，调用<code>putenv()</code>不会更新<code>os.environ</code>，所以最好设置<code>os.environ</code>。

####os.setegid(egid)
设置当前进程的有效组ID
适用平台：Unix


 