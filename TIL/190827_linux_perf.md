# Linux Tools에 있는 perf compile 해 사용하기

자신 버전에 맞는 `linux-tools`를 apt로 설치해도 되지만, 몇 버전 이하의 커널에 대한 tools는 apt로 설치가 불가하다.  

실험 컴퓨터는 4.1.52 버전 커널을 사용하기 때문에, linux kernel 4.1.52 source code를 다운받아 내장되어있는 perf를 compile 해 사용한다.

perf는 다음과 같은 방법으로 make 가능하다.
```bash
$ make -C tools/ perf_install # linux source code folder 
$ make perf_install # tools/ folder
$ make perf # tools/ folder
```
그리고 자신의 cpu 코어 갯수에 맞는 옵션을 지정해주면 된다(ex. `-j4`)

make 시 다음과 같은 에러가 발생하면 이 [패치](https://lore.kernel.org/patchwork/patch/850380/)를 참조하자

```
tools/perf/bench/futex.h:36:10: error: 'SYS_futex' undeclared
tools/perf/tests/mmap-thread-lookup.c:42:20: error: 'SYS_gettid' undeclared
```

또한 warning에 대해 error가 발생하는 경우, `(linux source code)/tools/perf/Makefile.perf`에 한줄을 추가하면 warning을 error로 처리하지 않는다.  
```Makefile
WERROR=0
```
