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



## 2.6  第2章节总结

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

**PS：大佬在video中说了，如果能把==第三、四个模块==熟练地完成的话，那就基本上是个==合格的Cpp程序员==了！**

（当我把源码看的差不多之后，可以自己手动从0->1实现一下这整个项目并画出逻辑结果图！再次感受一下这个项目的各种细枝末节！！！）



### 3.1  ftp的基础知识

自行看这3个word文档然后对着自己的服务器操作操作即可了！

![tmp1](C:\Users\11602\Desktop\Cpp天气大项目\tmp1.JPG)



### 3.2  ftp客户端源码的封装（ftp客户端这个小==项目难点总结，可弄进简历中==）

==注：==不论是在什么操作系统，都有十分成熟且现成可用的ftp服务端，我们要自己实现的是==ftp客户端==的代码！

这个ftp客户端小项目的难点：如何区分文件上传和下载时文件是否已过期。（这是本项目的一个难点，这个难点可以总结到我的简历中，当作是项目经验！）

①在==文件下载==的时候，为了防止拉回来的是一个已经modified的文件，所以就用文件的时间来判断是否已经改变，若已经改变就说明文件已经过期，返回拉取下载文件失败。若是只用文件的大小是否改变是判断不出来下载的文件是否变化了的，比如原来的aa变为现在的bb，大小是一样的，但是已经修改了！

②而==文件上传==时，我们能够保证不修改这个要上传de文件，而服务器那边要验证是否是已经modified的文件就只能够用文件的大小来判断，只要文件创建的时间在上传前和上传后一样的话，就说明这个文件安全完好无损地上传到服务器了！

### 3.3  文件下载功能的实现（==文件下载模块的小亮点总结，可弄进简历中==）

有1个重要的技术点

==采用xml的形式==作为可执行程序的输入参数

优势：在==程序输入参数比较多==的时候使用该方式很方便，也不容易出错！

可能面试官会提问？那你怎么把xml的输入参数转换为对应的你要的实际参数呢？

**答：**我会使用框架中的XML解析函数，有专门的函数能够解析出来！其实就是把xml的格式都去除后就能解析出对应的参数了（不论是字符串还是各种类型数字都能解析出来）当然，更加复杂的XML解析C库也是有的，比如百度搜libxml就有各种开源的库，要是话再按需去了解去使用即可！

下载功能图示：

![image-20220814214458761](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220814214458761.png)

文件下载的==三种常见需求==：

①增量下载文件，每次只下载服务器上新增的和被修改过的文件（==这个技术需求的实现是最难的==）

②下载文件后，删除ftp服务器上的文件

③下载文件后，把ftp服务器上到文件移动到备忘目录

（我们使用4个容器来实现！）



### 3.4  文件上传功能的实现

和上述完成的文件下载功能差不多，把上述的ftpgetfiles.cpp修改一下就可以实现文件的上传了！

上传功能图示：

![image-20220814214437791](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220814214437791.png)



完成前面的内容后，重新启动start.sh脚本！最终结果：

```bash
#可通过命令：tail -f /tmp/log/idc/*.log 来实时查询我项目的各个后台服务程序的日志情况！
```

![image-20220814235222448](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220814235222448.png)

### ==第1个小疑问点：==（这个小知识点可以写入我的项目总结点！）

==若有某些后台服务程序do事情失败 failed了！则很有可能是因为某些文件夹没有创建的缘故！可以逐个逐个排查故障在哪儿！==

比如我这里就是因为没有创建目录==/tmp/ftpputtest==下的这个文件！！！

![image-20220814235719883](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220814235719883.png)

创建成功之后：（很明显成功了！）

![image-20220814235759750](C:\Users\11602\AppData\Roaming\Typora\typora-user-images\image-20220814235759750.png)



### ==第2个小疑问点：==（这个小知识点也可以写入我的项目总结点！）

我当前后台服务程序，不论是上传 又或者是 下载的后台程序，随着时间的推移，上传or下载的文件量肯定是越来越多的了！那么此时就需要考虑到服务器或者客户端的磁盘容量的问题了。==为了解决磁盘空间被撑满的问题，我直接就写了2个清理上传 和 下载文件的后台服务程序，以便于即使是在磁盘空间非常少的云服务器上也可以运行这类比较大型的项目的后台程序！==

如果想查看是否有定时清除的话，大可以直接在对应的数据目录打印数据的各种信息(用ll命令)

比如我这里的：我这里设置了每间隔0.04天（0.04*24hours ==57.6分钟）就是每隔这么多分钟清理一次数据的意思！我可以每隔差不多60分钟看一次我的数据文档看看有没有满！！！

```bash
#ll /tmp/ftpputtest 或者  
# ll /idcdata/surfdata
```

### ==第3个小疑问点：==（这个小知识点也可以写入我的项目总结点！）

关于ftp.get()和ftp.put()函数，这里也有个小点可以深挖：

我这里项目开发的==上传文件功能模块==的算法中，是保证了一定能够核对上传到服务器端的文件与我本地的文件是否一致的！这样也可以防止别人和你扯皮子说我们没有上传文件什么的，这样子也是考虑到一些利益相关问题而开发的小细节把！

## 第3章节小总结：

①一般来说，在开发中，我们的程序出问题了，一般都是==优先查看日志==，看里面记录了什么东西，哪里出了问题就去哪儿debug！！！==写日志就是为了方便你或者别人日后来维护这个程序的！==

②如何在终端上实时地监控一个日志的更新信息呢？（方便用于调试和维护自己的服务程序）

#### ==如何在linux的bash终端一直持续监控日志呢？==答：一行代码搞定！且这个coding小技巧是非常重要的！

```bash
# 比如日志的路径名为：/tmp/log/idc/ftpgetfiles_surfdata.log
tail -f /tmp/log/idc/ftpgetfiles_surfdata.log
# 上面这行代码就可以让你在当前终端bash下实时监控该日志的信息！
```

③哪些程序会挂死？

```bash
1-涉及到网络通信、数据库操作的程序都很容易挂死（2~3天之内突然出状况程序死掉了）
2-比较复杂的程序可能会挂死
3-简单的程序基本上不会挂死
```

为了防止你开发的复杂的程序挂死，就比如本章节中开发的ftpgetfiles.cpp这种复杂但重要的程序。则必须要增加进程心跳的代码（框架中有对应的封装好的类以供我们使用：CPActive类,用于守护该后台进程，防止挂死后不do任何补救措施！）。

那么怎么个加心跳法呢？我们可以直接在需要消耗时间的子函数中去加心跳！放到日志里面即可！为了稳妥起见，多加一些进程心跳信息也没事儿！因为共享内存距离CPU非常接近，速度非常快！比SSD快100倍呢！

==④心得体会==：

ftp是很基本的tcp应用层协议！大部分程序员都应该都其了如指掌！大佬在讲课中所教授给我们的ftp文件传输项目和他在实际开发中用到模块是一毛一样的！！！他毫无保留！

对初学者来说，要掌握这个小模块项目，是有点难度的！但是，吃下这个项目后慢慢就会有程序员的开发思维了！通过这个项目，我们应该很清楚，项目开发并不是件非常容易的事情！有非常多的细节要点需要我们想清除并且考虑到位！你要上传or下载一个或者少量的文件其实很简单，难的其实是一个通用的上传和下载文件的ftp模块！（考虑各种事情！且一个程序的运行可能会需要10几个甚至更多参数！）

==⑤开发细节==：

解决问题的方法很重要，请优先考虑开源代码，不要从0开始造轮子！

（比如本章节就是从网上拿一个ftp客户端的C语言的源代码，然后封装一下来使用）

现实开发中的各种新需求，程序员都是这样的！千万不要盲目先动手造轮子！而是看看有没有开源库or框架等的代码库，有就抄过来，再修修补补改改，就能用拿来开发了！

2-我们框架中的Cftp类中，不会创建ftp服务端的目录！因为实际项目里，不同项目时由不同公司的人负责的！那么我要是在别人公司的服务器创建了目录，出了事情很可能会让我们自己背锅负责！这是要考虑到利益问题的！！！

⑥探讨效率：

ftp协议主要的应用场景是内部网络中不同业务系统之间进行数据交换的，效率不高也不太安全，但是非常简单。

==so，在下一个章节，我们将开发基于TCP协议的文件传输系统，它的性能远远超过ftp协议！==







# 第五章：MySQL数据库开发基础



对于C/CPP程序员来说，在平时的工作中，网络和数据库相关的开发工作是占大头的内容！！！网络相关的工作框架搭建好后基本就不需要改动了，但是数据库相关的工作需要时常更新！！！

当然，**在使用该MySQL的C/C++框架之前还需要设置一些小东西！**

这是我自己百度云服务器上面mysql的lib动态库文件所在安装路径！（后面使用到mysql框架时需要连接这几个动态库中的一个！）

（使用**whereis mysql **命令就可以查看你自己的服务器上面的mysql的各个文件安装的路径了！）

![image-20221008174518547](C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221008174518547.png)

同时，需要把使用到达该动态库赋值一份到/home/linzhuofan/lib中（这么do也是为了和实际开发贴合！因为实际开发中你没有sudo权限，只能这么干！），然后更新LD_LIBRARY_PATH！

我的系统编程笔记中有记录如何更新：

![image-20221008180337129](C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221008180337129.png)

更新后的结果如下：

![image-20221008180425630](C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221008180425630.png)

这样才能让g++ or makefile中g++ 编译我使用了mysql相关操作函数的C/C++程序时 **成功地编译！！！**（==这一点非常非常非常重要！==）

## 1：封装MySQL数据库开发的C-API（封装为Cpp的）

### 1.1：学习connection和sqlstatement类的使用方法 并运用于C/C++程序中操作MySQL数据库（增删改查）

connection和sqlstatement类是大佬已经封装好的使用C++操作sql的相关操作的类！拿来就用！！

以后我们在日常开发时，这2个类是经常要操作的！我们必须要对于这2个类的函数==**熟练掌握熟练于心**==！

**①connection类：**用来登录上MySQL数据库的类

**②sqlstatement类：**登录MySQL数据库成功后，需要写sql语句执行sql操作时，就需要用到这个类中的成员函数了！



