heap을 이용한 학생관리

- 최저점수 5명 출력

- 하루가 지나 하위 5명 점수 10점씩 증가

- 새로운 하위 5명 출력

  

~~~c
#include <stdio.h>
#include <stdlib.h>
#define MAX_ELEMENT 200
#define MAX_STUDENT 100


typedef struct {
	char id;
	int score;
} element;
typedef element Student;

typedef struct Class{
	Student* s[MAX_STUDENT];
	int n;
}Class;
typedef struct {
	element* heap[MAX_ELEMENT];
	int heap_size;
} HeapType;


// 생성 함수
HeapType* create()
{
	return (HeapType*)malloc(sizeof(HeapType));
}
// 초기화 함수
void init(HeapType* h)
{
	h->heap_size = 0;
}



// 현재 요소의 개수가 heap_size인 히프 h에 item을 삽입한다.
// 삽입 함수
void insert_min_heap(HeapType* h, element* item)
{
	int i;
	i = ++(h->heap_size);

	//  트리를 거슬러 올라가면서 부모 노드와 비교하는 과정
	while ((i != 1) && (item->score < h->heap[i / 2]->score)) {
		h->heap[i] = h->heap[i / 2];
		i /= 2;
	}
	h->heap[i] = item;     // 새로운 노드를 삽입
}
// 삭제 함수
element* delete_min_heap(HeapType* h)
{
	int parent, child;
	element *item, *temp;
	
	item = h->heap[1];
	temp = h->heap[(h->heap_size)--];
	parent = 1;
	child = 2;
	while (child <= h->heap_size) {
		// 현재 노드의 자식노드중 더 작은 자식노드를 찾는다.
		if ((child < h->heap_size) &&
			(h->heap[child]->score) > h->heap[child + 1]->score)
			child++;
		if (temp->score < h->heap[child]->score) break;
		// 한 단계 아래로 이동
		h->heap[parent] = h->heap[child];
		parent = child;
		child *= 2;
	}
	h->heap[parent] = temp;
	return item;
}

void add_student(Class *c, char id, int score){
	Student *s;
    s = (Student *)malloc(sizeof(Student));
    
    //학생의 id와 score를 저장하고 class에 s저장
    s->id=id;
    s->score=score;
    
	c->s[c->n++]=s;

	return;
}

void print_student(Student s){

	printf("학생정보 : 이름 %c, 점수 %d\n", s.id ,s.score);
}

void pass_one_day(Class *c){
	
	printf("하루가 지나 하위 5명의 점수가 10점 증가\n");
	
	HeapType* h;					//힙을 생성하고
	h=create();
	init(h);
	int j;
	for(j=0;j<sizeof(c);j++){		//c의 학생정보를 힙에 insert함
		insert_min_heap(h,c->s[j]);
	}
	
	int i;
	element *arr[200];
	for(i=0;i<5;i++){
		arr[i]=delete_min_heap(h);	//delete를 통해 학생정보를 가져온다
		arr[i]->score+=10;			//학생의 score를 10 증가한다.
	}
}
//arr[i]->score+=10이 c에 저장된 student의 score를 바꾸는 이유는
//insert에 c->s[j]를 통해 학생의 주소값이 전달되고
//delete에서 힙에 저장된 학생의 주소값을 리턴하고
//그 값을 student(element)를 저장하는 arr에 저장하고 값을 변경하기 때문에
//즉, 주소값을 계속 넘겨주기 때문에 arr의 변경이 c의 변경이 되는 것이다.




void print_bottom_five_students(Class *c){
	
	printf("하위 5명 학생 : \n");
	
	HeapType* h;
	h=create();
	init(h);
	int j;
	for(j=0;j<sizeof(c);j++){
		insert_min_heap(h,c->s[j]);
	}
	
	int i;
	element *arr[200];
	for(i=0;i<5;i++){
		arr[i]=delete_min_heap(h);
		print_student(*arr[i]);	
        //print_student는 student형 변수를 인자로 받기 떄문에
        //*arr[i]로 student value를 전달해야 한다.
	}
}

int main(void)
{

	Class *c;
	c = (Class *)malloc(sizeof(Class));
	
	c->n=0; //학생 수 초기화 
	
	add_student(c, 'A',80);
	add_student(c, 'B',71);
	add_student(c, 'C',62);		
	add_student(c, 'D',89);		
	add_student(c, 'E',53);		
	add_student(c, 'F',84);		
	add_student(c, 'G',75);		
	add_student(c, 'H',93);		
	
	int i = 0;
	for (i = 0; i<8; i++){
		print_student(*(c->s[i]));
	}
	
	
	print_bottom_five_students(c);

	pass_one_day(c);
	
	print_bottom_five_students(c);

}
~~~

