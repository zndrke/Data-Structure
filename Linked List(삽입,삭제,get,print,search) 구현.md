Linked List(삽입,삭제,get,print,search) 구현

~~~c
#include <stdio.h>
#include <stdlib.h>

typedef int element;	 

typedef struct ListNode{
	element data;			//노드의 데이터 저장 부분 
	struct ListNode* next;	//노드의 다음 노드 포인터 
}ListNode;

void insert(ListNode *head,element index,element value);
	//index번째 노드의 다음 노드에 value를 삽입하는 함수 
ListNode* insert_first(ListNode *head,element value);
	//head위치에 value를 삽입하는 함수 
ListNode* deleted(ListNode *head,element index);
	//index번째 노드를 삭제하는 함수 
ListNode* delete_first(ListNode* head);
	//head를 삭제하는 함수 
ListNode* clear(ListNode* head);
	//리스트를 초기화 
element get_entry(ListNode* head,int pos);
	//pos번째 리스트의 value를 뽑는 함수 
int is_empty(ListNode* head);
	//빈 리스트인지 확인하는 함수 
void print_list(ListNode* head);
	//리스트를 전부 출력하는 함수 
int search_list(ListNode* head, element value);
	//value가 존재하는지 확인하는 함수 
int main(void){
	ListNode* head=NULL;		//리스트를 생성 
	
	while(1){
		element a,b,c;
		printf("1.insert 2.insert first 3. delete 4. delete first\n");
		printf("5.clear 6. get entry 7. print list 8. search list\n");
		scanf("%d",&a);
		switch(a){
			case(1):	//insert
				printf("input index & value\n");
				scanf("%d %d",&b,&c);
				insert(head,b,c);
				break;
			case(2):	//insert first
				printf("input value\n");
				scanf("%d", &c);
				head=insert_first(head,c);
				break;
			case(3):	//delete
				printf("input index\n");
				scanf("%d",&b);
				head=deleted(head,b);
				break;
			case(4):	//delete first
				head=delete_first(head);
				break;
			case(5):	//clear
				head=clear(head);
				break;
			case(6):	//get_entry
				printf("input index\n");
				scanf("%d",&b);
				printf("%d번째 data는 %d\n",b,get_entry(head,b));
				break;
			case(7):	//print_list
				print_list(head);
				break;
			case(8):	//search_list
				printf("input value\n");
				scanf("%d",&c);
				search_list(head,c);
				break;	
		}
	} 
}



void insert(ListNode *head,element index,element value){
	ListNode* p=(ListNode*)malloc(sizeof(ListNode));	//새 노드 생성
	p->data=value;		//value 저장
	int i;
	for(i=0;i<index;i++){
		head=head->next;	//삽입할 위치를 찾음
	}
	p->next=head->next;		//p->next에 삽입할 index의 next 값을 넣고
	head->next=p;			//삽입할 index의 next에는 p를 저장
	return ;
}
ListNode* insert_first(ListNode *head,element value){
	ListNode* p=(ListNode*)malloc(sizeof(ListNode));
	p->data =value;		//생성된 노드에 값 저장
	p->next =head;		//head를 p->next에 저장
	head=p;				//생성된 노드 p는 새로운 head가 됨
	return head;
}
ListNode* deleted(ListNode *head,element index ){
	ListNode* deleted;		//삭제할 노드

	int i;
	for(i=0;i<index-1;i++){		//삭제할 노드의 직전 노드로 감
		head=head->next;
	}

	deleted=head->next;			//다음 노드를 삭제할 노드로 저장
	head->next=head->next->next;	//다다음 노드 값을 다음 노드로 저장
	free(deleted);				//삭제할 노드를 free시킴
	return head;
}
ListNode* delete_first(ListNode* head){
	ListNode* deleted;
	deleted=head;			//삭제할 노드 head를 delete에 저장
	head=deleted->next;		//head에 delete의 next를 저장
	free(deleted);
	return head;
}
ListNode* clear(ListNode* head){
	head=NULL;				//헤드를 초기화
	return head;
}
element get_entry(ListNode* head,int pos){
	ListNode* result;
	result =head;
	int i=0;
	for(i=0;i<pos;i++){
		result=result->next;	//값을 뽑을 노드로 이동
	}
	return result->data;		//값 반환
}
int is_empty(ListNode* head){
	if(head==NULL){				//head가 NULL이면 빈리스트
		printf("리스트가 빕\n");
		return 1; 
	}
	else 
		return 0;			
}
void print_list(ListNode* head){
	ListNode *p=head;
	if(is_empty(p)==1)
		return ;
		
	for(;p->next!=NULL;p=p->next){	//NULL이 아닐때까지 print
		printf("%d -> ",p->data);
	}
	printf("%d -> ",p->data);		//next가 NULL인 노드 데이터 출력
	printf("NULL\n");
}

int search_list(ListNode* head, element value){
	ListNode* p;
	for(p=head;p->next!=NULL;p=p->next){
		if (p->data==value){
			printf("%d를 찾았습니다\n",value);
			return 1;
		}
	}
	if (p->data==value){				//마지막 노드 다시 확인
			printf("%d를 찾았습니다\n",value);
			return 1;
		}
	printf("%d가 리스트에 없네요\n",value);
}
~~~

링크드 리스트 구현과 테스트하는 함수를 메인에 넣었다.

링크스 리스트에서 삽입과 삭제 구현에 조심해야 한다.

삽입할때는 

1. 새로운 노드(p) malloc을 통해 생성
2. p->data에 값 저장
3. n+1노드의 주소(n->next에 저장되어 있음)를 p->next에 저장
4. n->next에 p저장 



삭제는

1. 삭제할 노드를 저장할 deleted생성
2. n-1인덱스로 이동하여 (n-1)->next를 deleted에 저장
3. (n-1)->next가 (n-1)->next->next를 가리키게 함
4. deleted를 free하여 메모리 반환