#### 1.1.1：==创建SQL表格==demo C/C++代码

```c++
/*
 *  程序名：createtable.cpp，此程序演示开发框架操作MySQL数据库（创建表）。
 *  作者：林卓凡。
*/
#include "_mysql.h"
int main(int argc,char * argv[]){
    const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
        // if为正式的服务程序的话，我们会 把错误信息打印到日志中（）框架中的CLogFile类！方便维护该服务程序！
        // 但因为这个是demo 程序，就直接打印错误信息了！
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象
    /*
        当然上面这一条语句和下面这2条语句的意思是一毛一样的！！！
        sqlstatement stmt2;
        stmt2.connect(&conn);
    */
    // 准备创建表的SQL语句。
  // 超女表girls，超女编号id，超女姓名name，体重weight，报名时间btime，超女说明memo，超女图片pic。
  stmt.prepare("create table if not exists girls(id      bigint(10),\
                   name    varchar(30),\
                   weight  decimal(8,2),\
                   btime   datetime,\
                   memo    longtext,\
                   pic     longblob,\
                   primary key (id))");
    // 在C/CPP中，当你多行书写代码时，一定要记得加斜杠\!!!
  /*
  1、int prepare(const char *fmt,...)，SQL语句可以多行书写。
  2、SQL语句最后的分号;可有可无，建议不要写（兼容性考虑，考虑到兼容其他关系型数据库）。
  3、SQL语句中不能有说明文字。
  4、可以不用判断stmt.prepare()的返回值，stmt.execute()时再判断。
  */
    /* 
        执行sql语句，一定要判断返回值，0-成功，其他-失败
        失败代码在stmt.m_cda.rc中，失败描述在stmt.m_cda.message中。
    */
    if(stmt.execute() != 0){
       // 打印sql语句执行错误的信息！
        printf("stmt.execute() failed.\n%s\n%d\n%s\n",stmt.m_sql,stmt.m_cda.rc,stmt.m_cda.message);return -1;
    }
    return 0;
}
```



#### 1.1.2：==插入表格数据==的demo C/C++代码

```c++
/*
 *  程序名：inserttable.cpp，此程序演示开发框架操作MySQL数据库（插入信息到表中）。
 *  作者：林卓凡。
*/
#include "_mysql.h"
int main(int argc,char * argv[]){
    const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
        // if为正式的服务程序的话，我们会 把错误信息打印到日志中（）框架中的CLogFile类！方便维护该服务程序！
        // 但因为这个是demo 程序，就直接打印错误信息了！
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象
    /*
        当然上面这一条语句和下面这2条语句的意思是一毛一样的！！！
        sqlstatement stmt2;
        stmt2.connect(&conn);
    */
    // 定义用于超女表信息的结构体，与表中的各个字段一一对应（若不定义结构体，只定义表列字段对应的变量来存储对应信息也ok）
    // 但当表列字段太多时，比如100个，那你定义100个变量也太难管理了把？
    // 还不如当初直接用一个结构体来管理你表列各字段变量呢！
    struct st_girls{
        long   id;          // 超女编号
        // C语言中的每个字符串都有个多余的'\0',so在原字符串长度的基础上应该+1来定义该字符串（字符数组才ok）
        char   name[31];    // 超女姓名
        double weight;      // 超女体重
        char   btime[20];   // 超女报名时间
    }stgirls;
    // 准备 插入表格中的SQL语句！
    // stmt.prepare("\
    // insert into girls(id,name,weight,btime) values(:1+1,:2,:3+45.35,str_to_date(:4,'%%Y-%%m-%%d %%H:%%i:%%s'))");
        stmt.prepare("\
    insert into girls(id,name,weight,btime) values(?,?,?,str_to_date(?,'%%Y-%%m-%%d %%H:%%i:%%s'))");// 也ok但兼容性不如用:+参数序号的方式的兼容性好！
    printf("%s\n",stmt.m_sql);
    // 这里使用%%是为了转移%号！
    // 绑定输入变量的地址
    stmt.bindin(1,&stgirls.id);
    stmt.bindin(2,stgirls.name,30);
    stmt.bindin(3,&stgirls.weight);
    stmt.bindin(4,stgirls.btime,19);

    int total_insert_data_number = 0;
    // 模拟超女数据，向表格中插入5条测试数据
    for(int ii=0;ii<5;++ii){
        memset(&stgirls,0,sizeof(struct st_girls));     // 初始化结构体变量
        stgirls.id = ii+1;                              // 超女编号
        sprintf(stgirls.name,"西施%05dgirl",ii+1);      // 超女姓名
        stgirls.weight = 45.25 + ii;                    // 超女体重
        sprintf(stgirls.btime,"2022-10-09 10:33:%02d",ii+1);      // 超女报名时间
        /* 
            执行sql语句，一定要判断返回值，0-成功，其他-失败    
            失败代码在stmt.m_cda.rc中，失败描述在stmt.m_cda.message中。
        */
        if(stmt.execute() != 0){
        // 打印sql语句执行错误的信息！
            printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_sql,stmt.m_cda.message);return -1;
        }
        printf("成功插入了%ld条记录.\n",stmt.m_cda.rpc);// stmt.m_cda.rpc是本次执行SQL操作影响的记录数
        total_insert_data_number++;
    }
    printf("insert %ld table datas ok.\n",total_insert_data_number);
    conn.commit();// 提交数据库事务执行的SQL代码！（因为我这里设置了默认需要手动提交事务的代码！）
    return 0;
}
```

在C/CPP程序中，写SQL的字符串或日期时一定要注意**转义字符**的问题！

比如：

```c++
    stmt.prepare("\
    insert into girls(id,name,weight,btime) values(:1+1,:2,:3+45.35,str_to_date(:4,'%%Y-%%m-%%d %%H:%%i:%%s'))");
	or
    stmt.prepare("\
    insert into girls(id,name,weight,btime) values(?,?,?,str_to_date(?,'%%Y-%%m-%%d %%H:%%i:%%s'))");// 也ok但兼容性不如用:+参数序号的方式的兼容性好！
    printf("%s\n",stmt.m_sql);
    // 这里使用%%是为了转移义%号！
```



```c++
    /*
        插入数据codes的注意事项：
        1: prepare()准备SQL语句时，用:+参数序号的方式来表示参数，且参数的序号从1开始，连续，递增，参数也可以用问号表示，但是，问号的兼容性不好，不建议使用;
        2: SQL语句中右值才能作为参数，表名，字段名，关键字，函数名等都不能作为参数；
        3: prepare()中的参数 是可以参与运算或者用于函数参数的（比如上面的:1+1,:3+45.38）
        4: 如果SQL语句中的主体没有改变，只需要prepare()一次就可以了
        5: SQL语句中的每个参数，必须要调用bindin()绑定变量的地址
        6: 如果SQL语句的主体已改变，则prepare()之后，需要重新用bindin()绑定对应的变量
        7: prepare()方法有返回值，但一般不检查，如果你写的SQL语句有问题的话，调用execute()方法时能及时发现并返回错误！
        8: bindin()方法的返回值固定为0，不用判断返回值。
        9: prepare()和bindin()之后，每调用一次execute(),就执行一次SQL语句，SQL语句的数据来自被绑定变量的值
        10:prepare()和bindin()中参数的出现顺序必须要一致！且每来一个新的prepare(SQL语句)就必须重新对应更新各个列字段参数的bindin或bindout！
        比如：下面这个就是非常正确无误的写法！！！
        stmt.prepare("\
    	update girls set name = :1,weight = :2,btime = str_to_date(:3,'%%Y-%%m-%%d %%H:%%i:%%s'),WHERE id = :4");
    	stmt.bindin(1,stgirls.name,30);
    	stmt.bindin(2,&stgirls.weight);
    	stmt.bindin(3,stgirls.btime,19);
    	stmt.bindin(4,&stgirls.id);
    */ 
```



#### 1.1.3：==修改表格数据==的demo C/C++代码

```c++
/*
 *  程序名：updatetable.cpp，此程序演示开发框架操作MySQL数据库（更新表格中的信息）。
 *  作者：林卓凡。
*/
// 例子1：
#include "_mysql.h"
int main(int argc,char * argv[]){
    const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象

    struct st_girls{
        long   id;          // 超女编号
        // C语言中的每个字符串都有个多余的'\0',so在原字符串长度的基础上应该+1来定义该字符串（字符数组才ok）
        char   name[31];    // 超女姓名
        double weight;      // 超女体重
        char   btime[20];   // 超女报名时间
    }stgirls;
    // 准备 修改表格中数据的SQL语句！
    stmt.prepare("update girls set name='冰冰',weight=88.22,btime=now() where id in(2,3)");
    printf("%s\n",stmt.m_sql);
	// 绑定输入变量的地址
    stmt.bindin(1,stgirls.name,30);
    stmt.bindin(2,&stgirls.weight);
    stmt.bindin(3,stgirls.btime,19);
    stmt.bindin(4,&stgirls.id);
    if(stmt.execute() != 0){
    // 打印sql语句执行错误的信息！
        printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_sql,stmt.m_cda.message);return -1;
    }
    conn.commit();// 提交数据库事务执行的SQL代码！（因为我这里设置了默认需要手动提交事务的代码！）
    return 0;
}

// 例子2：
#include "_mysql.h"
int main(int argc,char * argv[]){
    const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象

    struct st_girls{
        long   id;          // 超女编号
        // C语言中的每个字符串都有个多余的'\0',so在原字符串长度的基础上应该+1来定义该字符串（字符数组才ok）
        char   name[31];    // 超女姓名
        double weight;      // 超女体重
        char   btime[20];   // 超女报名时间
    }stgirls;
    // 准备 修改表格中数据的SQL语句！
    stmt.prepare("\
    update girls set name=:1,weight=:2,btime=str_to_date(:3,'%%Y-%%m-%%d %%H:%%i:%%s') where id=:4");
    printf("%s\n",stmt.m_sql);
   	// 绑定输入变量的地址
    stmt.bindin(1,stgirls.name,30);
    stmt.bindin(2,&stgirls.weight);
    stmt.bindin(3,stgirls.btime,19);
    stmt.bindin(4,&stgirls.id);
    int total_update_data_number = 0;
    // 修改表格中的5条测试数据
    for(int ii=0;ii<5;++ii){
        memset(&stgirls,0,sizeof(struct st_girls));     // 初始化结构体变量
        stgirls.id = ii+1;                              // 超女编号
        sprintf(stgirls.name,"xhq%05dgirl",ii+1);      // 超女姓名
        stgirls.weight = 45.25 + ii;                    // 超女体重
        sprintf(stgirls.btime,"2022-10-09 12:33:%02d",ii+1);      // 超女报名时间
        /* 
            执行sql语句，一定要判断返回值，0-成功，其他-失败    
            失败代码在stmt.m_cda.rc中，失败描述在stmt.m_cda.message中。
        */
        if(stmt.execute() != 0){
        // 打印sql语句执行错误的信息！
            printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_sql,stmt.m_cda.message);return -1;
        }
        printf("成功update了%ld条记录.\n",stmt.m_cda.rpc);// stmt.m_cda.rpc是本次执行SQL操作影响的记录数
        total_update_data_number++;
    }
    printf("update %ld table datas ok.\n",total_update_data_number);
    conn.commit();// 提交数据库事务执行的SQL代码！（因为我这里设置了默认需要手动提交事务的代码！）
    return 0;
}
```



