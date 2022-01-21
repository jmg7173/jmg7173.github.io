# Docker container 내에서 tunneling하기

## Prerequisition
* Apt packages (on container)
  * openssh-server
  * (optional?) tmux

## 왜 하게 되었을까?

회사 서버 GPU 리소스가 부족해서 한 서버만 GPU(model inference)를 사용하도록 하고자 했다.  
그런데 ssh 포트가 서로 뚫려있길래 회사 내부 서버끼리 서비스에서 사용되는 포트들도 서로 뚫려있을 줄 알았는데 아니였다.  
web client에서 파일을 업로드하면 api server에서 분석 요청을 하도록 하는 로직인데, 파일이 많고 크다보니 api server로 inference server가 파일을 요청할 수 있도록 api server의 정보를 넘겨준다.
어떻게 해결하려고 했는지는 다음과 같다.
* 뚫고자하는 host에서 `ssh -L port:remote_ip:port user@remote_ip`로 뚫으려고 함
  * 당연히 request를 받아서 로직 처리할 때 api server로 다시 request 보내는건 container 내부에서 하니까 host에서 뚫어봤자 의미가 없음
* 그래서 container 내에서 똑같이 tunneling하려고 함
  * 근데 port가 안뚫림(????)
  * 이때 발생한 에러 메세지는 다음과 같음. `cannot assign requested address`
* [찾아보니](https://stackoverflow.com/a/63364551) ipv4임을 명시해줘야한다고 함 (`ssh -4 -L ...`)
  * 당연히 api쪽에는 요청 보낼 주소를 localhost:port로 줘야함(이 주소로 컨테이너 내에서 요청하면 터널링해서 요청하니까)
  * 연결 잘 됨!
  * 그리고 이 session 유지해야하니까 container 내에서 tmux session 열고 터널링하고 detach함
