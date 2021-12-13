## 进程

```c
#define pid_t short
```



- `pid_t fork()`

  - 创建一个子进程，创建失败返回-1，创建成功对父进程返回子进程的pid，对子进程返回0

  - 从`fork()`后的语句开始，子进程和父进程并发执行

  - 进程创建时会进行虚拟内存的复制（改变时对物理内存进行复制）

    ```c
    #include <stdio.h>
    #include <unistd.h>
    #include <sys/wait.h>
    
    int main(){
            int a = 1;
            int *p = &a;
            printf("parent : %p\t%d\n", p, *p);	//parent : 0x7fffe1c82544 1
            pid_t pid = fork();
            if(pid == 0){
                    *p = 2;
                    printf("child : %p\t%d\n", p, *p);	//child : 0x7fffe1c82544  2
                    exit(0);
            }
            else{
                    wait(NULL);
                    printf("parent : %p\t%d\n", p, *p); //parent : 0x7fffe1c82544 1
            }
    }
    ```

    

- `int exec??()`

  - 系统调用一个新进程覆盖当前进程（当前进程后续代码都不执行)

  - 不关闭已打开的文件

    ```c
    // ./test/test.c
    #include <stdio.h>
    #include <unistd.h>
    
    int main(){
            int status = 0;
            pid_t pid = fork();
            if(pid == 0){
                    printf("child process :");
                    execl("./hello", "hello", NULL);
                    exit(1);
            }
            else{
                    wait(&status);
                    printf("status : %d\n", status);
            }
    }
    
    // ./test/hello.c
    #include <stdio.h>
    
    int main(){
            printf("hello world!\n");
    }
    
    /**output
    child process : hello world!
    status : 0
    
    * wait(&status)是可以接收到hello的返回值的，比如返回a的话，status的值就是(a<<8)
    */
    ```

    

- ```c
  pid_t wait(int *status);	//存在status的低8位，可以用(status>>8)获取
  void exit(int status);		
  ```

  

- 进程标识符：

  ```c
  pid_t getpid();		//本身标识符
  pid_t getppid();	//父进程标识符
  
  pid_t setpgrp();	//形成新的组，本进程为组首，子进程继承父进程的组标识符
  
  ```

  

  