#### 1.1.4：==更新表格数据==的demo C/C++代码

#### 

```c++
/*
 *  程序名：updatetable.cpp，此程序演示开发框架操作MySQL数据库（更新表格中的信息）。
 *  作者：林卓凡。
*/
// 例子1：
#include "_mysql.h"
int main(int argc,char * argv[]){
    const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象

    struct st_girls{
        long   id;          // 超女编号
        // C语言中的每个字符串都有个多余的'\0',so在原字符串长度的基础上应该+1来定义该字符串（字符数组才ok）
        char   name[31];    // 超女姓名
        double weight;      // 超女体重
        char   btime[20];   // 超女报名时间
    }stgirls;
    // 准备 修改表格中数据的SQL语句！
    stmt.prepare("\
    update girls set name=:1,weight=:2,btime=str_to_date(:3,'%%Y-%%m-%%d %%H:%%i:%%s') where id=:4");
    printf("%s\n",stmt.m_sql);

    // 绑定输入变量的地址
    stmt.bindin(1,stgirls.name,30);
    stmt.bindin(2,&stgirls.weight);
    stmt.bindin(3,stgirls.btime,19);
    stmt.bindin(4,&stgirls.id);

    int total_update_data_number = 0;
    // 修改表格中的5条测试数据
    for(int ii=0;ii<5;++ii){
        memset(&stgirls,0,sizeof(struct st_girls));     // 初始化结构体变量
        stgirls.id = ii+1;                              // 超女编号
        sprintf(stgirls.name,"xhq%05dgirl",ii+1);      // 超女姓名
        stgirls.weight = 45.25 + ii;                    // 超女体重
        sprintf(stgirls.btime,"2022-10-09 12:33:%02d",ii+1);      // 超女报名时间
        /* 
            执行sql语句，一定要判断返回值，0-成功，其他-失败    
            失败代码在stmt.m_cda.rc中，失败描述在stmt.m_cda.message中。
        */
        if(stmt.execute() != 0){
        // 打印sql语句执行错误的信息！
            printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_sql,stmt.m_cda.message);return -1;
        }
        printf("成功update了%ld条记录.\n",stmt.m_cda.rpc);// stmt.m_cda.rpc是本次执行SQL操作影响的记录数
        total_update_data_number++;
    }
    printf("update %ld table datas ok.\n",total_update_data_number);
    conn.commit();// 提交数据库事务执行的SQL代码！（因为我这里设置了默认需要手动提交事务的代码！）
    return 0;
}

// 例子2：
#include "_mysql.h"
int main(int argc,char * argv[]){
    const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象

    struct st_girls{
        long   id;          // 超女编号
        // C语言中的每个字符串都有个多余的'\0',so在原字符串长度的基础上应该+1来定义该字符串（字符数组才ok）
        char   name[31];    // 超女姓名
        double weight;      // 超女体重
        char   btime[20];   // 超女报名时间
    }stgirls;
    // 准备 修改表格中数据的SQL语句！
    // stmt.prepare("\
    // update girls set name=:1,weight=:2,btime=str_to_date(:3,'%%Y-%%m-%%d %%H:%%i:%%s') where id=:4");
    // printf("%s\n",stmt.m_sql);

    stmt.prepare("update girls set name='冰冰',weight=88.22,btime=now() where id in(1,2,3)");
    printf("%s\n",stmt.m_sql);

    stmt.bindin(1,stgirls.name,30);
    stmt.bindin(2,&stgirls.weight);
    stmt.bindin(3,stgirls.btime,19);
    stmt.bindin(4,&stgirls.id);
    if(stmt.execute() != 0){
    // 打印sql语句执行错误的信息！
        printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_sql,stmt.m_cda.message);return -1;
    }
    conn.commit();// 提交数据库事务执行的SQL代码！（因为我这里设置了默认需要手动提交事务的代码！）
    return 0;
}
```



#### 1.1.5：==查询表格数据==的demo C/C++代码



```c++
 /*
    查询codes的注意事项：
    1、如果SQL语句的主体没有改变，则只需要prepare()一次就可以了。
    2、结果集中的字段，调用bindout()绑定输出变量的地址。
    3、bindout()方法的返回值固定为0，不用判断返回值。
    4、如果SQL语句的主体已经改变，prepare()之后需要重新bindout()绑定变量；
    5、调用execute()方法执行SQL语句，然后再循环调用next()方法获取结果集中的记录。
    6、每调用一次next()方法，会从结果集中获取一条记录，字段内容保存在已绑定的变量中。
  */
```

```c++
// 例子1：
/*
 *  程序名：selecttable.cpp，此程序演示开发框架操作MySQL数据库（查询表中数据）。
 *  作者：林卓凡。
*/
#include "_mysql.h"
int main(int argc,char *argv[])
{
  const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象
    struct st_girls{
        long   id;          // 超女编号
        // C语言中的每个字符串都有个多余的'\0',so在原字符串长度的基础上应该+1来定义该字符串（字符数组才ok）
        char   name[31];    // 超女姓名
        double weight;      // 超女体重
        char   btime[20];   // 超女报名时间
    }stgirls;

    // 准备修改表的SQL语句。
    stmt.prepare("select id,name,weight,date_format(btime,'%%Y-%%m-%%d %%H:%%i:%%s') from girls where id in (1,2,3,4,5)");
    // 2个%号是为了转义%字符！
    // 为SQL语句绑定输出变量的地址，bindout方法不需要判断返回值。
    stmt.bindout(1,&stgirls.id);
    stmt.bindout(2, stgirls.name,30);
    stmt.bindout(3,&stgirls.weight);
    stmt.bindout(4, stgirls.btime,19);

    // 执行SQL语句，一定要判断返回值，0-成功，其他-失败
    // 失败代码在stmt.m_cda.rc中，失败描述在stmt.m_cda.message中。
    if(stmt.execute() != 0){
        printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_cda.rc,stmt.m_cda.message);
        return -1;
    }
    // 用一个循环获取结果集中的每一条记录：
    // 本程序执行的是查询语句，执行stmt.execute()之后，将会在数据库的缓冲区中产生一个结果集
    while(true){
        // 先初始化输出结构体中的变量
        memset(&stgirls,0,sizeof(struct st_girls));
        // 从结果集中获取一条记录，一定要判断返回值，0-成功，1403-无记录，其他-失败
        // 在实际开发中，除了0和1403，其他的情况极少出现。
        if(stmt.next() != 0)break;
        // 把获取到达记录的值打印出来
 printf("id=%ld,name=%s,weight=%.02f,btime=%s\n",stgirls.id,stgirls.name,stgirls.weight,stgirls.btime);
    }
    // 请注意，stmt.m_cda.rpc变量非常重要，它保存了SQL被执行后影响的记录数
    printf("本次查询了grils表的%ld条记录。\n",stmt.m_cda.rpc);
    return 0;
}
// 例子2：
#include "_mysql.h"
int main(int argc,char *argv[])
{
  const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象
    struct st_girls{
        long   id;          // 超女编号
        // C语言中的每个字符串都有个多余的'\0',so在原字符串长度的基础上应该+1来定义该字符串（字符数组才ok）
        char   name[31];    // 超女姓名
        double weight;      // 超女体重
        char   btime[20];   // 超女报名时间
    }stgirls;
    int minid = -1,maxid = -1;// 查询条件 最大最小id值
    // 准备修改表的SQL语句。
    stmt.prepare("select id,name,weight,date_format(btime,'%%Y-%%m-%%d %%H:%%i:%%s') from girls where id between :1 and :2");
    // 2个%号是为了转义%字符！
    // 为SQL语句绑定输入变量的地址，bindin方法不需要判断返回值。
    stmt.bindin(1,&minid);
    stmt.bindin(2,&maxid);
    // 为SQL语句绑定输出变量的地址，bindout方法不需要判断返回值。
    stmt.bindout(1,&stgirls.id);
    stmt.bindout(2, stgirls.name,30);
    stmt.bindout(3,&stgirls.weight);
    stmt.bindout(4, stgirls.btime,19);
    // 注意：一定要在执行SQL语句之前就为这2个输入变量赋值了：
    // 指定待查询记录的最小id值
    minid = 1;
    // 指定待查询记录的最大id值
    maxid = 3;
    // 执行SQL语句，一定要判断返回值，0-成功，其他-失败
    // 失败代码在stmt.m_cda.rc中，失败描述在stmt.m_cda.message中。
    if(stmt.execute() != 0){
        printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_cda.rc,stmt.m_cda.message);
        return -1;
    }
    // 用一个循环获取结果集中的每一条记录：
    // 本程序执行的是查询语句，执行stmt.execute()之后，将会在数据库的缓冲区中产生一个结果集
    while(true){
        // 先初始化输出结构体中的变量
        memset(&stgirls,0,sizeof(struct st_girls));
        // 从结果集中获取一条记录，一定要判断返回值，0-成功，1403-无记录，其他-失败
        // 在实际开发中，除了0和1403，其他的情况极少出现。
        if(stmt.next() != 0)break;
        // 把获取到达记录的值打印出来
 printf("id=%ld,name=%s,weight=%.02f,btime=%s\n",stgirls.id,stgirls.name,stgirls.weight,stgirls.btime);
    }
    // 请注意，stmt.m_cda.rpc变量非常重要，它保存了SQL被执行后影响的记录数
    printf("本次查询了grils表的%ld条记录。\n",stmt.m_cda.rpc);
    return 0;
}
// 例子3：
#include "_mysql.h"
int main(int argc,char *argv[])
{
  const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象
    struct st_girls{
        long   id;          // 超女编号
        // C语言中的每个字符串都有个多余的'\0',so在原字符串长度的基础上应该+1来定义该字符串（字符数组才ok）
        char   name[31];    // 超女姓名
        double weight;      // 超女体重
        char   btime[20];   // 超女报名时间
    }stgirls;
    int minid = -1,maxid = -1;// 查询条件 最大最小id值
    // 准备修改表的SQL语句。
    stmt.prepare("select id,name,weight,date_format(btime,'%%Y-%%m-%%d %%H:%%i:%%s') from girls where id between :1 and :2");
    // 2个%号是为了转义%字符！
    // 为SQL语句绑定输入变量的地址，bindin方法不需要判断返回值。
    stmt.bindin(1,&minid);
    stmt.bindin(2,&maxid);
    // 为SQL语句绑定输出变量的地址，bindout方法不需要判断返回值。
    stmt.bindout(1,&stgirls.id);
    stmt.bindout(2, stgirls.name,30);
    stmt.bindout(3,&stgirls.weight);
    stmt.bindout(4, stgirls.btime,19);
    for(int ii = 0;ii < 3;++ii)// 循环查询，不只是查询一次！
    {
        // 注意：一定要在执行SQL语句之前就为这2个输入变量赋值了：
        // 指定待查询记录的最小id值
        minid = 1+ii;
        // 指定待查询记录的最大id值
        maxid = 3+ii;
        // 执行SQL语句，一定要判断返回值，0-成功，其他-失败
        // 失败代码在stmt.m_cda.rc中，失败描述在stmt.m_cda.message中。
        if(stmt.execute() != 0){
            printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_cda.rc,stmt.m_cda.message);
            return -1;
        }
        // 用一个循环获取结果集中的每一条记录：
        // 本程序执行的是查询语句，执行stmt.execute()之后，将会在数据库的缓冲区中产生一个结果集
        while(true){
            // 先初始化输出结构体中的变量
            memset(&stgirls,0,sizeof(struct st_girls));
            // 从结果集中获取一条记录，一定要判断返回值，0-成功，1403-无记录，其他-失败
            // 在实际开发中，除了0和1403，其他的情况极少出现。
            if(stmt.next() != 0)break;
            // 把获取到达记录的值打印出来
 printf("id=%ld,name=%s,weight=%.02f,btime=%s\n",stgirls.id,stgirls.name,stgirls.weight,stgirls.btime);
        }
        // 请注意，stmt.m_cda.rpc变量非常重要，它保存了SQL被执行后影响的记录数
        printf("本次查询了grils表的%ld条记录。\n",stmt.m_cda.rpc);
    }
    return 0;
}
```



