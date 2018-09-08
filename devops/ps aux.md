# ps aux in mac / linux

### example

```bash
$ ps aux  
USER       PID  %CPU %MEM  VSZ RSS     TTY   STAT START   TIME COMMAND
timothy  29217  0.0  0.0 11916 4560 pts/21   S+   08:15   0:00 pine  
root     29505  0.0  0.0 38196 2728 ?        Ss   Mar07   0:00 sshd: can [priv]   
can      29529  0.0  0.0 38332 1904 ?        S    Mar07   0:00 sshd: can@notty  
```

### [OUTPUT DETAIL]

- **USER** = user owning the process
- **PID** = process ID of the process
- **%CPU** = It is the CPU time used divided by the time the process has been running.
- **%MEM** = ratio of the process’s resident set size to the physical memory on the machine
- **VSZ** = virtual memory usage of entire process (in KiB)
- **RSS** = resident set size, the non-swapped physical memory that a task has used (in KiB)
- **TTY** = controlling tty (terminal)
- **STAT** = multi-character process state
- **START** = starting time or date of the process
- **TIME** = cumulative CPU time
- **COMMAND** = command with all its arguments



이 중 필요한 프로세스에 대한 결과만 선택적으로 보고자 한다면 grep 명령을 같이 사용한다. 

### [OPTION]

* -l : 자세한 형태의 정보를 출력한다.  
* -u : 각 프로세서의 사용자 이름과 시작 시간을 보여준다.  
* -j : 작업 중심 형태로 출력한다. -s : 시그널 중심 형태로 출력한다. 
* -v : 가상 메모리 중심 형태로 출력한다. 
* -m : 메모리 정보를 출력한다.  
* -a : 다른 사용자들의 프로세서도 보여준다. 
* -x : 로그인 상태에 있는 동안 아직 완료되지 않은 프로세서들을 보여준다.  유닉스 시스템은 사용자가 로그아웃하고 난 후에도 임의의 프로세서가 계속 동작하게 할 수 있다. 그러면 그 프로세서는 자신을 실행시킨 셸이 없이도 계속 자신의 일을  수행한다. 이러한 프로세서는 일반적인 ps 명령으로 확인할 수 없다. 이때 
* -x 옵션을  사용하면 자신의 터미널이 없는 프로세서들을 확인할 수 있다.  
* -S : 차일드(child) CPU 시간과 메모리 페이지 결함(fault) 정보를 추가 한다.  
* -c : 커널 task_structure로 부터 명령 이름을 보여준다.  
* -e : 환경을 보여준다.  
* -w : 긴(wide) 형태로 출력한다. 한 행 안에 출력이 잘리지 않는다.  
* -h : 헤더를 출력하지 않는다.  
* -r : 현재 실행중인 프로세서를 보여준다. 
* -n : USER 와 WCHAN 을 위해 수치 출력을 지원한다.



출처 :

1. OUTPUT DETAIL : [https://superuser.com/questions/117913/ps-aux-output-meaning](https://superuser.com/questions/117913/ps-aux-output-meaning)

2. OPTION : http://lily.mmu.ac.kr/lecture/08sm/Fedora2/7jang/4.htm