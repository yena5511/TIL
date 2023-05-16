마크다운이란?
===========

텍스트 기반의 마크업언어로 특수기호와 문자를 이용한 매우 간단한 구조의 문법을 사용하여 웹에서도 보다 빠르게 컨텐츠를 작성하고 보다 직관적으로 인식할 수 있다.

마크다운 사용법(문법)

큰제목: 문서 제목

this is an H1
=============



작은 제목: 문서 부제목

this is am H2
-------------



글머리 : 1~6까지만 지원

# This is a H1
## This is a H2
### This is a H3
#### This is a H4
##### This is a H5
###### This is a H6
####### This is a H7(지원하지 않는다)



BlockQuote
==========
이메일에서 사용하는 > 블럭인용문자를 이용한다
----------------------------------------


> This is a first blockqute.
>	> This is a second blockqute.
>	>	> This is a third blockqute.

이 안에서는 다른 마크다운 요소를 포함할 수 있다.



목록
===
● 순서있는 목록(번호)
-------------------
순서있는 목록은 숫자와 점을 사용한다.

1.첫번째
2.세번째
3.두번째



● 순서없는 목록(글머리 기호: *, +, - 지원)

>*빨강
>   >   -녹색
>   >   >+파랑


코드
====
4개의 공백 또는 하나의 탭으로 들여쓰기를 만나면 변환되기 시작하여 들여쓰지 않은 행을 만날때까지 변환이 계속된다.

들여쓰기
-------
코드
4개의 공백 또는 하나의 탭으로 들여쓰기를 만나면 변환되기 시작하여 들여쓰지 않은 행을 만날때까지 변환이 계속된다.

This is a normal paragraph:
This is a code block.
end code block.

한줄 띄어쓰지 않으면 인식이 제대로 안되는 문제가 발생합니다.


This is a normal paragraph:

    This is a code block.

end code block.



수평선 <hr/>
==========
아래 줄은 모두 수평선을 만든다. 마크다운 문서를 미리보기로 출력할 때 페이지 나누기 용도로 많이 사용한다.

* * *

***


링크
=========
일반적인 URL 혹은 이메일주소인 경우 적절한 형식으로 링크를 형성한다.
* 외부링크: <https://github.com/yena5511>




강조
=====
*single asterisks*

_single underscores_

**double asterisks**

__double underscores__

~~cancelline~~


줄바꿈
======
3칸 이상 띄어쓰기( )를 하면 줄이 바뀐다.

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기해야 한다.



