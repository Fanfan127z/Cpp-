# 第一章

![项目开发框架逻辑图](C:\Users\11602\Desktop\Cpp天气大项目\项目开发框架逻辑图.JPG)

![一些实际开发中必须要清楚的常识](C:\Users\11602\Desktop\Cpp天气大项目\一些实际开发中必须要清楚的常识.JPG)



![大佬的开发框架](C:\Users\11602\Desktop\Cpp天气大项目\大佬的开发框架.JPG)

![大佬的开发框架2](C:\Users\11602\Desktop\Cpp天气大项目\大佬的开发框架2.JPG)



![大佬的开发框架3](C:\Users\11602\Desktop\Cpp天气大项目\大佬的开发框架3.JPG)

# 第二章：如何开发永不停机的服务程序

## 如何开发永不停机的服务程序

首先，我们需要清楚的前端开发和后端开发的区别是：

==前端开发==所见即所得，重点是实现功能；

==后台开发==除了实现功能，还需要考虑服务程序的效率与稳定性。

用不停机的服务程序：

①用守护进程监控服务程序的运行状态；

②如果服务程序故障，则调度进程将重启服务程序。

③保证系统7x24小时不断地运行。

这一小节所学的内容，调度与守护是后台程序员普遍采用的方案，与编程语言无关，与业务无关的通用功能模块。本章节涉及的知识点比较多，但是难度不大，可以一边学知识，一边写代码，适应项目开发的节奏。==做项目的目的是为了解决业务的需求，把项目做好前提是必须熟悉业务的流程和数据。==在课程的最后一章节，将介绍如何获得业务开发经验。

### 2.1  生成测试数据-模块编写

1-实现生成测试数据的功能；

2-逐步掌握这个视频中大佬自己的开发框架的使用；

3-熟悉csv，xml，和json这三种常见的数据格式；

本项目，主要是用到2种测试数据，全国气象站点参数和全国气象站分钟观测数据。

全国气象站点参数：

![image-20220605225935185](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220605225935185.png)



![image-20220605230140304](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220605230140304.png)



#### 2.1.1.1  实现生成测试数据的功能；

业务要求：

①根据全国气象站点参数，模拟生成的观测数据；

②程序每分钟运行一次，每次生成839行数据，存放在一个文件（.csv文件）中。

代码逻辑：

1）搭建程序框架（运行的参数、说明文档、运行日志）。

2）把全国气象站点参数文件加载到站点参数的容器中。

3）遍历站点参数容器，生成每个站点的观测数据，存放在站点观测数据容器中。

4）把站点观测数据容器中的记录写入文件。

==该小功能的coding过程截图or文字总结记录：==

![image-20220605233739044](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220605233739044.png)



![image-20220611195556786](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220611195556786.png)

若按照上面的方式来创建并写入数据到一个文件中时，是存在漏洞的，比如你写的过程中，别的程序又读取了这个文件中的数据，这样就会造成共享数据读写不一致的问题！即便你可以使用多线程的相关知识来deal这个问题，但这很麻烦，不好！

而正常的方法如下所示：

![image-20220611195525228](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220611195525228.png)

xml文件格式的存储方式：

![image-20220611203452910](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220611203452910.png)

csv文件格式存储方式：

![image-20220611203520141](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220611203520141.png)

json文件格式存储方式：

![image-20220611203630555](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220611203630555.png)

![image-20220611210414621](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220611210414621.png)



##### ==小结经验：==

①在实际的项目开发过程中，我们最好一边写一小段代码就一边编译运行，看看有没有错误！（否则你一次性地写一大堆代码编译不通过那不就头都大了？!）

②CLogFile 这个日志类几乎必须出现在各种后台服务程序中，因此我要阅读好别人写好封装好的框架原代码，然后拿来为我所用！(当然，以后我必须也要自己造出属于自己的轮子，以供我自己使用来开发自己的框架！总不能一辈子都靠别人的框架吧对不？当然，我肯定从别人的框架中模仿过来造出属于自己的框架，这样子肯定是最好的情况啦！)

③要想学好技术，那么视频中的每一行代码都必须要亲自敲出来！

④小细节：==字符串变量==在每次使用前==最好都给其初始化==一下，否则程序==容易产生bug==，memset(strBuffer,0,sizeof(strBuffer));// 当然这里用char []字符数组作为字符串变量！

