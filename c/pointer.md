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

## 포인터 배열
```c
#include <stdio.h>

int main() {
	//포인터 배열 : 포인터들을 배열로 만든 것

	int a = 1, b = 2, c = 3, d = 4, e = 5;
	int* arr[5]; //포인터 배열 선언
	arr[0] = &a;
	arr[1] = &b;
	arr[2] = &c;
	arr[3] = &d;
	arr[4] = &e;

	for (int i = 0; i < 5; i++) {
		printf("배열에 들어있는 값 : %d\n", *arr[i]);

	}
	
}
```

```c
#include <stdio.h>

int main() {
	char fruits[4][10] = {
		"apple",
		"blueberry",
		"orange",
		"melon"
	};
	for (int i = 0; i < 4; i++) {
		printf("%s\n", fruits[i]);
	}

}
```

```c
#include <stdio.h>

int main() {
	char* p[4] = {"apple", "blueberry", "orange", "melon"};
	//p[0] = "pointer";

	for (int i = 0; i < 4; i++) {
		printf("%s", p[i]);
		printf("\n");
	}

}
```

```c
#include <stdio.h>
#include <string.h>
int main() {
	
	char a[4][10] = { "you", "house", "home", "school" };
	char temp[10];

	strcpy(temp, a[0]);//you
	int max = strlen(a[0]);//3

	for (int i = 1; i < 4; i++) {
		if (max < strlen(a[i])) {
			max = strlen(a[i]);
			strcpy(temp, a[i]);
		}
	}
	printf("%s", temp);
}
```


```c

#include <stdio.h>
#include <string.h>
int main() {

	char* p[4]= { "you", "house", "home", "school" };

	char* temp = p[0];
	int max = strlen(p[0]);//3

	for (int i = 1; i < 4; i++) {
		if (max < strlen(p[i])) {
			max = strlen(p[i]);
			temp = p[i];
		}
	}
	printf("%s", temp);
}
```

```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<string.h>

int main() {
	//1. 영어 단어들을 저장하는 포인터 배열 선언
	char* dic[] = { "dog", "lion", "fox", "horse", "tiger", "raccoon", "bear", "dolphin", "elephant" };

	//2. 랜덤한 단어 하나 선택해서 word 포인터 변수에 저장
	srand(time(NULL));
	int num = rand() % (sizeof(dic) / sizeof(dic[0]));
	char* word = dic[num];
	//printf("%s\n", word);

	//3. word 포인터 변수가 가리키는 단어의 길이를 len 변수에 저장
	//ex. lion -> 4를 len에 저장
	int len = strlen(word);

	//4. 포인터 변수 pword를 선언하여 동적 메모리 할당 + 데이터를 모두 '-'로 채우기
	//ex. lion일 경우 --> _ _ _ _로 채우기 (띄어쓰기 제외)
	char* pword; //포인터를 선언하여 동적 메모리 주소로 사용
	pword = malloc(sizeof(char) * (len + 1));//동적 메모리 할당받은 메모리는 배열처럼 사용 가능
	for (int i = 0; i < len; i++) {
		pword[i] = '_';
	}
	//단어 마지막 끝에 null문자 넣기
	pword[len] = '\0';


	int cnt = 0;  //카운터 변수
	int user = 0; //
	char ch; // 사용자 문자 하나 입력받은 변수
	printf("\n<< 행맨 게임 만들기>>\n\n");

	while (1) {
        if (!strcmp(word, pword))
        {
            printf("\n%d번만에 성공!", cnt);
            free(pword);
            return 0;
        }
		cnt++;
		printf("\n 현재 문자 출력 : %s\n", pword);
		printf("문자 한 개 입력 : ");
		scanf("%c", &ch);
		rewind(stdin);//입력 버퍼 초기화 fflush(stdin);

		// 행맨 게임 알고리즘 찍기(반복문 사용)
        for (int i = 0; i < len; i++)
        {
            if (word[i] == ch)
            {
                pword[i] = ch;
            }
        }
	}

}
```