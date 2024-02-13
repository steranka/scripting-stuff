# Basics on a UNIX process

- redirection - stdin, stdout, stderr, /dev/tty
- mywhoami - Effective UserID (EUID), 
- process groups
  ./test-process-groups 1
  PGID=xxx
  kill -2 -${PGID}


```
sudo -u nobody ./mywhoami
nobody
uid=65534(nobody) gid=65534(nogroup) groups=65534(nogroup
```

(Q) How can I tell if my output is being redirected to a pipe?
(A) See SO https://stackoverflow.com/questions/911168/how-can-i-detect-if-my-shell-script-is-running-through-a-pipe

if [ -t 1 ] ; then echo terminal; else echo "not a terminal"; fi
if [ -t 1 ] ; then echo terminal; else echo "not a terminal"; fi | cat


    
## Running process groups
To run the test that starts multiple process groups run
./test-process-groups 3 &
pstree -gl | less
Look for  output like this
```
-bash(2631541)-+-less(2652764)
               |-pstree(2652764)
               `-test-process-gr(2651650)-+-sleep(2651650)
                                          |-start-process-g(2651650)-+-sleep(2651650)
                                          |                          |-start-process(2651650)---sleep(2651650)
                                          |                          |-start-process(2651650)---sleep(2651650)
                                          |                          `-start-process(2651650)---sleep(2651650)
                                          |-start-process-g(2651650)-+-sleep(2651650)
                                          |                          |-start-process(2651650)---sleep(2651650)
                                          |                          |-start-process(2651650)---sleep(2651650)
                                          |                          `-start-process(2651650)---sleep(2651650)
                                          `-start-process-g(2651650)-+-sleep(2651650)
                                                                     |-start-process(2651650)---sleep(2651650)
                                                                     |-start-process(2651650)---sleep(2651650)
                                                                     `-start-process(2651650)---sleep(2651650)
```
Each process is running in the same process group, so if the process group is killed every process
in the process group is killed.