// 其实这就是《Effective C++》中 term04还是07教给我们的小技巧啦~

⑤在写大型项目的代码时，主要一定要编写边调试，可以写测试代码把东西打印输出到日志中！~

⑥在实际do项目时，绝大多数场景下都需要用到==时间time==的操作，因此务必要对我们这个框架中的时间操作有个深刻的学习并辅之以连续，以掌握it，然后用到我们后面自己的项目开发当中去！

#### 2.1.1.2  逐步掌握这个视频中大佬自己的开发框架的使用；





#### 2.1.1.3  熟悉csv，xml，和json这三种常见的数据格式；





### 2.2  服务程序调度-模块编写

#### 2.2.1  学习Linux==信号==的基础知识和使用方法；

kill -l命令可以查到linux下的共64种信号的信号值与对应的名字！

需要重点note的信号：

![image-20220612161343089](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612161343089.png)



![image-20220612161406910](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612161406910.png)



![image-20220612161435768](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612161435768.png)

打了加粗的信号肯定是场景用到的！

![image-20220612162442884](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612162442884.png)

==所以说，对于Cpp or其他任何一种后台（服务端）语言的开发岗位来说，信号是 very 重要的知识点！==



#### 2.2.2  学习Linux==多进程==的基础知识和使用方法；

多进程中经常用到SIGCHLD信号！

![image-20220612170417643](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612170417643.png)

![image-20220612170509420](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612170509420.png)

![image-20220612171507947](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612171507947.png)



![image-20220612172052618](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612172052618.png)

![image-20220612172136440](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612172136440.png)



当然，父进程中打开的文件描述符fd也同样会被复制到子进程中去！

如果父进程先退出，子进程将变成孤儿进程（由init进程将子进程资源回收掉）

如果子进程先退出，内核向父进程发送SIGCHLD信号，若父进程不处理这个信号，则子进程会成为==僵尸进程==！(这将会浪费系统资源！)

![image-20220612205140508](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612205140508.png)

defunct就代表该子进程变成僵尸进程的意思！

![image-20220612211935482](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612211935482.png)

==孤儿进程==其实没有什么危害，反正init1号进程会完成原来父进程的工作。

![image-20220612212238283](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612212238283.png)

但是，==僵尸进程的危害可就大了：==

![image-20220612205246034](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612205246034.png)

deal解决僵尸进程的方法：

①：在父进程中忽略内核给父进程发送的SIGCHLD信号！

test codes：

```c++
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<iostream>
#include<unistd.h>
#include<errno.h>
#include<sys/types.h>
#include<signal.h>
using namespace std;
int main(int argc,char* argv[]){
    signal(SIGCHLD,SIG_IGN);//在父进程中忽略SIGCHLD信号！以避免产生僵尸进程！
    pid_t pid = fork();
    if(pid < 0){
     printf("create child process error!\n");
     printf("Error no.%d: %s\n", errno, strerror(errno));
    }else if(pid == 0){
     printf("this is child process,pid == [%d],ppid == [%d]\n",getpid(),getppid());
     sleep(5);
    }else if(pid > 0){
     printf("this is father process,pid == [%d],ppid == [%d]\n",getpid(),getppid());
     sleep(10);// 子进程sleep5s，父进程sleep10s，让子进程先退出！从而形成僵尸进程！
    }
    return 0;
 }       
```

②：在父进程中添加等待子进程退出后并回收掉代码！

test codes：

```c++
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<iostream>
#include<unistd.h>
#include<errno.h>
#include<sys/types.h>
#include<signal.h>
using namespace std;
int main(int argc,char* argv[]){
    pid_t pid = fork();
    if(pid < 0){
     printf("create child process error!\n");
     printf("Error no.%d: %s\n", errno, strerror(errno));
    }else if(pid == 0){
     printf("this is child process,pid == [%d],ppid == [%d]\n",getpid(),getppid());
     sleep(5);
    }else if(pid > 0){
     printf("this is father process,pid == [%d],ppid == [%d]\n",getpid(),getppid());
     pid_t exit_pid = wait(NULL);
     if(exit_pid == -1){
     	printf("there is no child process already!\n");
     }
     printf("子进程成功回收掉~\n");
     sleep(10);// 子进程sleep5s，父进程sleep10s，让子进程先退出！从而形成僵尸进程！
    }
    return 0;
 }   
```