#### 1.1.6：==删除表格数据==的demo C/C++代码

```c++
/*
 *  程序名：deletetable.cpp，此程序演示开发框架操作MySQL数据库（删除表中数据）。
 *  作者：林卓凡。
*/
#include "_mysql.h"
int main(int argc,char *argv[])
{
  const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象

    int minid = -1,maxid = -1;// 查询条件 最大最小id值
    // 准备删除表的SQL语句。
    stmt.prepare("delete from girls where id between :1 and :2");
    // 为SQL语句绑定输入变量的地址，bindin方法不需要判断返回值。
    stmt.bindin(1,&minid);
    stmt.bindin(2,&maxid);
    // 注意：一定要在执行SQL语句之前就为这2个输入变量赋值了：
    // 指定待查询记录的最小id值
    minid = 1;
    // 指定待查询记录的最大id值
    maxid = 3;
    // 执行SQL语句，一定要判断返回值，0-成功，其他-失败
    // 失败代码在stmt.m_cda.rc中，失败描述在stmt.m_cda.message中。
    if(stmt.execute() != 0){
        printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_cda.rc,stmt.m_cda.message);
        return -1;
    }
    // 请注意，stmt.m_cda.rpc变量非常重要，它保存了SQL被执行后影响的记录数
    printf("本次删除了grils表的%ld条记录。\n",stmt.m_cda.rpc);
    conn.commit();// 提交数据库的事务
    return 0;
}
```



#### 1.1.7：==二进制大文件的存取==的demo C/C++代码

**在MySQL中的BLOB二进制大对象什么都可以传**，比如：照片，视频，音频，excel文档等等。

但吴老师建议我们最好**只在数据库**中**保存对应二进制对象的名称**就行，然后**把对应的实体（照片，视频，音频...）内容存放到磁盘**上！以降低数据库存储数据的压力！！！

![image-20221010144424545](C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221010144424545.png)



```c++
/*
  程序名：filetoblob.cpp，此程序演示开发框架操作MySQL数据库（演示将BLOB二进制文件对象上传到mysql数据库）。
  作者：林卓凡。
*/
#include "_mysql.h"
#include<iostream>
#include<string>
using namespace std;
int main(int argc,char * argv[]){
    const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象

    struct st_girls{
        long   id;              // 超女编号
        char   pic[100000];     // 超女图片的内容，这里定义为10w个字节，大概不到100KB
        // 当然啦，你采用动态分配内存的方式也是ok的！！！
        unsigned long picsize;  // 图片内容占用的字节数(即：图片的大小 )
    }stgirls;
    // 准备 修改表格中数据的SQL语句！
    stmt.prepare("update girls set pic=:1 where id=:2");
    printf("%s\n",stmt.m_sql);
    // 绑定输入变量
    stmt.bindinlob(1,stgirls.pic,&stgirls.picsize);
    stmt.bindin(2,&stgirls.id);
    // 只修改超女表中id为1和2的记录
    for(int i = 1;i <= 2;++i){
        memset(&stgirls,0,sizeof(struct st_girls));// 初始化结构体变量
        // 为结构体变量的成员赋值。
        stgirls.id = i;// 超女编号
        // 把图片内容加载到stgirls.pic中。
        string t = to_string(i) + ".jpg";
        // filetobuf()是_mysql.h开发框架中的把filename对应的文件加载到buffer中的函数！(very important!)
        stgirls.picsize = filetobuf(t.c_str(),stgirls.pic);
        // if (ii==1) stgirls.picsize=filetobuf("1.jpg",stgirls.pic);
        // if (ii==2) stgirls.picsize=filetobuf("2.jpg",stgirls.pic);
        
        // 执行SQL语句
        if(stmt.execute() != 0){
            // 打印sql语句执行错误的信息！
            printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_sql,stmt.m_cda.message);return -1;
        }
        printf("成功修改了%ld条记录。\n",stmt.m_cda.rpc);
        // stmt.m_cda.rpc是本次执行SQL影响的记录数
    }
    printf("update table girls ok.\n");
    conn.commit();// 提交数据库事务执行的SQL代码！（因为我这里设置了默认需要手动提交事务的代码！）
    return 0;
}
```

```c++
/*
  程序名：blobtofile.cpp，此程序演示开发框架操作MySQL数据库（演示将BLOB二进制文件对象从mysql数据库中取出来放到指定的文件中去）。
  作者：林卓凡。
*/

#include "_mysql.h"
int main(int argc,char * argv[]){
    const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象

    struct st_girls{
        long   id;              // 超女编号
        char   pic[100000];     // 超女图片的内容，这里定义为10w个字节，大概不到100KB
        // 当然啦，你采用动态分配内存的方式也是ok的！！！
        unsigned long picsize;  // 图片内容占用的字节数(即：图片的大小 )
    }stgirls;
    // 准备 修改表格中数据的SQL语句！
    stmt.prepare("select id,pic from girls where id in (1,2)");
    printf("%s\n",stmt.m_sql);
    // 绑定输出变量
    stmt.bindout(1,&stgirls.id);
    stmt.bindoutlob(2,stgirls.pic,100000,&stgirls.picsize);
    // 执行SQL语句
    if(stmt.execute() != 0){
        // 打印sql语句执行错误的信息！
        printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_sql,stmt.m_cda.message);return -1;
    }
    // 本程序执行的是查询语句，执行stmt.execute()后，将会在数据库的缓冲区中产生一个结果集
    while(true){
        // 先初始化输出结构体中的变量
        memset(&stgirls,0,sizeof(struct st_girls));
        // 从结果集中获取一条记录，一定要判断返回值，0-成功，1403-无记录，其他-失败
        // 在实际开发中，除了0和1403，其他的情况极少出现。
        if(stmt.next() != 0)break;

        // 生成文件名
        char filename[101];memset(filename,0,sizeof(filename));
        sprintf(filename,"%ld_output.jpg",stgirls.id);
        // 把内容写入文件
        // buftofile()函数是_mysql.h开发框架中将buffer中的内容写入文件filename的函数！(very important!)
        buftofile(filename,stgirls.pic,stgirls.picsize);
    }
    // 请注意，stmt.m_cda.rpc变量非常重要，它保存了SQL被执行后影响的记录数
    printf("本次查询了girls表的%ld条记录\n",stmt.m_cda.rpc);
    return 0;
}
```



**注意**：

**1-二进制大对象占用的磁盘空间非常大，这会对数据库造成压力！**

**2-不要把大量的二进制大对象存入数据库**

**3-把==二进制大对象==存放在==磁盘文件==中，把==文件名==存放在==数据库表==中，这样子就会减少数据库的压力了！**



### 1.2：MySQL数据库开发的注意事项（==技巧,可用于在面试时谈及这个问题，然后吹自己是怎么deal的==）

**1-**一个connection对象在同一时间内只能连接一个数据库（断开后也可重连，具体可研究下connection类的源码）

**2-**同一个程序中，创建多个connection对象 就可以同时连多个数据库

**3-**每个connection对象的事务的独立的

