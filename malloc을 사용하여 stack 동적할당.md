malloc을 사용하여 stack 동적할당

~~~c
#include <stdio.h>
#include <stdlib.h>
#define INI_CAPACITY 2	//초기 스택을 2로 하고 2배씩 증가

typedef int element;
typedef struct{
	element *data;
	int capacity; 	//현재 최대 용량 
	int top;	//현재 차있는 용량 
} Stack;

int create(Stack *s){	//스택을 만드는 함수
	s->top=-1;
	s->capacity = INI_CAPACITY; 	//초기 용량 
	s->data = (element *)malloc(s->capacity*sizeof(element)); //배열 용량 초기화
} 

int is_empty(Stack *s){	//스택이 비었는지 검사하는 함수
	if(s->top ==-1)
		return 1;
	else 
		return 0;
}

int is_full(Stack *s){	//스택이 꽉 찼는지 검사하는 함수
	if(s->top== (s->capacity)-1)
		return 1;
	else 
		return 0;
}

int push(Stack *s,element item){
	if(is_full(s)){
		printf("stack is full; increasing size to %d\n",s->capacity*2);
		s->capacity *=2;
		s->data = (element *)realloc(s->data,s->capacity*sizeof(element));	
        //realloc 함수로 2배씩 늘림, (늘리고 싶은 배열, 새로운 용량*sizoof(데이터타입))
	}
	
	(s->top)++;
	s->data[s->top]= item;	//순서중요 탑을 옮기고 push
}

element pop(Stack *s){
	if(is_empty(s)==1){
		printf("stack is empty");
		return NULL;
	} 
	else {
		return s->data[(s->top)--];	//반환과 top감소를 한번에 함 
	}
}

element peek(Stack *s){
	if(is_empty(s)==1){
		printf("stack is empty");
		return NULL;
	} 
	else {
		return s->data[s->top];
	}
}

int main(void){
	
	Stack s;
	
	create(&s);
	
	push(&s,1);
	push(&s,2);
	push(&s,3);
	push(&s,3);
	push(&s,3);
	push(&s,3);
	push(&s,3);
	printf("%d\n",peek(&s));
	printf("%d\n",pop(&s));
	printf("%d\n",pop(&s));
	printf("%d\n",pop(&s));
	printf("%d\n",pop(&s));
	
	return 0;
}
~~~