③：设置SIGCHLD的信号处理函数，在这个信号处理函数handler中一捕捉到SIGCHLD信号就会执行wait（以免因为第②种方法中父进程就调用wait阻塞卡在哪儿等待回收子进程而没法do别的事情了！）

```c++
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<iostream>
#include<unistd.h>
#include<errno.h>
#include<sys/types.h>
#include<signal.h>
using namespace std;
void handler(int sig){
    wait(NULL);
    printf("子进程成功回收掉~\n");
}
int main(int argc,char* argv[]){
    signal(SIGCHLD,handler);
    //在父进程中捕捉SIGCHLD信号！以干掉子进程先于父进程退出后所产生的僵尸进程！
    pid_t pid = fork();
    if(pid < 0){
     printf("create child process error!\n");
     printf("Error no.%d: %s\n", errno, strerror(errno));
    }else if(pid == 0){
     printf("this is child process,pid == [%d],ppid == [%d]\n",getpid(),getppid());
     sleep(5);
    }else if(pid > 0){
     printf("this is father process,pid == [%d],ppid == [%d]\n",getpid(),getppid());
     sleep(10);// 子进程sleep5s，父进程sleep10s，让子进程先退出！从而形成僵尸进程！
    }
    return 0;
 }      
```



#### 2.2.3  开发服务程序调度模块。

![image-20220613101018077](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220613101018077.png)





为了实现==调度==功能，步骤如下：
①先执行fork函数，创建一个子进程，让子进程调用execl系统调用来执行新的程序。
新程序将替换子进程，而不会影响父进程。                                                                           
②在父进程中，调用wait函数等待新程序运行并将其回收，这即可实现调度功能了！

代码：(这段代码我能学到非常重要的实际开发知识！very good！)

```c++
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<signal.h>
#include<sys/types.h>
#include<sys/wait.h>
#include<iostream>
using namespace std;                                                               
int main(int argc,char* argv[]){
   // 为了实现调度功能，步骤如下：
   // 先执行fork函数，创建一个子进程，让子进程调用execl系统调用来执行新的程序
   // 新程序将替换子进程，而不会影响父进程。
   // 在父进程中，可调用wait函数等待新程序运行并将其回收，这即可实现调度功能了！
   // ./procct1 5 /usr/bin/ls -lt /tmp/project.tgz
   // 這段代码的意思就是：每间隔5s钟执行一次指定的ls操作！
char* pargv[argc-1];
   // 这是把all输入参数放到一个新的字符串数组中
   // 方便后续使用excev系统调用来do变参输入时拉起程序的功能！
   // 此时不论你输入有多少个参数，都可以实现！比如拉起ls ls -l ls -a -h -l等等。。。
   // 若这里你不明白为什么要这样设置这个可变参数组的大小，你画个图对照着下面调用execl的语句就知道了~
      for(int ii = 2;ii < argc;++ii){
          pargv[ii - 2] = argv[ii];
      }
      pargv[argc - 2] = NULL;
      while(1){
          int ret = fork();
          if(ret < 0){
              printf("fork process failed!\n");
              break;
          }
          else if(ret == 0){
              // int eret = execl("/usr/bin/ls","/usr/bin/ls","-ltr","/tmp/tmp.txt",(char*)0);
              // int eret = execl(argv[2],argv[2],argv[3],argv[4],(char*)0);
              int eret = execv(argv[2],pargv);
              if(eret < 0){
                  printf("child process execl failed!\n");
                  break;
              }
          }else if(ret > 0){
              printf("this is father process~\n");
              wait(NULL);
              sleep(atoi(argv[1])); 
          }  
      }
}
```



![image-20220613110827617](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220613110827617.png)



==小结经验：==

①：kill和killall命令差不多，只是用kill命令使用信号时必须要知道进程的pid（进程号），但是killall就不需要知道进程pid，只需要知道该进程程序名字就能使用对应信号让该进程程序do相应的捕捉动作了！

②：killall -0 程序名字(可执行程序文件名字)可以查看该程序（进程）是否还存在；若存在，则不会有任何提示，也不会对运行中的进程产生任何影响；而若不存在了，则会打印出no process found的提示！

![image-20220612165535299](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220612165535299.png)

③：不论是考试面试，还是项目实际开发场景中，多进程都是重点！

④：在一个程序中，==拉起/调用别的程序/进程，====多使用execl or execv==，若忘记怎么用可直接百度！这比查看man手册还快的！