**4-**多个进程==不能共享同一个==已连接成功的connection对象。（你自己想想嘛，多个进程同时操作这1个已连接成功的conn对象的话，一个进程要求commit事务，而另一个进程又要rollback事务，那我听谁的？对吧？）

**5-**多个sqlstatement对象，可以绑定同一个connection对象（也就是说，对于同一个mysql客户端连接，我可以操作多条SQL语句）

==**6-**如果执行了select语句，在结果集没有获取完之前，同一个connection对象中的全部sqlstatement对象都不能执行任何的SQL语句。==

**==7-C语言不能表示空的整数和浮点数，实战中可以用字符串存放整数和浮点数，以表示空值。==**

（这个是**我们C/CPP程序员日常的项目开发中处理MySQL数据库时非常常见的问题，吴老师讲解了自己的deal经验**，如下代码所示）

**问题**：现在我们要在grils这个表格中插入weight有可能是NULL空值的data

**deal方法**：超女体重本来是double weight;但是为了插入控制NULL，改为了字符串 => char   weight[10]; 



```c++
/*
 *  程序名：inserttable2.cpp，此程序演示开发框架操作MySQL数据库（插入信息到表中,带空值！）。
 *  作者：林卓凡。
*/
#include "_mysql.h"
int main(int argc,char * argv[]){
    const char *connstr = "106.13.216.221,linzhuofan,123456,demo_bd,3306";
    const char *charset = "utf8";
    connection conn;// 定义数据库连接类的对象
    // 登录数据库，返回值：0-成功；其他是失败，存放了MySQL的错误代码！
    // 失败代码在conn.m_cda.rc中，失败描述在conn.m_cda.message中。
    if(conn.connecttodb(connstr,charset) != 0){
        printf("connect databse failed.\n%s\n",conn.m_cda.message);
        return -1;
        // if为正式的服务程序的话，我们会 把错误信息打印到日志中（）框架中的CLogFile类！方便维护该服务程序！
        // 但因为这个是demo 程序，就直接打印错误信息了！
    }
    // 定义数据库操作类对象，并绑定对应已经连接好的数据库连接对象conn
    sqlstatement stmt(&conn);// 操作SQL语句的对象
    struct st_girls{
        long   id;          // 超女编号
        // C语言中的每个字符串都有个多余的'\0',so在原字符串长度的基础上应该+1来定义该字符串（字符数组才ok）
        char   name[31];    // 超女姓名
        char   weight[10];  // 超女体重(本来是double weight;)但是为了插入控制NULL，改为了字符串！！！
        char   btime[20];   // 超女报名时间
    }stgirls;
    // 准备 插入表格中的SQL语句！
        stmt.prepare("\
    insert into girls(id,name,weight,btime) values(?,?,?,str_to_date(?,'%%Y-%%m-%%d %%H:%%i:%%s'))");// 也ok但兼容性不如用:+参数序号的方式的兼容性好！
    printf("%s\n",stmt.m_sql);
    // 绑定输入变量的地址
    stmt.bindin(1,&stgirls.id);
    stmt.bindin(2,stgirls.name,30);
    stmt.bindin(3,stgirls.weight,10);
    stmt.bindin(4,stgirls.btime,19);

    int total_insert_data_number = 0;
    // 模拟超女数据，向表格中插入5条测试数据
    for(int ii=5;ii<10;++ii){
        memset(&stgirls,0,sizeof(struct st_girls));     // 初始化结构体变量
        // 这会让这整个结构体变量的all成员变量都初始化为null！
        stgirls.id = ii+1;                              // 超女编号
        sprintf(stgirls.name,"西施%05dgirl",ii+1);      // 超女姓名
        // stgirls.weight = 45.25 + ii;                    // 超女体重
        if(ii < 8)sprintf(stgirls.weight,"%.2f",45.25 +ii);
        // 当ii >= 8时，我们这里插入的weight就是上面memset()初始化时给weight字符数组初始化的null值了！       
        sprintf(stgirls.btime,"2022-10-10 10:33:%02d",ii+1);      // 超女报名时间
        /* 
            执行sql语句，一定要判断返回值，0-成功，其他-失败    
            失败代码在stmt.m_cda.rc中，失败描述在stmt.m_cda.message中。
        */
        if(stmt.execute() != 0){
        // 打印sql语句执行错误的信息！
            printf("stmt.execute() failed.\n%s\n%s\n",stmt.m_sql,stmt.m_cda.message);return -1;
        }
        printf("成功插入了%ld条记录.\n",stmt.m_cda.rpc);// stmt.m_cda.rpc是本次执行SQL操作影响的记录数
        total_insert_data_number++;
    }
    printf("insert %ld table datas ok.\n",total_insert_data_number);
    conn.commit();// 提交数据库事务执行的SQL代码！（因为我这里设置了默认需要手动提交事务的代码！）
    return 0;
}
```



## 2：学习数据库设计工具

平时的学习还是可以只使用用==Navicat==工具即可，但是在工作后，日常项目开发所涉及的表格非常多，这时就需要我们生成数据库设计文档和SQL语句了！此时再使用Navicat就不太合适了，因此，下面就介绍并学习PowerDesigner软件的使用！使用该软件可自动生成数据库的设计文档和SQL语句！



### 2.1：学习PowerDesigner软件的使用方法

==**(只需要看吴老师的第五章的第5.9p这一个视频就知道如何使用这个软件来开发数据库和生成文档了！)**==

