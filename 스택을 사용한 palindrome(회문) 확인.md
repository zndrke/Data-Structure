스택을 사용한 palindrome(회문) 확인

~~~c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_STACK_SIZE 10 //스택의 최대 크기
#define INITIAL_CAPACITY 2

int is_palindrome(char*);
typedef char element;
typedef struct {
	element *data;
	int capacity; //현재 최대 용량 
	int top; //현재 차있는 용량 
} Stack;

int create(Stack *s);
int is_empty(Stack *s);
int is_full(Stack *s);
int push(Stack *s, element item);
element pop(Stack *s);
element peek(Stack *s);

int main()
{
	char string[100];
	int result;

	printf("Input a string\n");
	gets(string);

	result = is_palindrome(string);

	if (result == 1)
		printf("\"%s\" is a palindrome string.\n", string);
	else
		printf("\"%s\" isn't a palindrome string.\n", string);

	return 0;
}

int is_palindrome(char *string)
{
	Stack s;
	create(&s);
	int i, j;
	char arr[strlen(string)];	//입력 크기의 배열 생성
	char ch;
	for (i = 0, j = 0;(ch = string[i]) != '\0';i++) {
		if (ch == '\n')					//뉴라인 나올때 까지 입력받음
			break;
		else if (ch >= 'a'&&ch <= 'z') {	//소문자이면 배열과 스택에 저장
			push(&s, ch);
			arr[j++] = ch;
		}
		else if (ch >= 'A'&&ch <= 'Z') {
			ch += 32;					//대문자이면 소문자로 바꿔서 저장
			push(&s, ch);
			arr[j++] = ch;
		}
	}

	for (i = 0;i<j;i++) {			//배열과 스택을 비교
		if (pop(&s) != arr[i])
			return 0;
	}
	return 1;
}

int create(Stack *s) {
	s->top = -1;
	s->capacity = INITIAL_CAPACITY; //맘대로 설정한 초기 크기
	s->data = (element *)malloc(s->capacity * sizeof(element));
}


int is_empty(Stack *s) {
	if (s->top == -1) {
		return 1;
	}
	else {
		return 0;
	}
}
int is_full(Stack *s) {
	if (s->top == (s->capacity) - 1) {
		return 1;
	}
	else {
		return 0;
	}

}
int push(Stack *s, element item) {
	if (is_full(s)) {
		//printf("stack is full; increasing size to %d\n", s->capacity * 2);
		s->capacity *= 2;
		s->data = (element *)realloc(s->data, s->capacity * sizeof(element));

	}

	(s->top)++;
	s->data[s->top] = item;

}
element pop(Stack *s) {
	if (is_empty(s)) {
		printf("스택이 비었습니다");
		exit(1);
	}
	else {
		return s->data[(s->top)--];

	}
}
element peek(Stack *s) {
	if (is_empty(s)) {
		printf("스택이 비었습니다");
		exit(1);
	}
	else {
		return s->data[s->top];
	}
}

~~~



is_palindrome 함수를 보면

입력받은 스트링을 알파벳(소문자로 변환하여)만 저장한다. 

스택과 배열에 각각 저장하여 뉴라인이 나오면 저장을 종료한다. 

배열에서는 스트링의 앞에서부터 추출되고, 스택에서는 배열의 뒤에서부터 추출된다.

전체 배열과 스트링을 비교하여 전부 일치하면 회문이고 하나라도 다르면 회문이 아니다.