⑤：一般来说，对于一个服务程序，需要把所有的信号和IO都关掉！这么做的目的是不希望该程序被干扰，因为服务程序都是运行在后台的！！！

具体代码如下：

```c++
#include<signal.h>
// 关闭信号和IO，本程序不希望被打扰 
for(int ii = 0;ii <= 64;++ii){
    signal(ii,SIG_IGN);close(ii);
}
```

⑥：让一个程序运行在Linux的后台的代码：

```c++
/* 让该程序运行在后台
   生成子进程，父进程退出，让程序运行在后台，由系统init1号进程托管   */                       if(fork()!=0)exit(0);
// 启动SIGCHLD信号，让父进程可以wait子进程退出的状态
signal(SIGCHLD,SIG_DFL);
```

⑦：==调度程序==可以用来启动其他需要运行在Linux后台的任何服务程序！so调度程序是very 有用的！



调度程序最终代码如下：

```c++
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<signal.h>
#include<sys/types.h>
#include<sys/wait.h>
#include<iostream>
using namespace std;
int main(int argc,char* argv[]){
    if(argc < 3){// 这个程序若没有超过3个参数则必然是调用失败的！
        printf("Using:./procct1 timeItv program argvs ...\n");
        printf("Example:~/myProj/source_codes/project/tools1/bin/procct1 5 /usr/bin/tar zcvf /tmp/tmp.tgz /usr/include/ \n\n");
        printf("本程序是服务程序的调度程序，周期性启动服务程序或shell脚本。\n");
        printf("timeItv 运行周期，单位：秒。被调度的程序运行结束后，在timeItv秒后会被procct1重新启动\n");
        printf("program 被调度的程序名，必须使用全路径。\n");
        printf("argvs   被调度的程序的参数。\n");
        printf("注意，本程序不会被kill杀死，但可以使用kill -9强行杀死\n\n\n");
        return -1;
    }
    // 关闭信号和IO，本程序不希望被打扰
    for(int ii = 0;ii <= 64;++ii){
        signal(ii,SIG_IGN);close(ii);
    }
    // 让该程序运行在后台
    // 生成子进程，父进程退出，让程序运行在后台，由系统init1号进程托管
    if(fork()!=0)exit(0);
    // 启动SIGCHLD信号，让父进程可以wait子进程退出的状态
    signal(SIGCHLD,SIG_DFL);

    // 我们最常用的一个能运行拉起/运行其他程序的系统调用是
    // execl 和 execv
    // execl 是用参数中指定程序替换了当前进程的正文段，数据段，堆和栈
    // 因此一旦execl类的系统调用执行成功，当前进程的数据都发生变化了！再也不是原来的进程中所保存的数据了！
    // 为了实现调度功能，步骤如下：
    // 先执行fork函数，创建一个子进程，让子进程调用execl系统调用来执行新的程序
    // 新程序将替换子进程，而不会影响父进程。
    // 在父进程中，可调用wait函数等待新程序运行并将其回收，这即可实现调度功能了！
    
    // ./procct1 5 /usr/bin/ls -lt /tmp/project.tgz
    // 這段代码的意思就是：每间隔5s钟执行一次指定的ls操作！


    char* pargv[argc-1];// 这是把all输入参数放到一个新的字符串数组中
    // 方便后续使用excev系统调用来do变参输入时拉起程序的功能！
    // 此时不论你输入有多少个参数，都可以实现！比如拉起ls ls -l ls -a -h -l等等。。。
    // 若这里你不明白为什么要这样设置这个可变参数组的大小，你画个图对照着下面调用execl的语句就知道了~
    for(int ii = 2;ii < argc;++ii){
        pargv[ii - 2] = argv[ii];
    }
    pargv[argc - 2] = NULL;
    while(1){
        int ret = fork();
        if(ret < 0){
            printf("fork process failed!\n");
            break;
        }
        else if(ret == 0){
            // int eret = execl("/usr/bin/ls","/usr/bin/ls","-ltr","/tmp/tmp.txt",(char*)0);
            // int eret = execl(argv[2],argv[2],argv[3],argv[4],(char*)0);
            int eret = execv(argv[2],pargv);
            if(eret < 0){
                printf("child process execl failed!\n");
                exit(0);// 当然，若execl系统调用成功了，那么后面的exit(0)肯定是不会执行的！因为数据都给替换为新的程序的数据了！
            }
        }else if(ret > 0){
            printf("this is father process~\n");
            wait(NULL);
            sleep(atoi(argv[1])); 
        }

    }
    return 0;
}
```

