2024年3月13日



## _exit



![image-20240313181859014](./.55_glibc/image-20240313181859014.png)

### posix

这里的应该是备选实现，即如果相应的OS平台上没有`_exit`函数的话，就使用这个。

![image-20240313181951233](./.55_glibc/image-20240313181951233.png)

#### abort

![image-20240313183641887](./.55_glibc/image-20240313183641887.png)

![image-20240313183724798](./.55_glibc/image-20240313183724798.png)

![image-20240313183747328](./.55_glibc/image-20240313183747328.png)

![image-20240313183812126](./.55_glibc/image-20240313183812126.png)

![image-20240313184001586](./.55_glibc/image-20240313184001586.png)

### sysdeps/unix/sysv/linux

![image-20240313182035284](./.55_glibc/image-20240313182035284.png)

### INLINE_SYSCALL (exit_group, ...)

![image-20240313202831737](./.55_glibc/image-20240313202831737.png)

![image-20240313202951140](./.55_glibc/image-20240313202951140.png)

![image-20240313203729492](./.55_glibc/image-20240313203729492.png)

![image-20240313203813147](./.55_glibc/image-20240313203813147.png)

#### internal_syscall1

![image-20240313203953601](./.55_glibc/image-20240313203953601.png)

#### SYS_ify

![image-20240313204131573](./.55_glibc/image-20240313204131573.png)

#### `__NR_exit_group` = 94

![image-20240313204341020](./.55_glibc/image-20240313204341020.png)

## exit

![image-20240313185014154](./.55_glibc/image-20240313185014154.png)

从`exit`到`_exit`，`status`的值没有改变过。

![image-20240313185048764](./.55_glibc/image-20240313185048764.png)

![image-20240313185125361](./.55_glibc/image-20240313185125361.png)

![image-20240313185152131](./.55_glibc/image-20240313185152131.png)

![image-20240313185216922](./.55_glibc/image-20240313185216922.png)

![image-20240313185243161](./.55_glibc/image-20240313185243161.png)

![image-20240313185309398](./.55_glibc/image-20240313185309398.png)

![image-20240313185349137](./.55_glibc/image-20240313185349137.png)

`exit`函数在最后会调用`_exit`函数，它是terminate一个进程的原始函数。



## kill

![image-20240314143018793](./.55_glibc/image-20240314143018793.png)

### __kill?

![image-20240314143134736](./.55_glibc/image-20240314143134736.png)

![image-20240314144721997](./.55_glibc/image-20240314144721997.png)

![image-20240314143254958](./.55_glibc/image-20240314143254958.png)

### __set_errno

![image-20240314144619563](./.55_glibc/image-20240314144619563.png)

### libc.pdf

成功返回0，不成功返回-1

![image-20240314153744939](./.55_glibc/image-20240314153744939.png)

![image-20240314153813986](./.55_glibc/image-20240314153813986.png)



## write

![image-20240314164622633](./.55_glibc/image-20240314164622633.png)

weak_alias

![image-20240314171202606](./.55_glibc/image-20240314171202606.png)

![image-20240314171315749](./.55_glibc/image-20240314171315749.png)

![image-20240314171400082](./.55_glibc/image-20240314171400082.png)

### SYSCALL_CANCEL(write, ...)

![image-20240314171531663](./.55_glibc/image-20240314171531663.png)

![image-20240314171611053](./.55_glibc/image-20240314171611053.png)

![image-20240314172128447](./.55_glibc/image-20240314172128447.png)

![image-20240314172202403](./.55_glibc/image-20240314172202403.png)

![image-20240314172241602](./.55_glibc/image-20240314172241602.png)

`__VA_ARGS__` = `write, fd, buf, nbytes`，因此**n = 3**。

![image-20240314172451610](./.55_glibc/image-20240314172451610.png)

因此，`__INLINE_SYSCALL_NARGS(__VA_ARGS__)` = 3。

所以，此时`INLINE_SYSCALL_CALL` = `__INLINE_SYSCALL3`

![image-20240314173250357](./.55_glibc/image-20240314173250357.png)

### INLINE_SYSCALL

![image-20240314173613803](./.55_glibc/image-20240314173613803.png)

![image-20240314173832480](./.55_glibc/image-20240314173832480.png)

![image-20240314173909530](./.55_glibc/image-20240314173909530.png)

#### internal_syscall3

![image-20240314174038409](./.55_glibc/image-20240314174038409.png)

![image-20240314174116010](./.55_glibc/image-20240314174116010.png)

#### __NR_write = 64

![image-20240321110342035](./.55_glibc/image-20240321110342035.png)

## wait

![image-20240321102929904](./.55_glibc/image-20240321102929904.png)



__wait

![image-20240321103055814](./.55_glibc/image-20240321103055814.png)



weak_alias

![image-20240321103147919](./.55_glibc/image-20240321103147919.png)

### __waitpid

![image-20240321103519936](./.55_glibc/image-20240321103519936.png)

![image-20240321103621752](./.55_glibc/image-20240321103621752.png)

### __wait4

![image-20240321103817154](./.55_glibc/image-20240321103817154.png)

![image-20240321103959952](./.55_glibc/image-20240321103959952.png)

`./posix/wait4.c`

![image-20240321104047845](./.55_glibc/image-20240321104047845.png)



### __wait4_time64

![image-20240321104409195](./.55_glibc/image-20240321104409195.png)

![image-20240321104601973](./.55_glibc/image-20240321104601973.png)

![image-20240321104650826](./.55_glibc/image-20240321104650826.png)

![image-20240321104719532](./.55_glibc/image-20240321104719532.png)

![image-20240321104750227](./.55_glibc/image-20240321104750227.png)

