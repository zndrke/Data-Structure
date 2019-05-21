Binary Tree와 전위, 중위, 후위 탐색

~~~c
#include <stdio.h>

typedef struct TreeNode {
	int data;
	struct TreeNode *left, *right;
} TreeNode;

//         n7
//      n5    n6
//    n1 n2  n3 n4

void preorder(TreeNode *root);
void inorder(TreeNode *root);
void postorder(TreeNode *root);

int main(){

	TreeNode n1 = {1, NULL, NULL };
	TreeNode n2 = {2, NULL, NULL };
	TreeNode n3 = {3, NULL, NULL };
	TreeNode n4 = {4, NULL, NULL };
	TreeNode n5 = {5, &n1, &n2 };
	TreeNode n6 = {6, &n3, &n4 };
	TreeNode n7 = {7, &n5, &n6 };
	TreeNode *root = &n7;
	
	
	printf("preorder : ");
	preorder(root);
	printf("\n");
	
	printf("inorder : ");
	inorder(root);
	printf("\n");
	
	printf("postorder : ");
	postorder(root);
	printf("\n");
	
    
    
	// 동적할당하여 값을 저장할 경우 아래와 같이 함 
	
//	TreeNode *n1, *n2, *n3;
//	
//	n1 = (TreeNode *)malloc(sizeof(TreeNode));
//	n2 = (TreeNode *)malloc(sizeof(TreeNode));
//	n3 = (TreeNode *)malloc(sizeof(TreeNode));
//	
////	//root
////	n3->data = 30;
////	n3->left = n1;
////	n3->right = n2;
////	
////	n1->data = 10;
////	n1->left = NULL;
////	n1->right = NULL;
////	
////	n2->data = 20;
////	n2->left = NULL;
////	n2->right = NULL;
//	
//	printf("   %d\n",n3->data );
//	printf("   /  | \n");
//	printf("  %d  %d",n3->left->data, n3->right->data);
//	
//	
}

void preorder(TreeNode *root){
	 if (root == NULL) return;
	 
	 printf("[%d]  ", root->data); //시킬 작업 
	 preorder(root->left);
	 preorder(root->right); 
}

void inorder(TreeNode *root){
	 if (root == NULL) return;
	 
	 inorder(root->left);
 	 printf("[%d]  ", root->data); //시킬 작업 
	 inorder(root->right); 
}
void postorder(TreeNode *root){
	 if (root == NULL) return;
	 
	 postorder(root->left);
	 postorder(root->right); 
  	 printf("[%d]  ", root->data); //시킬 작업 

}
~~~



전위: left -> 루트 -> right

중위: 루트 -> left -> right

후위: left -> right -> 루트