```C
./procct1 60 ~/myProj/source_codes/project/idcMyCodes/c/crtsurfdata1 ~/myProj/source_codes/project/idcMyCodes/ini/stcode.ini /tmp/idc/surfdata ~/log/idc/crtsurfdata.log xml,json,csv
```

上面这行命令就是使用调度程序来 启动我服务器上的全国气象站点数据每分钟测试数据自动生成的调用代码！

==值得注意的是==：因为是在服务器端的后台运行的，因此你这样子每分钟生成3份文件（.csv|.xml|.json），一天之内可能你的云服务器的内存空间就耗尽了，so注意要及时kill - 9 掉对应的程序！否则的话你服务器饱满了你还怎么拿来学习对吧？

### 2.3  守护进程的实现

#### 2.3.1  学习Linux共享内存的基础知识和使用方法；

![image-20220613152812095](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220613152812095.png)

linux中各个进程间的内存空间是相互独立的，所以若想让进程间共享内存，就需要通过调用如上所述的4个系统调用来do！

```c
ipcs -m 命令可以查看所创建的共享内存！
ipcrm -m 对应的shmid号 命令可以删除所创建的共享内存！
```

![image-20220613160502552](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220613160502552.png)



#### 2.3.2  学习Linux信号量的基础知识和使用方法;

![image-20220622211143747](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220622211143747.png)

关于信号量的一个例子：

![image-20220622212032575](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220622212032575.png)



==补充==：

​	==二值信号量==是信号量的一种特殊形式，表示资源只有可用和不可用两种状态：0<->不可用；1<->可用。（这和互斥锁的功能类似，要么锁着，要么开着）。因此，我们页可以采用==信号量==来给共享资源加锁！

​	由于信号量的Linux系统调用API在使用上比较麻烦，因此我们这里使用开发框架中已经封装好信号量相关常用操作的==类CSEM==来写代码！

```c
ipcs -s 命令可以查看所创建的信号量的相关信息！
ipcrm sem 对应的semid号 命令可以删除所创建的信号量！
```

![Linux信号量3](C:\Users\11602\Desktop\Cpp天气大项目\Linux信号量3.JPG)







#### 2.3.3  开发守护进程模块，与调度模块结合协同工作，保证服务程序永不停机。

这里我们再次复习下我们本章节的==主要目的==：

①开发调度程序（procctl），用于启动服务程序，若服务程序（无论因何种原因）被kill，则60s后调度程序会再次启动！（这里我们写调度程序时就约定好60s后再次运行！当然你也可以自定义）

②开发守护进程，作用：检查服务程序是否死机！若服务程序死机（挂起），那么守护进程将终止它。然后当服务程序被终止后，调度程序（procctl）将重新启动它！

（procctl是调度程序名）

![image-20220623223850671](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220623223850671.png)

![image-20220623224247523](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220623224247523.png)

服务程序会不断把心跳信息写入到共享内存中，以表示自己（该程序）是活着的！

要实现守护进程，分==2个大步骤==：

①服务程序在共享内存中维护自己的心跳信息。

②开发守护程序，终止已经死机了的服务程序。

==守护进程==实现代码：==checkProcess.cpp==

```c++
#
```

注意：

①exit函数==不会调==用==局部对象==的析构函数。

②exit函数==会调==用==全局对象==的析构函数。

③return语句==会调用局部和全局对象==的析构函数。

### 2.4  开发2个常用的小工具（解决数据堆积问题）

1-开发==压缩文件==模块；==gzipfiles.cpp==

2-开发==清理历史数据文件==模块；==deletefiles.cpp==

这2个小工具都放在tools1工具目录下！



### 2.5  服务程序运行的策略（即启动我们工作中==开发项目的流程==如下，这是非常重要的技能）

正常来说，我们程序员在服务器上启动程序的可执行文件不可能是一行一行去启动的，一定是有个==总的启动脚本的！==

