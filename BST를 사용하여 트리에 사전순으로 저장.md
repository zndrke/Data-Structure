BST를 사용하여 트리에 사전순으로 저장

~~~c
#include <stdio.h>
#include <ctype.h>
#include <string.h>
#include <stdlib.h>

#define BUFSIZE 100
#define NUMBER 0
#define MAXWORD 100

char buf[BUFSIZE];
int bufp = 0;

struct tnode{			
    char *word;			//문자열을 담는 char 포인터
    int count;			//카운팅
    struct tnode *left;	//왼쪽 자식노드
    struct tnode *right;	//오른쪽 자식노드
};

struct tnode *talloc(void){		//tnode를 초기화하는 함수
    return (struct tnode *) malloc(sizeof(struct tnode));
}

int getch(void)
{
    return (bufp > 0)? buf[--bufp] : getchar();
    //버퍼가 비어있지 않으면 버퍼에서 데이터를 가져온다
    //버퍼가 비어있으면 getchar로 데이터를 입력받음
}

void ungetch(int c)
{
    if (bufp >= BUFSIZE)
        printf("ungetch: too many characters\n");	//buffer is full
    else
        buf[bufp++] = c;	//버퍼에 저장
}

int getword(char *word, int lim)
{
    int c, getch(void);
    void ungetch(int);
    char *w = word;
    
    while (isspace(c = getch()))	
        //getch로 입력받은 것이 공백이 아니면 빠져나감
        ;
    if(c != EOF)		//c가 EOF가 아니면
        *w++ = c;		//w에 c를 저장함
    if(!isalnum(c)){	//c가 알파벳이 아니면
        *w = '\0';		//string을 만든다
        return c;		
    }	
    for( ; --lim > 0; w++)	//c가 알파벳이면
        if(!isalnum(*w = getch())){	//if *w=getch() is not alpha or num 
            ungetch(*w);			//then ungetch(*w)
            break;			
        }
        //만약 *w=getch()이 알파벳이나 숫자이면 w에 저장한다
    
    *w = '\0';	//make string
    
    return word[0];	//스트링의 첫번째 주소를 반환함
}

struct tnode *addtree(struct tnode *p, char *w){
    int cond;
    
    if (p == NULL){		//p가 할당되지 않으면
        p = talloc();	
        p->word = strdup(w);	//p의 word에 w를 복제하고
        p->count = 1;			//카운트를 1로 함
        p->left = p->right = NULL;	//left node and right node is NULL
    } else if ((cond = strcmp(w, p->word)) == 0)	//w가 word이면 
        p->count++;			//카운트를 증가
    else if (cond < 0)		//word가 작으면
        p->left = addtree(p->left, w);	//왼쪽 노드로 이동
    else				//크면
        p->right = addtree(p->right, w);	//오른쪽으로 이동
    return p;
}

void treeprint(struct tnode *p)	//중위순회를 함
{
    if (p != NULL)	
    {
        treeprint(p->left);		//왼쪽노드
        printf("word: %4s count : %d\n", p->word, p->count);				//현재 노드를 프린트함
        treeprint(p->right);	//오른쪽 노드
    }
}

int main(int argc, const char * argv[]) {
    struct tnode *root;
    char word[MAXWORD];
    
    root = NULL;
    while (getword(word, MAXWORD) != EOF)	//get word
        if (isalpha(word[0]))	//만약 첫번째 주소가 알파벳이면
            root = addtree(root, word);	//트리에 단어를 저장함 
    treeprint(root);	//트리를 중위순회로 출력하고 종료
    return 0;
}

~~~

단어가 입력되면 사전순으로 트리에 저장한다. 

BST알고리즘을 사용하여 알파벳이 작으면 오른쪽 노드에 크면 왼쪽노드에 저장한다.

중위순회를 사용하면 사전순으로 단어가 출력된다.