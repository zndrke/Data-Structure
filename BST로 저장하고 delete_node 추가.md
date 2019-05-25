BST로 저장하고 delete_node 추가

~~~c
#include <stdio.h>
#include <stdlib.h>

typedef int element;

typedef struct TreeNode{	//이진트리 
	element key;
	struct TreeNode *left, *right;
} TreeNode; 

TreeNode * search(TreeNode *node, element key);
TreeNode * new_node(element key);
TreeNode * insert_node(TreeNode * node, element key);
TreeNode * find_min_node(TreeNode  *node);
TreeNode * delete_node(TreeNode *node, element key);


int main(){
	TreeNode *root = NULL;
	
	root = insert_node(root, 15);
	root = insert_node(root, 20);
	root = insert_node(root, 5);
	root = insert_node(root, 7);
	//BST로 저장됨 
	
	printf("최소값 : %d \n", find_min_node(root)->key);
	
	printf("%d 찾기 \n",5);
	if (search(root,5)!=NULL) {
		printf("트리 안에 있음\n\n");
	} else{ //search 결과가 NULL
		printf("트리에 없음\n\n");
	}
	
	root = delete_node(root, 5);
	
	printf("%d 찾기 \n",5);
	if (search(root,5)!=NULL) {
		printf("트리 안에 있음\n");
	} else{ //search 결과가 NULL
		printf("트리에 없음\n");
	}
}


TreeNode * delete_node(TreeNode *node, element key){
	//0번째 : 트리가 비었음
	if (node ==NULL) {
		return node;
	}
	
	//지울 노드 찾기
	if (key < node->key){
		node->left = delete_node(node->left, key);
        //key가 작으면 왼쪽으로
	} else if(key > node->key){
		node->right = delete_node(node->right, key);
        //크면 오른쪽으로
	} else { //찾았음
		//1. 단말 노드임
		if(node->left == NULL && node->right ==NULL){
			free(node);
			return NULL;
		} 
		//2. 둘중 하나만 자식 노드 있음
		else if (node->left == NULL){   
			TreeNode * temp = node->right;
			free(node);
			return temp;
		} 
        else if (node->right == NULL){
			TreeNode * temp = node->left;
			free(node);
			return temp;
		} 
		//3. 양쪽 노드가 다 자식이 있음 
		else {
			TreeNode * temp = find_min_node(node->right); //선택 
			node->key = temp->key;
			
			node->right = delete_node(node->right, temp->key);	
		}
	}
}


TreeNode * find_min_node(TreeNode *node){
	if (node == NULL) return NULL;
	
	if (node->left ==NULL){
		return node;	
	} else {
		return find_min_node(node->left);
	}
	
}


TreeNode * insert_node(TreeNode * node, element key){
	
	if (node == NULL) // 트리가 빈 트리 
	{
		return new_node(key);
	} else {
		if (node->key == key) {
			return node;
		} else if( node->key > key){
			node->left = insert_node(node->left, key);
			// if (node->left == NULL) { node->left = new_node(key)}	
		} else if (node->key < key ) {
			node->right = insert_node(node->right, key);
		}
		
		return node;
		
	}
	 
}

TreeNode * new_node(element key){
	TreeNode * temp = (TreeNode *)malloc(sizeof(TreeNode));
	temp->key = key;
	temp->left = NULL;
	temp->right = NULL;
	return temp;
}



TreeNode * search(TreeNode *node, element target){
	//node == NULL일 경우 : 
	//트리가 비었거나 원하는 값이 없음  
	if (node ==NULL) return NULL;
	
	printf("NODE : %d \n", node->key);
	
	
	if (node->key == target){
		return node;
	} else if (node->key<target){ //target>현재노드  값 
		return search(node->right, target); 
	} else { //target < 현재노드
	 	return search(node->left, target); 
	}
	  
}
~~~



delete_node가 추가되었다.

BST모양으로 만들어진 트리에서 특정 노드를 삭제하고자 한다. 이때 

1. 특정 노드가 말단노드일떄
2. 특정 노드가 left 혹은 right 한쪽만 노드를 가질때
3. 특정 노드가 left right 노드를 모두 가질때

에 따라 다른 동작을 한다.



이에 앞서 특정 노드를 찾기 위해 key값보다 크면 오른쪽으로  

작으면 왼쪽으로 노드를 계속 이동한다. 

특정 노드의 부모 노드는 delete_node하여 대체된 노드를 left혹은 right 노드에 저장한다.



1번의 경우 말단 노드이기 때문에 삭제하고 NULL을 리턴하면 된다.

2번의 경우  특정노드를 제거하고 특정노드의 자식노드를 리턴하면 된다.

3번의 경우는 삭제한 특정노드를 대체할 노드를 선택해야 한다. 이때 두가지 방법이 있다

1) 왼쪽 노드의 가장 큰값으로 대체

2) 오른쪽 노드의 가장 작은값으로 대체

왜냐하면 왼쪽노드의 가장 큰 값은 오른쪽 노드의 가장 작은 값보다 작고

오른쪽 노드의 가장 작은 값은 왼쪽 노드의 가장 큰 값보다 크기 때문에 

오류가 발생하지 않는다. 코드에서는 2)의 방법을 사용했다.

3번의 순서는 

(1) 오른쪽에서 가장 작은 key를 찾아 tmp에 저장한다

(2) 현재 노드의 key를 tmp의 key로 대체하여 특정노드가 삭제되었다

(3) 오른쪽의 가장 작은 값인 tmp의 key를 delete_node를 통해 삭제한다.