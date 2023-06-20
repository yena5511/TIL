```c
#include <stdio.h>
#include<string.h> //문자열 함수
int main() {
	//1. 배열로 문자열 처리
	//strcpy()함수
	char a[] = "apple";
	char b[10];
	strcpy(a, "hello");
	strcpy(b, a);
	printf("%s\n", a);
	printf("%s\n", b);

	//strlen() 함수 (문자길이 반환)
	char c[] = "icecream";
	printf("%s의 길이 :%d\n", c, strlen(c)); //8


	//strcmp 함수(s1, s2) (문자열 비교)
	//같으면 0
	//뒤가 더 크면 음수
	//앞이 더 크면 양수
	char d[] = "abcd";
	char e[] = "abcd";
	printf("%d\n", strcmp(d, e));


	// strcat(s1, s2) 함수 : s1 끝에 s2 결함, 
	//s1의 문자열 크기 주의

	char f[20] = "ice";
	char g[] = "cream";
	strcat(f, g);
	printf("%s\n", f);





	//2. 포인터로 문자열 처리
		//c언어 모든 변수는 데이터 세그먼트 영역에 저장
		 //-> 읽기, 쓰기 다 가능
		//
		//"hello"는 문자열 상수
		//문자열 상수는 텍스트 세그먼트 영역에 저장
		 //-> 읽기만 가능


	char* p = "hello";
	//문자열 상수는 읽기 전용이기 때문에 쓰기 안 됨
	//p[0] = 'A';
	printf("%s의 주소 : %p, 크기 %d\n", p, p, sizeof(p));

	p = "apple";
	printf("%s의 주소 : %p, 크기 %d\n", p, p, sizeof(p));


	p = "how are you";
	printf("%s의 주소 : %p, 크기 %d", p, p, sizeof(p));

	
}
```

11p  21.연습문제
```c
#include<stdio.h>

int main() {
	int a[5] = { 1, 2, 3, 4, 5 };
	int result = 0, i = 0;
	int* p = a;
	for (i = 0; i < 5; i++) {
		result = result + *p;
		p++;   //주소증가
	}
	printf("배열의 합 = %d", result);
}
```

21. 연습문제
```c
#include<stdio.h>
int StringPointer(char* s);
int main() {
	int len;
	char string[100] = "";

	pritnf("문자열 입력: ");
	gets(string);

	printf("문자열 길이: %d \n", StringPointer(string));
}

int StringPointer(char* s) {

	int len = 0;
	while ( *s != '\0')
	{
		len++;
		s++;
	}
	return len;
}
```

21. 연습문제
```c
#include<stdio.h>
void Stringcopy(char* pa, char* pb);
int main() {
	char a[20] = "I LOVE YOU";
	char b[20];
	Stringcopy(a, b);
	printf("a 배열 : %s \n", a);
	printf("b 배열 : %s \n", b);
}


void Stringcopy(char* pa, char* pb) {
	int i;
	for (i = 0; pa[i] != '\0'; i++) {
		pb[i] = '\0';
	}

	pb[i] = '\0';
}
```
