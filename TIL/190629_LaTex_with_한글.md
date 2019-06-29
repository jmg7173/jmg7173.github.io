# LaTex로 한글로 논문작업하는 경우

LaTex로 논문작업하는데 한글로 하는 경우 아무 작업없이 한글 쓴 후 compile하면 에러가 난다.  
이러면 다음을 .tex 문서에 추가하면 된다.

```tex
\usepackage[T1]{fontenc}
\usepackage{CJKutf8}
...
\begin{document}
\begin{CJK}{UTF8}{mj}
논문으로 한글작업한다 이말이야
\end{CJK}
\end{document}
```

`\begin{CJK}{UTF8}{mj}`와 `\end{CJK}`를 `document`사이에 넣어주면 된다.

참고: http://oksure.org/archives/3086