[PowerDesigner对应的破解下载地址和教程](https://zhuanlan.zhihu.com/p/179260147)

我已经保存到我自己的百度网盘上面去了！

对于**自增字段**，一般设计为**主键**，但这句话并**不是完全对的**！（**还得看对应的应用场景**）

对于一个**自增字段**，我也可以设计为**普通的键值**！

```mysql
drop index idx_girls_3 on T_GIRLS;

drop index idx_girls_2 on T_GIRLS;

drop index idx_girls_1 on T_GIRLS;

drop table if exists T_GIRLS;

/*==============================================================*/
/* Table: T_GIRLS                                               */
/*==============================================================*/
create table T_GIRLS
(
   id                   bigint not null comment '超女编号。',
   name                 varchar(30) not null comment '超女姓名，中文名。',
   weight               numeric(5,2) comment '超女体重，单位：kg',
   btime                datetime default CURRENT_TIMESTAMP comment '报名时间。',
   pic                  longblob comment '照片',
   rsts                 numeric(1) default 1 comment '记录状态，1-启用；2-禁用，default：1',
   keyid                int auto_increment comment '记录编号，自增字段',
   primary key (id),
   key AK_GIRLS_KEYID (keyid) # 对于一个自增字段，我也可以设计为普通的键值！
);

alter table T_GIRLS comment '本表存放了很多漂亮妹妹的基本信息。';

/*==============================================================*/
/* Index: idx_girls_1                                           */
/*==============================================================*/
create index idx_girls_1 on T_GIRLS
(
   name
);

/*==============================================================*/
/* Index: idx_girls_2                                           */
/*==============================================================*/
create index idx_girls_2 on T_GIRLS
(
   rsts
);

/*==============================================================*/
/* Index: idx_girls_3                                           */
/*==============================================================*/
create unique index idx_girls_3 on T_GIRLS
(
   keyid
);
```



这是把生成的数据库文档按照一定的比例（这是吴老师调好的比例）输出乘RTF格式（即word格式）的文档！

![image-20221011132244036](C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221011132244036.png)



然后再在word里面把一些中文说明文字改一改，比如主要键改为主键，外来键改为外键，TRUE改为Y，FALSE改为N，这直接用word里面的全部替换功能就ok的了！

最后做出来的word文档是这么靓仔的：

![image-20221011132724686](C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221011132724686.png)



**注意**：

一个项目的（设计）文档一定是要清晰可见的。

我们平时用到PowerDesigner的功能很少，不超过10%，也就设计表格，增加字段列，生成sql语句，生成word文档（即：RTF格式的文档）。这一个软件就足够了！！！

### 2.2：生成数据库设计文档和SQL语句







## 3：把测试数据文件入库(非通用代码模块)

**注意：**视频中的数据库版本是MySQL 5.7.34，字符集是utf8，数据库版本可以不同，但是字符集一定要相同，否则很容易出现乱码的问题！！！

### 3.1：站点参数入库

先设计一个**全国站点参数**table，对应的sql语句如下所示：

```c++
/*
    obtcodetodb.cpp，本程序（非通用功能模块，so放到idcMyCodes目录下）
    用于把全国站点参数数据保存到数据库T_ZHOBTCODE表格中。
    作者：林卓凡
*/
#include"_public.h"
#include"_mysql.h"
CLogFile logfile; // 全局日志文件，这样all的函数都可使用该日志文件
CPActive PActive; // 进程心跳类，帮助我外部的checkProcess进程检查本进程的心跳的！
connection conn;  // 创建 连接数据库的类对象
struct st_stcode{
    char provname[31];  // 省
    char obtid[11];     // 站点号
    char cityname[31];   // 站点名
    char lat[11];         // 纬度
    char lon[11];         // 经度
    char height[11];      // 海拔高度
};
vector<struct st_stcode> vstcode;// 存放全国气象站点参数的容器。
// 程序退出和信号2、15的处理函数
void EXIT(int sig);
// 把站点参数文件中加载到vstcode容器中。
bool LoadSTCode(const char *inifile);

int main(int argc,char* argv[])
{
    // 帮助文档。
    if(argc != 5)
    {
        printf("\n"); 
        printf("Using:./obtcodetodb inifile connstr charset logfile\n");
        printf("Example:/home/linzhuofan/project/tools1/c/procct1 120 /home/linzhuofan/project/idcMyCodes/bin/obtcodetodb \
        /home/linzhuofan/project/idcMyCodes/ini/stcode.ini \"106.13.216.221,linzhuofan,123456,demo_bd,3306\" utf8 /tmp/log/idc/obtcodetodb.log\n\n");

        // /home/linzhuofan/project/tools1/c/procct1 120 /home/linzhuofan/project/idcMyCodes/bin/obtcodetodb /home/linzhuofan/project/idcMyCodes/ini/stcode.ini "106.13.216.221,linzhuofan,123456,demo_bd,3306" utf8 /tmp/log/idc/obtcodetodb.log
        printf("本程序用于把全国站点参数数据保存到数据库表中，如果站点不存在则插入，站点已存在则更新。\n");
        printf("inifile 站点参数文件名（全路径）。\n");   
        printf("connstr 数据库连接参数：ip,username,password,dbname,port\n");   
        printf("charset 数据库的字符集。\n");   
        printf("logfile 本程序运行的日志文件名。\n");   
        printf("程序每间隔120秒会运行一次，由于procct1调度程序来调度运行。\n");
        return -1;
    }
    // 处理程序退出的信号。
    /*
        关闭全部的信号和输入输出
        设置信号，在shell状态下可用"kill + 进程号"正常终止该进程
        但请不要用 "kill -9 + 进程号"来强制终止该进程！
        在测试开发阶段时，该代码不需启用，等正式用于生产环境时，就需要把该语句启用！
    */ 
    CloseIOAndSignal();signal(SIGINT,EXIT);signal(SIGTERM,EXIT);
    // 打开日志文件。
    if( logfile.Open(argv[4],"a+")==false )
    { printf("打开日志文件失败（%s）。\n",argv[4]); return -1; }

    logfile.Write("obtcodetodb 开始运行。\n");
    PActive.AddPInfo(10,"obtcodetodb");   // 进程的心跳，因为该进程插入气象站点数据到表中是非常快的，一般1s之内都能够do完，so给10秒的心跳足够了。
    // 注意，在调试程序的时候，可以启用类似以下的代码，防止超时。
    // PActive.AddPInfo(5000,"obtcodetodb");

    // 把全国站点参数文件加载到vstcode容器中。  
    if (LoadSTCode(argv[1])==false) return -1;
    // 连接数据库。(要是加载参数文件内容失败那后面就无需继续do链接数据库还有插入更新数据等的操作了！)
    if ( conn.connecttodb(argv[2],argv[3])!=0 )
    {logfile.Write("connect database(%s) failed.\n%s\n",argv[2],conn.m_cda.message); return -1;}
    
    logfile.Write("connect database(%s) ok.\n",argv[2]);

    struct st_stcode stcode;
    // 准备插入表的SQL语句。
    sqlstatement stmtins(&conn);// 创建 执行SQL语句的类对象
    stmtins.prepare("insert into T_ZHOBTCODE(obtid,cityname,provname,lat,lon,height,upttime) values(:1,:2,:3,:4*100,:5*100,:6*10,now())");

    // 绑定输入变量的地址
    stmtins.bindin(1,stcode.obtid,10);
    stmtins.bindin(2,stcode.cityname,30);
    stmtins.bindin(3,stcode.provname,30);
    stmtins.bindin(4,stcode.lat,10);
    stmtins.bindin(5,stcode.lon,10);
    stmtins.bindin(6,stcode.height,10);
    // 准备更新表的SQL语句。
    sqlstatement stmtupt(&conn);// 创建 执行SQL语句的类对象
    stmtupt.prepare("update T_ZHOBTCODE set cityname=:1,provname=:2,lat=:3*100,lon=:4*100,height=:5*10,upttime=now() where obtid=:6");
    // 绑定输出变量的地址
    stmtupt.bindout(1,stcode.cityname,30);
    stmtupt.bindout(2,stcode.provname,30);
    stmtupt.bindout(3,stcode.lat,10);
    stmtupt.bindout(4,stcode.lon,10);
    stmtupt.bindout(5,stcode.height,10);
    stmtupt.bindout(6,stcode.obtid,10);
    
    int ins_count=0,upt_count=0;
    CTimer Timer;// 创建时间类变量，do计时的工作！

    // 遍历vstcode容器。
    for(int ii = 0;ii < vstcode.size();++ii)
    {   
      // 从容器中取出一条记录到结构体stcode中。
      memcpy(&stcode,&vstcode[ii],sizeof(struct st_stcode));
      // 执行插入到SQL语句。
      if(stmtins.execute()!=0)
      {
          // 如果记录已经存在，执行更新SQL的语句。
          if(stmtins.m_cda.rc == 1062)
          {
            // MYSQL中的1062错误代码：说明要插入到记录已存在，则此时切换为更新记录
            if(stmtupt.execute()!=0)
            { // 更新data失败就写日志！
              logfile.Write("stmtupt.execute() failed.\n%s\n%s\n",stmtupt.m_sql,stmtupt.m_cda.message);return -1;
            }
            else{
              // 更新成功，更新记录数+1
              upt_count++;
            }
          }
          else{
            // 插入data失败就写日志！
            logfile.Write("stmtins.execute() failed.\n%s\n%s\n",stmtins.m_sql,stmtins.m_cda.message);return -1;
          }
      }
      else{
          // 插入成功，插入记录数+1
          ins_count++;
      }
    }
    // 把操作完数据库表中的总记录数，插入记录数，更新记录数，消耗时长记录日志。
    logfile.Write("总记录数=%ld，插入记录%d条，更新记录%d条，耗时=%.2f秒。\n",vstcode.size(),ins_count,upt_count, Timer.Elapsed());

    // 提交事务
    conn.commit();
    return 0;
}

// 程序退出和信号2、15的处理函数
void EXIT(int sig){
    logfile.Write("程序退出，sig=%d\n\n",sig);
    conn.disconnect();// 手动断开与数据库的连接，当然这行代码你不写也是ok的，
    // 因为connection的析构函数中会自动帮你调用该函数！
    exit(0);
}


// 把站点参数文件中加载到vstcode容器中。
bool LoadSTCode(const char *inifile)
{
  CFile File;

  // 打开站点参数文件。
  if (File.Open(inifile,"r")==false)
  {
    logfile.Write("File.Open(%s) failed.\n",inifile); return false;
  }

  char strBuffer[301];// 多的这一个字节是存放C语言的字符串中的'\0'结束字符的！

  CCmdStr CmdStr;

  struct st_stcode stcode;

  while (true)
  {
    // 从站点参数文件中读取一行，如果已读取完，跳出循环。
    if (File.Fgets(strBuffer,300,true)==false) break;

    // 把读取到的一行拆分。
    CmdStr.SplitToCmd(strBuffer,",",true);

    if (CmdStr.CmdCount()!=6) continue;     // 扔掉无效的行。

    // 把站点参数的每个数据项保存到站点参数结构体中。
    memset(&stcode,0,sizeof(struct st_stcode));
    CmdStr.GetValue(0, stcode.provname,30); // 省
    CmdStr.GetValue(1, stcode.obtid,10);    // 站号
    CmdStr.GetValue(2, stcode.cityname,30);  // 站名
    CmdStr.GetValue(3, stcode.lat,10);      // 纬度
    CmdStr.GetValue(4, stcode.lon,10);      // 经度
    CmdStr.GetValue(5, stcode.height,10);   // 海拔高度
    // 把站点参数结构体放入站点参数容器。
    vstcode.push_back(stcode);
  }
  return true;
}
```



### 3.2：观测数据入库（比站点数据入库要难，这个观测数据入库模块的编写和实战项目的难度很接近了！）

==**注意**==：

对于观测到的数据，因为这些数据都是从传感器硬件机器那里获取到的，读取到多少就是多少，你没法改变！就算硬件读取错了顶多该数据不可用，但你没法改变这些数据，因为你改了也没用！So**只需要对该观测数据do insert的SQL操作**即可了！

```bash
obtmindtodb.cpp ，其中用到的业务中的通用的结构体和类的声明 及定义在idcapp.h和idcapp.cpp中！！！
```





补充任务：**开发一个执行SQL脚本文件的工具程序！（通用的模块）**

在Linux下，命令行输入**如下指令**可执行一个**SQL脚本**

```bash
mysql -ulinzhuofan -p123456 -Ddemo_bd < /home/linzhuofan/project/idcMyCodes/sql/cleardata.sql
```

但是，我们的**调度程序procct1**并**不支持<这种重定向**的操作！！！

因此，吴老师直接教我们可以直接开发一个执行SQL脚本文件（即：以.sql结尾的文件）的工具！

```c++
/*
    execsql.cpp，本程序是:执行SQL代码的通用工具(可用于自动化批量执行删除SQL的代码)
    作者：林卓凡
*/

#include"_public.h"
#include"_mysql.h"

CLogFile logfile; // 全局日志文件，这样all的函数都可使用该日志文件
CPActive PActive; // 进程心跳类，帮助我外部的checkProcess进程检查本进程的心跳的！
connection conn;  // 创建 连接数据库的类对象


// 程序退出和信号2、15的处理函数
void EXIT(int sig);

int main(int argc,char* argv[])
{
    // 帮助文档。
    if(argc != 5)
    {
        printf("\n"); 
        printf("Using:./execsql sqlfile connstr charset logfile\n");
        printf("Example:/home/linzhuofan/project/tools1/c/procct1 120 /home/linzhuofan/project/tools1/bin/execsql \
        /home/linzhuofan/project/idcMyCodes/sql/cleardata.sql \"106.13.216.221,linzhuofan,123456,demo_bd,3306\" utf8 /tmp/log/idc/execsql.log\n\n");

        // /home/linzhuofan/project/tools1/c/procct1 120 /home/linzhuofan/project/tools1/bin/execsql /home/linzhuofan/project/idcMyCodes/sql/cleardata.sql "106.13.216.221,linzhuofan,123456,demo_bd,3306" utf8 /tmp/log/idc/execsql.log
        printf("本程序用于把全国站点参数数据保存到数据库表中，如果站点不存在则插入，站点已存在则更新。\n");
        printf("sqlfile 是sql脚本文件名，每条sql语句可以多行书写，分号;表示一条sql语句的结束，不支持注释。\n");   
        printf("connstr 数据库连接参数：ip,username,password,dbname,port\n");   
        printf("charset 数据库的字符集。\n");   
        printf("logfile 本程序运行的日志文件名。\n");   
        printf("程序每间隔120秒会运行一次，由于procct1调度程序来调度运行。\n");

        return -1;
    }
    // 处理程序退出的信号。
    /*
        关闭全部的信号和输入输出
        设置信号，在shell状态下可用"kill + 进程号"正常终止该进程
        但请不要用 "kill -9 + 进程号"来强制终止该进程！
        在测试开发阶段时，该代码不需启用，等正式用于生产环境时，就需要把该语句启用！
    */ 
    CloseIOAndSignal();signal(SIGINT,EXIT);signal(SIGTERM,EXIT);
    // 打开日志文件。
    if( logfile.Open(argv[4],"a+")==false )
    { printf("打开日志文件失败（%s）。\n",argv[4]); return -1; }

    logfile.Write("obtcodetodb 开始运行。\n");
    PActive.AddPInfo(10,"obtcodetodb");   // 进程的心跳，因为该进程插入气象站点数据到表中是非常快的，一般1s之内都能够do完，so给10秒的心跳足够了。
    // 注意，在调试程序的时候，可以启用类似以下的代码，防止超时。
    // PActive.AddPInfo(5000,"obtcodetodb");

    // 连接数据库。(要是加载参数文件内容失败那后面就无需继续do链接数据库还有插入更新数据等的操作了！)
    if ( conn.connecttodb(argv[2],argv[3])!=0 )
    {logfile.Write("connect database(%s) failed.\n%s\n",argv[2],conn.m_cda.message); return -1;}
    
    logfile.Write("connect database(%s) ok.\n",argv[2]);

    // 这个程序的流程非常简单，就是打开*.sql文件一行一行读取并执行sql语句即可！
    CFile File;
    // 打开SQL文件。
    if( File.Open(argv[1],"r")==false)
    {logfile.Write("File.Open(%s) failed.\n",argv[1]); EXIT(-1); }
    logfile.Write("File.Open(%s) ok.\n",argv[1]);

    char strsql[1001];  // 存放从SQL文件中读取到SQL语句！（可视具体SQL代码多少来定这个程序！）
    while(true){

        memset(strsql,0,sizeof(strsql));
        // 从SQL文件中读取以分号";"结束的行
        if(File.FFGETS(strsql,1000,";")==false) break;// 一旦读取不到sql语句就跳出循环不执行该SQL脚本了！
        
        // 删除掉SQL语句每一行最后的分号;。
        char* pp = strstr(strsql,";");
        if(pp==nullptr)continue;// 
        pp[0] = 0;
        logfile.Write("当前准备要执行的SQL语句是：[%s]\n",strsql);
        int ret = conn.execute(strsql);

        // 把SQL语句执行结果写日志
        if(ret==0)logfile.Write("exec ok(rpc=%d).\n",conn.m_cda.rpc);
        else logfile.Write("exec failed(%s).\n",conn.m_cda.message);

        PActive.UptATime();// 进程心跳。
    }
    // 提交事务
    conn.commit();
    logfile.Write("\n");    // 加个空行，方便查看日志！
    return 0;
}

// 程序退出和信号2、15的处理函数
void EXIT(int sig){
    logfile.Write("程序退出，sig=%d\n\n",sig);
    conn.disconnect();// 手动断开与数据库的连接，当然这行代码你不写也是ok的，
    // 因为connection的析构函数中会自动帮你调用该函数！
    exit(0);
}
```

```makefile
# 这是为守护进程专门写好的makefile文件方便编译该.cpp文件

# 开发框架头文件路径
PUBINCL = -I/home/linzhuofan/project/public
# 开发框架cpp文件名字，这里直接包含进来，而没有采用链接库的形式，是为了方便调试
PUBCPP = /home/linzhuofan/project/public/_public.cpp
# 编译参数
CFLAGS = -g # 使得该程序编译时 出现error or warning的信息！
# ftp开发框架cpp文件名字，这里直接包含进来，而没有采用链接库的形式，是为了方便调试
FTPCPP = /home/linzhuofan/project/public/_ftp.cpp
# 指定你do的动态or静态库的路径。
LINKPATH = -L/home/linzhuofan/project/public/

# 这是我自己的服务器上的mysql头文件存放的目录。
# -I指定的是当前源文件.c/.cpp中include到达头文件所在的目录
MYSQLINCL = -I/usr/include/mysql

# 这是我自己的服务器上的mysql动态库文件存放的目录。
MYSQLLIB = -L/usr/lib64/mysql

# 需要链接的mysql库。-l加上去lib和去.so/.a后的动态库or静态库的文件名
MYSQLLIBS = -lmysqlclient

# 开发框架_mysql.h的_mysql.cpp文件名,这里直接包含进来，没有采用链接库，是为了方便调试。
MYSQLCPP = /usr/include/mysql/_mysql.cpp

# cp obtmindtodb /home/linzhuofan/project/idcMyCodes/bin/
CFLAGS2=-g -Wno-write-strings


all:procct1 serverProgram checkProcess book gzipfiles deletefiles ftpgetfiles ftpputfiles execsql

procct1:procct1.cpp
	g++ $(CFLAGS) -o procct1 procct1.cpp $(PUBINCL) $(PUBCPP) -lm -lc
	cp procct1 ../bin/. # 把生成的可执行程序放到上一级的bin目录下
serverProgram:serverProgram.cpp
	g++ $(CFLAGS) -o serverProgram serverProgram.cpp $(PUBINCL) $(PUBCPP) -lm -lc
	cp serverProgram ../bin/. # 把生成的可执行程序放到上一级的bin目录下
checkProcess:checkProcess.cpp
	g++ $(CFLAGS) -o checkProcess checkProcess.cpp $(PUBINCL) $(PUBCPP) -lm -lc
	cp checkProcess ../bin/. # 把生成的可执行程序放到上一级的bin目录下
book:book.cpp
	g++ $(CFLAGS) -o book book.cpp $(PUBINCL) $(PUBCPP) -lm -lc
	cp book ../bin/. # 把生成的可执行程序放到上一级的bin目录下
gzipfiles:gzipfiles.cpp
	g++ $(CFLAGS) -o gzipfiles gzipfiles.cpp $(PUBINCL) $(PUBCPP) -lm -lc
	cp gzipfiles ../bin/. # 把生成的可执行程序放到上一级的bin目录下
deletefiles:deletefiles.cpp
	g++ $(CFLAGS) -o deletefiles deletefiles.cpp $(PUBINCL) $(PUBCPP) -lm -lc
	cp deletefiles ../bin/. # 把生成的可执行程序放到上一级的bin目录下

ftpgetfiles:ftpgetfiles.cpp
	g++ $(CFLAGS) -o ftpgetfiles ftpgetfiles.cpp $(PUBINCL) $(PUBCPP) $(FTPCPP) $(LINKPATH) -lftp -lm -lc
	cp ftpgetfiles ../bin/. # 把生成的可执行程序放到上一级的bin目录下

ftpputfiles:ftpputfiles.cpp
	g++ $(CFLAGS) -o ftpputfiles ftpputfiles.cpp $(PUBINCL) $(PUBCPP) $(FTPCPP) $(LINKPATH) -lftp -lm -lc
	cp ftpputfiles ../bin/. # 把生成的可执行程序放到上一级的bin目录下

execsql:execsql.cpp
	g++ $(CFLAGS) -o execsql execsql.cpp $(PUBINCL) $(PUBCPP) $(MYSQLINCL) $(MYSQLLIB) $(MYSQLLIBS) $(MYSQLCPP) -lm -lc
	cp execsql ../bin/. # 把生成的可执行程序放到上一级的bin目录下

clean:
	rm -f procct1 serverProgram checkProcess book gzipfiles deletefiles ftpgetfiles ftpputfiles execsql

```





## 第5章节小总结：



1：**数据库**是每个程序员应该掌握的基本常识，不算是什么技术了。

2：**C语言操作mysql虽然有对应的C库函数**，但是**非常难用（太TMD原始了）**，不好！这里直接使用框架中封装好的，弄成C++形式的框架中的函数来doSQL连接和各种操作即可！（**有了我们的connection和sqlstatement类，那么对于mysql的操作其实就变为了体力活儿而已了！这让我们可以把更多的精力放在业务的开发上！**比如将全国站点观测数据入库等等。。。）因为我们写代码的目的就是以更好的方式更省时的方式来完成项目，完成了才能够赚钱！

3：**PowerDesigner工具，太TMD方便了，不仅能够图形化设计MySQL中的各种表格，还能够自动导出你想要的各种word或者XML形式的表格文档，一举两得，节省开发时间！！！**

（虽然该工具非常强大，但是就连大佬平时都只是拿来写表格，视图，导出word文档等等内容，并没有把该工具玩儿花，而且也不需要玩得很熟悉！P5.9第五章这一个小视频里面的内容足够应付平时的工作了，大佬平时做开发的话也就是用这个工具的这些内容而已！）

4：**在业务开发中，遇到操作SQL的地方，我们会==十分注重业务代码的逻辑和细节==！通常我们都会把操作该业务主要的内容都==封装为一个类==，这样你的代码逻辑就非常清晰，也十分容易维护，更让别人容易读懂你的代码，不像是一些屎山代码！！！**

（比如：**操作表格T_ZHOBTMIND的all的业务代码的各种逻辑和细节**，**都专门封装为一个类**，这样就好方便维护和操作了！！！）

5：**暂时，我们都不需要也没有那个能力去细究connection和sqlstatement这2个类的封装细节**，这设计到mysql底层的一些API还有一些原理性的内容，我们做完这个大项目有时间再看就ok了，没有时间不看也ok，你**只要熟练掌握这2个C/C++操作MySQL的类进行业务的开发那也是非常ok的了！**







# 第四章：开发基于TCP协议的文件传输系统



### 一、基础知识

掌握计算机网络基础知识（基本常识）。

掌握socket的常用函数，能编写最简单的网络通信程序。



### 二、封装socket的API

socket的常用函数非常多，直接用来写网络通信的代码会非常麻烦（有很多细节需要处理，需要细致地扣）。因此，这个章节我们直接把socket 常用的API都封装好，都封装在一个框架中！（类和函数）

这样做会有以下**两个好处**：

1：解决**TCP报文粘包/分包**的问题。

2：封装socket的常用函数，后续使用coding起来就会非常方便！

#### 2.1 TCP报文粘包/分包的问题。

粘包和分包的原理（我们百度一下，几分钟就能够理解了）

**粘包：**比如，发送方分别发送了两个字符串报文“hello”和“world”，但接收方却一次性收到了“helloworld”。（也就是这两个报文粘到一起了）

**分包：**比如，发送方发送了一个字符串报文“helloworld”，但接收方却接收到了两个字符串报文“hello”和“world”。



TCP协议的保证：

1：报文内容的顺序不变，如果发送方发送“hello”，接收方也一定顺序接收到“hello”。

2：分割点包中间不会插入其他数据。

**为了==解决TCP报文的粘包和分包==问题，我们在项目的开发中可以采用==自定义==的报文格式！**

（这个可以写在简历上！！！但不太好讲，因为实现细节其实我还是悟不到到底是怎么deal的！！！**可能还得回看一次视频P4-2**）

比如：==报文长度+报文内容== ==0010==abcdefghi

在我们的TCP-socket的框架类中，**TcpRead()和TcpWrite()这两个函数**就已经实现了解决TCP粘包和分包问题了，我们先用起来先！（用起来之后再介绍其中的粘包和分包细节的处理）



在开发框架_public.h(_public.cpp)中，吴哥把socket通信的相关函数封装为了2个类，分别是：

（把socket通信的函数封装为类之后会非常易于使用，这使得我们可以把更多的精力放在业务代码的实现上面！）

```c++
// socket通讯的客户端类
class CTcpClient;
// socket通讯的服务端类
class CTcpServer
```



**注意：**在网络socket通信的过程中，如果一旦客户端或者服务端读取或发送数据报文失败了，那么99.99%的原因是因为某一端关闭了端口了！所以当我们在写网络通信的代码时，大可不必记录这种发送or读取报文数据失败这么详细的日志（除非人家要求要去这么干！！！）

我们可以先demo程序在把这两个类用起来，再讨论其实现细节的问题！



### 三、多进程的网络服务端

这个章节的会讲解如下内容：

实际开发中，网络通信模型是这样的：（往往是**一个服务端S**对应着**多个客户端C**）

服务端即可以使用多进程or多线程来与多个客户端进行连接do事情，

（这里第四章就用来多进程来do，我当然可以添加多线程来do这个事情！）

也可以使用IO多路复用技术，使用单进程（单个服务端进程）与多个客户端进程通信。（这个技术和运用以后再介绍！）

![image-20221111232455680](C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221111232455680.png)





**多进程服务端程序**的流程图：

<img src="C:/Users/11602/Desktop/Cpp%E5%A4%A9%E6%B0%94%E5%A4%A7%E9%A1%B9%E7%9B%AE/%E5%A4%9A%E8%BF%9B%E7%A8%8B%E6%9C%8D%E5%8A%A1%E7%AB%AF%E7%A8%8B%E5%BA%8F%E6%B5%81%E7%A8%8B%E5%9B%BE.JPG" alt="多进程服务端程序流程图" style="zoom:80%;" />



Linux多进程：

![image-20221112142926673](C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221112142926673.png)



#### 3.1：搭建多进程的网络服务程序框架(这个过程有很多点可以深挖，然后作为面试引导面试官对我进行提问)

##### 3.1.1：多进程网络服务程序框架细节and重点讲解：

在多进程中，父进程只负责Accept监听客户端有无连接到来（只会使用listenfd），而子进程只负责处理客户端通信（只会使用connfd），而且我们知道，父子进程各自是相互独立的，互不影响的，因此，关闭各自的文件描述符fd对各自也毫无影响。

so，我们可以

**在父进程中==关闭connfd==这个fd。**

```c++
TcpServer.CloseClient();
```

**在子进程中==关闭listenfd==这个fd。**

```c++
TcpServer.CloseListen();
```

**问题：有同学可能会问了，我为什么要关闭父子进程中的这两个文件描述符呢？为啥要折腾这一下子呢？不就是2个小小的fd而已嘛，不关闭就不关闭了！和项目开发有啥关系吗？**（这个点是可以写进去简历的难点中的！我项目中对这个问题进行了思考，然后写了这两行代码来deal这个问题！）

[Linux下每个进程能打开的文件描述符是有限制的](https://www.csdn.net/tags/MtjaQgysNTA5MzYtYmxvZwO0O0OO0O0O.html)

**答：**其实，这很有关系的！每个进程能够打开的文件描述符是有限制的，不可能一个进程能打开无限个fd，当一个进程打开的fd文件描述符越多，那么该进程消耗系统的资源也就越多，所以为了节省系统资源，提高服务端程序对性能，就必须要启用上面说的这两行代码来关闭对应的多余的文件描述符，更何况，这是多进程的模型，1S对多C，当有非常多客户端的连接到来时，服务端S进程就会打开很多很多的fd，这样就会消耗系统资源！



当然，服务端的程序肯定是需要日志来记录对应程序的运行状态的！（这个属于后台开发的常识）

此外：

1) **在多进程的服务程序中，如果杀掉一个子进程，和这个子进程通讯的客户端会断开，但是并不会影响其他的子进程和客户端，也不会影响父进程。**
2) **如果杀掉父进程，也不会影响还正在通讯中的子进程客户端，但是，新的客户端将无法再重新建立连接了。**
3) **如果用killall+程序名 的命令，可以杀掉父进程和全部的子进程。**



