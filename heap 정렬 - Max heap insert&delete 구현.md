heap 정렬 - Max heap insert&delete 구현

~~~c
#include <stdio.h>
#include <stdlib.h>
#define MAX_ELEMENT 200

typedef struct {
	int key;
} element;

typedef struct {
	element heap[MAX_ELEMENT];
	int heap_size;
} HeapType;

HeapType* create();
void init(HeapType* h);
void insert_max_heap(HeapType* h, element item);
element delete_max_heap(HeapType* h);

int main(void)
{
	element e1 = { 2 }, e2 = { 6 }, e3 = { 7 }, e4 = {15}, e5 = {3} , e6 = {12};
	HeapType* heap;

	heap = create(); 	// 히프 생성
	init(heap);	// 초기화

	// 삽입
	insert_max_heap(heap, e1);
	insert_max_heap(heap, e2);
	insert_max_heap(heap, e3);
	insert_max_heap(heap, e4);
	insert_max_heap(heap, e5);
	insert_max_heap(heap, e6);
	
	
	printf(" %d, %d, %d\n", heap->heap[1], heap->heap[2], heap->heap[3]); 

	// 삭제
	e4 = delete_max_heap(heap);
	printf("< %d > ", e4.key);
	e5 = delete_max_heap(heap);
	printf("< %d > ", e5.key);
	e6 = delete_max_heap(heap);
	printf("< %d > \n", e6.key);

	free(heap);
	return 0;
}

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
void insert_max_heap(HeapType* h, element item)
{
	//item이 들어갈 힙의 위치
	int i = 0;
	i = h->heap_size + 1;
	h->heap_size++;  //힙 크기 조절
	
	//upheap
	while( (i!=1) && (item.key > (h->heap[i/2]).key ) ){
		//루트 노드가 아니고 부모보다 크면
		 
		h->heap[i] = h->heap[i/2] ;  
        //부모 자리의 노드를 자기 자리로 끌어내림  
		i = i / 2 ; //부모 자리로 올라감 
	} 
	h->heap[i] = item;
}

// 삭제 함수  (우선순위 큐에서의 dequeue ) 
element delete_max_heap(HeapType* h)
{
	element result, temp;
	int i; //현재 위치 
	int j; //내려갈 위치 
	
	result = h->heap[1]; //우선순위가 가장 높은 노드 
	temp = h->heap[h->heap_size] ;	//마지막 노드 
	h->heap_size--;
	//마지막 노드는 delete될 노드와 자리를 바꿈 
	
	
	i = 1; //temp가 들어갈 인덱스
	j = 2; //왼쪽 자식 인덱스 , j+1은 항상 오른쪽 자식 인덱스 

    
    //down heap
	while(j<=h->heap_size){
		if ((j < h->heap_size) && (h->heap[j].key < h->heap[j+1].key)) 
        {
			j++; //자식이 둘이고 오른쪽이 더 큰 경우 오른쪽 길로 
		}
		
		if (temp.key > h->heap[j].key) { //자식중 큰 값과 비교 
				//더 크면 멈춤 
				break; 
		} else {
			//내려감  
				h->heap[i] = h->heap[j];
				i = j;
				j = j * 2 ;
		}		
	} 
	h->heap[i] = temp;
	//temp의 위치에 저장 
	return result; //루트노드에 있던 값  
}
~~~



max_heap을 만들 때 insert_max_heap 연산은 upheap을 이용하고

delete_max_heap 연산은 downheap을 이용한다. 



먼저 insert연산은 

1. 힙의 size를 1증가시키고 증가된 크기를 i로 한다.
2. i가 루트(1)가 아니고 입력한 key가 부모 노드의 key보다 크면
3. 부모자리의 노드를 i 위치의 노드로 끌어 내리고
4. 인덱스 i를 i/2하여 부모 노드의 인덱스로 함 
5.  2.3.4.를 반복 하면 i가 1이되어 루트 노드이거나 부모가 i보다 커서 while문을 종료
6. 그 위치에 값을 저장한다.



delete연산은 

힙의 루트에 있는 데이터를 반환한다. 그리고 힙의 size에 위치한 데이터를 루트로 올려서 downheap하는 과정이다.

1. result에 루트 노드를 저장한다.
2. temp에 마지막 노드를 저장하고 힙의 size를 1 감소시킨다.
3. i는 1부터 j는 2부터 내려간다. 
4. j는 j(왼쪽노드)와 j+1(오른쪽노드)중에 더 큰 값을 j로 한다.
5. temp가 위에서 내려오면서 자식보다 크면 멈춘다. 
6. 자식보다 작으면 i는 j로 j는  j*2하여 아래로 내려간다.
7. 4.5.6. 과정을 반복하여 자식보다 큰 위치에 temp를 저장한다
8. 마지막으로 result를 리턴하여 루트에 있던 가장 큰 값을 반환한다.