```bash
#############################
# start.sh：启动数据中心后台服务程序的脚本文件
# 使用该脚本方式：sh start.sh
#############################

# 检查服务程序是否超时，配置在/etc/rc.local中由root用户执行，在普通用户linzhuofan下就不要执行该行代码了！
~/myProj/source_codes/project/tools1/bin/procct1 300 ~/myProj/source_codes/project/tools1/bin/checkProcess

# 压缩数据中心后台服务程序的备份日志(0.04天之前的数据文件就压缩掉，以节省空间)
~/myProj/source_codes/project/tools1/bin/procct1 300 ~/myProj/source_codes/project/tools1/bin/gzipfiles /tmp/log "*.log.20*" 0.04

# 生成用于测试的全国气象站点观测的分钟数据
~/myProj/source_codes/project/tools1/bin/procct1 60 ~/myProj/source_codes/project/idcMyCodes/bin/crtsurfdata /tmp/idc/surfdata /tmp/log/crtsurfdata.log xml,json,csv

# 清理原始的全国气象站点观测的分钟数据目录(0.04天之前的数据文件就清理掉，因为无用了)
~/myProj/source_codes/project/tools1/bin/procct1 300 ~/myProj/source_codes/project/tools1/bin/deletefiles /tmp/idc/surfdata "*" 0.04
```

```bash
#############################
# killall.sh：清除/停止数据中心后台all的调度和服务程序的脚本文件
# 使用该脚本方式：sh killall.sh
#############################

# 注意：一定要先杀掉调度程序，否则你先杀掉服务程序在一定时间内调度程序还会重新启动他们
# 用-9参数杀掉调度程序procct1，注意是procct然后加个数字1！而不是l！！！
killall -9 procct1 
# 通知每个服务程序gzipfiles crtsurfdata deletefiles让他们自行退出
killall gzipfiles crtsurfdata deletefiles
# 休眠3秒，让服务程序有时间退出
sleep 3
# 再次强行杀掉每个服务程序gzipfiles crtsurfdata deletefiles，无论他们是否已经结束退出完毕
killall -9 gzipfiles crtsurfdata deletefiles
```

==如何在操作系统启动时把调度和服务程序启动起来呢？（这是非常重要的小技能点）==

![image-20220728184357485](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220728184357485.png)

==在root超级用户下打开文件： vi /etc/rc.local==

==（启动项目的服务程序，就是把项目的程序部署到服务器上！）==

```bash
# 检查服务程序是否超时(启动守护进程即可)
# 注意：守护进程必须是root用户去执行，别的用户无法杀掉！且守护进程不需要重启！否则就不能连续监控其他的进程程序了，风险非常大！
~/myProj/source_codes/project/tools1/bin/procct1 30 ~/myProj/source_codes/project/tools1/bin/checkProcess 
# 启动数据中心的后台服务程序
su - linzhuofan -c "/bin/sh ~/myProj/source_codes/project/idcMyCodes/c/start.sh"
# 切换到linzhuofan这个用户去执行sh start.sh的命令  
```

==然后在该终端下输入如下命令来添加可执行权限 ：chmod +x /etc/rc.d/rc.local== 

==然后你就可以重启下你的操作系统（重启你的服务器）：init 6==

==在root用户下输入：ps -ef | grep procct1看看是否由对应用户正确启动了==

![image-20220728185147970](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220728185147970.png)



### 2.6  第2章节总结

本章节，我们开发了如下4个任务：

①生成测试数据

②调度程序

③守护程序

④文件压缩和清除

大佬给出的==学习建议==：

①==多思考，少硬背！==

像本章节我们学习的信号量，共享内存登的知识点，大佬本人自己都记不住，但是他用到的时候就去百度去CSDN，查一下就出来了，这比你自己去做笔记更加高效！你只需要多思考和理解为啥要这么用，为啥我用这些linux系统编程or开发框架中的知识点就能do出这个开发任务就行了！大概知道有哪些框架内容可用，然后就知道如何去开发我们的任务了！

②==程序员是写出来的，而不是看出来的！==

你看再多的书or视频，不多多写代码，还是没用的！

③==养成查资料的习惯，培养自学能力！==

保持工作的态度：

①==把课程的内容当成工作去认真对待==。

②养成==严谨==的习惯，你考试可以临时抱佛脚蒙过去，但是你开发一个银行业务系统，10w条数据你的程序会弄错一条然后把人家的钱搞没了，这样还是0分！

③我们后台开发程序员必须要尽量写出==稳定、高效、简洁==的代码。

关于开发框架：

①掌握接口的使用方法即可，用代码多去测试和验证。

②==框架的源代码并不是重点==，现阶段你看不懂就算了呗，不用纠结！等我学完这个课程我的coding能力上去之后有时间再看看，没时间也可以不用看，能熟练掌握第①条建议其实足够用了！