**多进程网络服务端程序的退出有==3种情况==**：

1）如果是子进程收到退出信号，则该子进程断开与客户端连接的socket，然后退出。（表示我们只想让某个客户端连接断开而已）

2）如果是父进程收到退出信号，父进程先关闭监听的socket，然后向全部的子进程发出退出信号。（表示我们想让所有的服务端与客户端的连接都断开）

3）如果父子进程都收到退出信号，则本质上与第2种情况相同。（表示我们想让所有的服务端与客户端的连接都断开）

基于以上3点，我们需要为客户端和服务端的程序分别设置**不同的信号退出代码**！

```c++
// 父进程退出函数。
void FatherExit(int sig)
{
  // 以下2行代码是为了防止信号处理函数在执行的过程中被信号中断。(lead to没法正常退出该父进程对应的服务端程序)
  signal(SIGINT,SIG_IGN);signal(SIGTERM,SIG_IGN);
  logfile.Write("父进程退出,sig=%d\n",sig); // 在日志文件中把信号的值也记录下来
  // 父进程退出时，需要给all的子进程发送退出信号，且关闭listenfd
  TcpServer.CloseListen();    // 关闭服务端用于监听的socket fd
  kill(0,15);                 // 通知全部的子进程退出(子进程id==0)
  // fork 函数返回值信息：
  // On success,the PID of the child process is returned in the parent, and 0 is returned in the child
  // SIGTERM==15 表示 采用“kill   进程编号”或“killall 程序名”通知程序。
  exit(0);
}
// 子进程退出函数。
void ChildExit(int sig)
{
  // 以下2行代码是为了防止信号处理函数在执行的过程中被信号中断。(lead to没法正常退出该子进程对应的客户端程序)
  signal(SIGINT,SIG_IGN);signal(SIGTERM,SIG_IGN);
  logfile.Write("子进程退出,sig=%d\n",sig); // 在日志文件中把信号的值也记录下来
  // 子进程退出时，需要关闭connfd
  TcpServer.CloseClient();    // 关闭客户端用于通信的socket fd
  exit(0);
}
```



