링크드리스트 reverse추가

기존 링크드 리스트에서 reverse함수만 추가되었다.

~~~c

#include <stdio.h>
#include <stdlib.h>
typedef int element;

typedef struct ListNode{
	element data;
	struct ListNode* next;
}ListNode;

void insert(ListNode *head,ListNode *pre,element value);
ListNode* insert_first(ListNode *head,element value);
ListNode* deleted(ListNode *head,ListNode *pre );
ListNode* delete_first(ListNode* head);
ListNode* clear(ListNode* head);
element get_entry(ListNode* head,int pos);
int is_empty(ListNode* head);
void print_list(ListNode* head);

int search_list(ListNode* head, element value);

ListNode* reverse(ListNode* head);

int main(void){
	ListNode* head=NULL;
	head=insert_first(head,5);
	head=insert_first(head,4);
	insert(head,head->next,3);
	
	print_list(head);
	
	//뒤집기 
	
	head =reverse(head); 
	print_list(head);

	return 0;
}

void insert(ListNode *head,ListNode *pre,element value){
	ListNode* p=(ListNode*)malloc(sizeof(ListNode));
	p->data=value;
	p->next=pre->next;
	pre->next=p;
	return ;
}
ListNode* insert_first(ListNode *head,element value){
	ListNode* p=(ListNode*)malloc(sizeof(ListNode));
	p->data =value;
	p->next =head;
	head=p;
	return head;
}
ListNode* deleted(ListNode *head,ListNode *pre ){
	ListNode* deleted;
	
	if(pre->next==NULL){
		printf("cannot delete that node\n");
		return head;
	}
	deleted=pre->next;
	pre->next=deleted->next;
	free(deleted);
	return head;
}
ListNode* delete_first(ListNode* head){
	ListNode* deleted;
	deleted=head;
	head=deleted->next;
	free(deleted);
	return head;
}
ListNode* clear(ListNode* head){
	head=NULL;
	return head;
}
element get_entry(ListNode* head,int pos){
	ListNode* result;
	result =head;
	int i=0;
	for(i=0;i<pos;i++){
		result=result->next;
	}
	return result->data;
}
int is_empty(ListNode* head){
	if(head==NULL){
		printf("리스트가 빕\n");
		return 1; 
	}
	else 
		return 0;			
}
void print_list(ListNode* head){
	ListNode *p=head;
	for(;p->next!=NULL;p=p->next){
		printf("%d -> ",p->data);
	}
	printf("%d -> ",p->data);
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
	if (p->data==value){
			printf("%d를 찾았습니다\n",value);
			return 1;
		}
	printf("%d가 리스트에 없네요\n",value);
}

ListNode* reverse(ListNode* head){
	ListNode* cur;						//(1)
	ListNode* prev;
	ListNode* next;
	
	next= head;							//(2)
	cur=NULL;
	prev=NULL;
	while(next!=NULL){					//(3)
		prev=cur;
		cur=next;
		next=next->next;
		
		cur->next=prev;
	}
	return cur;							//(4)
}

void reverseprint(ListNode* head){		//재귀를 이용하여 역방향 출력
    ListNode* p;
    p=head;
    if(p==NULL)
        return;
    reverseprint(p->next);
    printf("%d ->",p->data);
}
~~~



1. 구조체 ListNode의 포인터 cur,prev,next를 만든다.

2. next는 head로, cur와 prev는 NULL로 초기화한다.

3. next가 NULL이 아닐 때까지 while문을 실행하여

   한칸씩 이동하는 동작(prev=cur ; cur=next ; next=next->next)과 cur의 next를 이전 노드를 가리키는 동작(cur->next=prev)를 실행한다. 

4. next가 NULL이되면 cur에는 마지막 요소까지 저장되어있다. 따라서 cur를 리턴하여 head에 저장하면 링크드 리스트가 역방향으로 저장된다.



reverseprint함수는 재귀를 이용하여 리스트를 역방향으로 출력하는 함수이다. 



