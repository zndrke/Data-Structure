malloc을 사용하여 메모리 동적할당 

~~~c
// MALLOC.C: malloc을 이용하여 정수 10를 저장할 수 있는 동적 메모리를 
// 할당하고 free를 이용하여 메모리를 반납한다. 
#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>

#define SIZE 10

int main(void)
{
	int *p;

	p = (int *)malloc(SIZE * sizeof(int));
	if (p == NULL) {
		fprintf(stderr, "메모리가 부족해서 할당할 수 없습니다.\n");
		exit(1);
	}

	for (int i = 0; i<SIZE; i++)
		p[i] = i;

	for (int i = 0; i<SIZE; i++)
		printf("%d ", p[i]);

	free(p);
	return 0;
}
~~~



malloc을 사용하여 구조체 동적 할당

~~~c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct  studentTag {
	char name[10]; // 문자배열로 된 이름
	int age;	  // 나이를 나타내는 정수값
	double gpa;	  // 평균평점을 나타내는 실수값
} student;

int main(void)
{
	student *p;

	p = (student *)malloc(sizeof(student));	//student형 포인터를 student 타입의 사이즈 만큼 
	if (p == NULL) {
		fprintf(stderr, "메모리가 부족해서 할당할 수 없습니다.\n");
		exit(1);
	}

	strcpy(p->name, "Park");
	p->age = 20;

	free(s);
	return 0;
}
~~~