关于Linux系统or网络编程

==要以本项目的开发中的需求为导向，掌握最常用的技术即可，现阶段无需深入研究！等我们把这个项目搞透之后，或者工作了之后再进行深入研究也未尝不可！==

==最后，项目中用到达知识点若你还没掌握，可以随时停下视频，然后去百度orCSDN，了解个大概之后再回来继续跟着看跟着敲！==



# 第三章：开发基于ftp协议的文件传输系统

我对自己的寄语：我应该总结我每一章学习的知识点和笔记，然后放到我自己de博客orGithub上面去，当然，我应该设置为私有的！这是我自己学到的东西)

## 开发基于ftp协议的文件传输系统

ftp协议很简单，so创建ftp服务很容易。且适用于内部网络环境。

要开发基于ftp协议的文件系统，需要完成如下几个任务or学习如下几个知识点。

**一、ftp的基础知识**（==基础先导知识==）

①ftp协议的基本概念。

②在CentOS7中安装和配置ftp服务。

③掌握ftp的常用命令。

**二、ftp客户端的封装**（==基础先导知识==）

①寻找开源的ftplib库，封装为C++的Cftp类。

②掌握Cftp类的使用方法。

（只要掌握它就能够开发自己的ftp文件传输系统了）

**三、文件下载功能的实现**（==较难coding的模块==）

①开发通用的文件下载模块，从ftp服务器上下载文件。

==文件的下载是数据采集子系统的一个模块。（把数据从其他地方拿过来）==

**四、文件上传功能的实现**（==较难coding的模块==）

①开发通用的文件上传模块，把文件上传到ftp服务器。

==文件的上传是数据分发和数据交换子系统的一个模块。==

**PS：大佬在video中说了，如果能把第三、四个模块熟练地完成的话，那就基本上是个合格的Cpp程序员了！**

### 3.1  ftp的基础知识

自行看这3个word文档然后对着自己的服务器操作操作即可了！

![tmp1](C:\Users\11602\Desktop\Cpp天气大项目\tmp1.JPG)



### 3.2  ftp客户端的封装（ftp客户端这个小==项目难点总结，可弄进简历中==）

==注：==不论是在什么操作系统，都有十分成熟且现成可用的ftp服务端，我们要自己实现的是==ftp客户端==的代码！

这个ftp客户端小项目的难点：如何区分文件上传和下载时文件是否已过期。（这是本项目的一个难点，这个难点可以总结到我的简历中，当作是项目经验！）

①在==文件下载==的时候，为了防止拉回来的是一个已经modified的文件，所以就用文件的时间来判断是否已经改变，若已经改变就说明文件已经过期，返回拉取下载文件失败。若是只用文件的大小是否改变是判断不出来下载的文件是否变化了的，比如原来的aa变为现在的bb，大小是一样的，但是已经修改了！

②而==文件上传==时，我们能够保证不修改这个要上传到文件，而服务器那边要验证是否是已经modified的文件就只能够用文件的大小来判断，只要文件创建的时间在上传前和上传后一样的话，就说明这个文件安全完好无损地上传到服务器了！

### 3.3  文件下载功能的实现（==文件下载模块的小亮点总结，可弄进简历中==）

有1个重要的技术点

==采用xml的形式==作为可执行程序的输入参数

优势：在==程序输入参数比较多==的时候使用该方式很方便，也不容易出错！

可能面试官会提问？那你怎么把xml的输入参数转换为对应的你要的实际参数呢？我会使用框架中的XML解析函数，有专门的函数能够解析出来！其实就是把xml的格式都去除后就能解析出对应的参数了（不论是字符串还是各种类型数字都能解析出来）当然，更加复杂的XML解析C库也是有的，比如百度搜libxml就有各种开源的库，要是话再按需去了解去使用即可！

文件下载到三种常见需求：

①增量下载文件，每次只下载新增的和修改过的文件

②下载文件后，删除ftp服务器上到文件

③下载文件后，把ftp服务器上到文件移动到备忘目录

### 3.4  文件上传功能的实现



小总结：

①一般来说，在开发中，我们的程序出问题了，一般都是==优先查看日志==，看里面记录了什么东西，哪里出了问题就去哪儿debug！！！==写日志就是为了方便你或者别人日后来维护这个程序的！==

②



