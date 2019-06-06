heap을 이용한 huffman 코드

~~~c
#include <stdio.h>
#include <stdlib.h>
#define MAX_ELEMENT 200

typedef struct TreeNode {
	int weight;
	char ch;
	struct TreeNode *left;
	struct TreeNode *right;
} TreeNode;

typedef struct {
	TreeNode* ptree;
	char ch;
	int key;
} element;

typedef struct {
	element heap[MAX_ELEMENT];
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
void insert_min_heap(HeapType* h, element item)
{
	int i;
	i = ++(h->heap_size);

	//  트리를 거슬러 올라가면서 부모 노드와 비교하는 과정
	while ((i != 1) && (item.key < h->heap[i / 2].key)) {
		h->heap[i] = h->heap[i / 2];
		i /= 2;
	}
	h->heap[i] = item;     // 새로운 노드를 삽입
}
// 삭제 함수
element delete_min_heap(HeapType* h)
{
	int parent, child;
	element item, temp;

	item = h->heap[1];
	temp = h->heap[(h->heap_size)--];
	parent = 1;
	child = 2;
	while (child <= h->heap_size) {
		// 현재 노드의 자식노드중 더 작은 자식노드를 찾는다.
		if ((child < h->heap_size) &&
			(h->heap[child].key) > h->heap[child + 1].key)
			child++;
		if (temp.key < h->heap[child].key) break;
		// 한 단계 아래로 이동
		h->heap[parent] = h->heap[child];
		parent = child;
		child *= 2;
	}
	h->heap[parent] = temp;
	return item;
}

// 이진 트리 생성 함수
TreeNode* make_tree(TreeNode* left,TreeNode* right)
{
	TreeNode* node = (TreeNode*)malloc(sizeof(TreeNode));
	node->left = left;
	node->right = right;
	return node;
}
// 이진 트리 제거 함수
void destroy_tree(TreeNode* root)
{
	if (root == NULL) return;	//제거할 트리가 없음
	destroy_tree(root->left);	//왼쪽 순회
	destroy_tree(root->right);	//오른쪽 순회
	free(root);
}

int is_leaf(TreeNode* root)
{
	return !(root->left) && !(root->right);
    //노드의 left 와 right가 없으면 leaf노드
}
void print_array(int codes[], int n)
{
	int i;
	for (i = 0; i < n; i++)
		printf("%d", codes[i]);
	printf("\n");
}

void print_codes(TreeNode* root, int codes[], int top)
{

	// 1을 저장하고 순환호출한다. 
	if (root->left) {
		codes[top] = 1;
		print_codes(root->left, codes, top + 1);
        //왼쪽노드의 경우 오른쪽 노드보다 빈도수가 낮다
        //왼쪽 노드는 배열에 1을 저장하고 top을 증가
	}

	// 0을 저장하고 순환호출한다. 
	if (root->right) {
		codes[top] = 0;
		print_codes(root->right, codes, top + 1);
        //오른쪽 노드는 빈도수가 높은 노드로 0을 저장한다
	}

	// 단말노드이면 코드를 출력한다.
	if (is_leaf(root)) {
		printf("%c: ", root->ch);	//문자를 출력하고
		print_array(codes, top);	//코드를 출력한다.
	}
}

// 허프만 코드 생성 함수
void huffman_tree(int freq[], char ch_list[], int n)
{
	int i;
	TreeNode *node, *x;
	HeapType* heap;
	element e, e1, e2;
	int codes[100];
	int top = 0;

	heap = create();
	init(heap);
	for (i = 0; i<n; i++) {		//노드 n개를 생성함
		node = make_tree(NULL, NULL);	//노드를 생성하고
		e.ch = node->ch = ch_list[i];	//문자를 저장
		e.key = node->weight = freq[i];	//빈도수를 저장
		e.ptree = node;		//노드를 ptree에 저장
		insert_min_heap(heap, e);	//힙을 만든다
	}
	for (i = 1; i<n; i++) {	//합치는 과정은 n-1번 시행
		// 최소값을 가지는 두개의 노드를 삭제
		e1 = delete_min_heap(heap);
		e2 = delete_min_heap(heap);
		// 두개의 노드를 합친다.
		x = make_tree(e1.ptree, e2.ptree);
        //e1을 left로 e2를 right로 하는 트리를 만듦
		e.key = x->weight = e1.key + e2.key;
		e.ptree = x;
		printf("%d+%d->%d \n", e1.key, e2.key, e.key);
		insert_min_heap(heap, e);
        //두 노드의 빈도수를 합쳐서 min힙으로 다시 넣는다.
	}
	e = delete_min_heap(heap); // 최종 트리
	print_codes(e.ptree, codes, top);
	destroy_tree(e.ptree);
	free(heap);
}

int main(void)
{
	char ch_list[] = { 's', 'i', 'n', 't', 'e' }; //문자와
	int freq[] = { 4, 6, 8, 12, 15 };			//빈도수 설정
	huffman_tree(freq, ch_list, 5);				//허프만 코드로 변환
	return 0;
}
~~~

허프만 코드란 :

주어진 문자열을 트리를 이용해 2진수로 압축하는 알고리즘이다.

문자의 빈도수가 높을 수록 짧은 코드를 부여

문자는 하나당 8비트를 할당한다. 이를 압축하여 최소 2비트로 시작하는 문자로 변환한다.



변환 과정은

1. 빈도수를 기준으로 모든 문자를 heap에 min_insert한다.

2. 가장 빈도수가 적은 2 노드를 추출하여 두 노드의 빈도수를 더하고

   더해진 빈도수를 다시 min_insert한다.

3. 이 과정을 n-1번 반복하면 트리가 완성된다.

4. print_code를 통해 왼쪽으로 갈 때 1을 찍고 오른쪽으로 갈때 0을 찍는다.

5. 말단 노드의 높이에 따라 코드 길이가 부여된다.

6. 이렇게 만들어진 코드는 서로 중복되지 않는다.

7. 복호화 과정은 코드에 맞춰 트리를 따라 내려가면 문자를 찾을 수 있다.