**注意**：==实际上，不只是网络后台服务程序是这么do退出操作的，其他服务程序基本上也是类似如此来执行程序退出工作的，这需要我们把上面这两个函数都代码吃透，然后就能运用到自己的项目开发当中了！！！==



##### 3.1.2：在多进程网络服务程序框架中写一些业务代码的讲解：

（以网银APP的业务代码为例，让你学习如何在框架的基础上写业务的代码）

**网银APP通讯协议的业务描述（描述如下图and文字）**：

**注意**：网银APP是客户端C，银行的业务系统是服务端S，通讯报文的格式采用XML格式。

<img src="C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221112171540867.png" alt="image-20221112171540867" style="zoom: 67%;" />



**业务1**：登录服务，用户输入手机号和密码即可登录

客户端，需要这么do：发送业务代码（这里，srvcode==1表示登录业务）+手机号+密码的XML格式的报文

服务端，需要这么do：

若接收成功时，响应业务代码状态（这里是retcode=0表示登录成功）+message详细信息（显示登录成功）；

否则，返回响应业务代码状态（这里是retcode=-1表示登录失败）+message详细信息（显示用户名或密码不正确）；



<img src="C:/Users/11602/AppData/Roaming/Typora/typora-user-images/image-20221112172231283.png" alt="image-20221112172231283" style="zoom:67%;" />



**业务2**：查询卡里的账户余额服务（这里为了简化业务，假设只有一张卡）

客户端，需要这么do：发送业务代码（这里，srvcode==2表示查询账户余额的业务）+卡号的XML格式的报文

服务端，需要这么do：

若接收成功时，响应业务代码状态（这里是retcode=0表示查询卡余额成功）+message详细信息（显示余额）；

否则，返回响应业务代码状态（这里是retcode=-1表示登录失败）+message详细信息（显示卡号不存在）；





接下来，我们将来实现网银APP的业务逻辑：

```c++

```



#### 3.2：讲解TCP短连接/长连接 和 采用==心跳机制==来管理长连接的实现及其运用原理

本小节，视频将讲解TCP长连接和短连接的原理以及应用场景，和 采用 心跳机制来管理长连接的方法

若对TCP长短连接的知识点没有任何了解的话，先暂停视频，查看百度或CSDN博客了解一下先！

[TCP长短连接的知识点](https://blog.csdn.net/TABE_/article/details/117429913)







当然，==**本章节的重点**==是下面这块：

### 四、开发 基于TCP协议的文件传输系统

1：实现文件的上传和下载功能。

2：采用异步通信机制（比FTP上传下载文件的速度快很多倍），实现文件的快速传输。





我的一点关于该小项目的想法：

想法1：我后面可以稍微地学点QT界面，然后把我写的基于TCP协议的文件传输系统。用手机号+密码 或 用户名+密码来登录

想法2：或者，我干脆直接拿别人写好的服务器项目，添加我这里的系统代码，然后再整个登录页面，+后台mysql存放一些登录相关的表格，然后去使用起来！！！这样我的项目其实也挺丰富的了！！！(这都是能够把我所学的东西整合成一个项目的！！！)