#### SYSCALL_CANCEL(wait4, ...)

详见write函数分析

`__VA_ARGS__` = `wait4, pid, stat_lock, options, usage`，因此**n = 4**。

如果是RV32：

`__VA_ARGS__` = `waitid, idtype, pid, &info, options, usage`，因此**n = 5**。

##### internal_syscall4

![image-20240321105917620](./.55_glibc/image-20240321105917620.png)



./sysdeps/unix/sysv/linux/riscv/sysdep.h

![image-20240321110016131](./.55_glibc/image-20240321110016131.png)

##### __NR_wait4 = 260

![image-20240321110533104](./.55_glibc/image-20240321110533104.png)

##### internal_syscall5

![image-20240321111233610](./.55_glibc/image-20240321111233610.png)

![image-20240321111314349](./.55_glibc/image-20240321111314349.png)

##### __NR_waitid = 95

![image-20240321110801058](./.55_glibc/image-20240321110801058.png)



## execvp



![image-20240321151810204](./.55_glibc/image-20240321151810204.png)

![image-20240321151849058](./.55_glibc/image-20240321151849058.png)

### libc_hidden_def

![image-20240322111703637](./.55_glibc/image-20240322111703637.png)

![image-20240326102828507](./.55_glibc/image-20240326102828507.png)

所以这里的实现对外是不可见的，另外的`execvp`实现在了别的库里？

#### unistd_maybe

![image-20240326102612770](./.55_glibc/image-20240326102612770.png)

### __execvpe

![image-20240321151951149](./.55_glibc/image-20240321151951149.png)

![image-20240321152033399](./.55_glibc/image-20240321152033399.png)

### __execvpe_common

![image-20240321152130580](./.55_glibc/image-20240321152130580.png)

![image-20240321152214974](./.55_glibc/image-20240321152214974.png)

![image-20240321152239745](./.55_glibc/image-20240321152239745.png)

![image-20240321152312606](./.55_glibc/image-20240321152312606.png)

![image-20240321152344776](./.55_glibc/image-20240321152344776.png)

![image-20240321152404207](./.55_glibc/image-20240321152404207.png)

### __execve

![image-20240321152904267](./.55_glibc/image-20240321152904267.png)

![image-20240321152954657](./.55_glibc/image-20240321152954657.png)

这应该是个stub函数。

#### syscalls.list

在文件`sysdeps/unix/sysv/linux/syscalls.list`中：

![image-20240326125147091](./.55_glibc/image-20240326125147091.png)



![image-20240326142720870](./.55_glibc/image-20240326142720870.png)

在`sysdeps/unix/make-syscalls.sh`中

![image-20240326145811989](./.55_glibc/image-20240326145811989.png)

因此，所有这些没有实现的syscall，应该都是由其它的代码和脚本间接生成的。

![image-20240326150033877](./.55_glibc/image-20240326150033877.png)

#### __rtld_execve

![image-20240322182723102](./.55_glibc/image-20240322182723102.png)

![image-20240322182446866](./.55_glibc/image-20240322182446866.png)

![image-20240322182858839](./.55_glibc/image-20240322182858839.png)



#### __NR_execve = 221

![image-20240322183955475](./.55_glibc/image-20240322183955475.png)





### maybe_script_execute

![image-20240321160557319](./.55_glibc/image-20240321160557319.png)

![image-20240321160715805](./.55_glibc/image-20240321160715805.png)

![image-20240321160738207](./.55_glibc/image-20240321160738207.png)



## vfork

![image-20240322110033028](./.55_glibc/image-20240322110033028.png)

![image-20240322110114180](./.55_glibc/image-20240322110114180.png)

###  __fork

![image-20240322110531294](./.55_glibc/image-20240322110531294.png)

![image-20240322110721234](./.55_glibc/image-20240322110721234.png)

![image-20240322112454864](./.55_glibc/image-20240322112454864.png)

### __libc_fork

![image-20240322112557630](./.55_glibc/image-20240322112557630.png)

![image-20240322112627579](./.55_glibc/image-20240322112627579.png)

![image-20240322112647131](./.55_glibc/image-20240322112647131.png)

![image-20240322112712706](./.55_glibc/image-20240322112712706.png)

![image-20240322112734293](./.55_glibc/image-20240322112734293.png)



### _Fork

![image-20240322112851313](./.55_glibc/image-20240322112851313.png)

./posix/_Fork.c中，stub_warning代表这是个stub。

![image-20240322112925527](./.55_glibc/image-20240322112925527.png)

./sysdeps/nptl/_Fork.c中的，才是真正的实现。

![image-20240322120253288](./.55_glibc/image-20240322120253288.png)

![image-20240322120311988](./.55_glibc/image-20240322120311988.png)

#### INTERNAL_SYSCALL_CALL

![image-20240322120646885](./.55_glibc/image-20240322120646885.png)

![image-20240322120742025](./.55_glibc/image-20240322120742025.png)

![image-20240322122215721](./.55_glibc/image-20240322122215721.png)

![image-20240314172241602](./.55_glibc/image-20240314172241602.png)

`__VA_ARGS__` = `set_robutst_list, $self->robust_head, sizeof (struct robuts_list_head)`，因此**n = 2**。

![image-20240314172451610](./.55_glibc/image-20240314172451610.png)

__INTERNAL_SYSCALL2

![image-20240322122058421](./.55_glibc/image-20240322122058421.png)

![image-20240314173909530](./.55_glibc/image-20240314173909530.png)

#### internal_syscall2

![image-20240322122504034](./.55_glibc/image-20240322122504034.png)

![image-20240322122535086](./.55_glibc/image-20240322122535086.png)