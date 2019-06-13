DFS(Depth First Search)  -인접행렬로 구현

~~~c
#include <stdio.h>
#include <stdlib.h>
#define MAX_VERTICES 100

typedef struct GraghType{
	//노드 안에 정보를 넣고싶다면 int nodes[];
	int n; 			//현재 vertex의 수 
	int adj_matrix[MAX_VERTICES][MAX_VERTICES]; 	
	
}GraghType;

int visited[MAX_VERTICES];	//dfs에서 한번 방문 했는지를 표시하기위한 배열 

void init(GraghType *g);
void insert_vertex(GraghType *g,int v);
void insert_edge(GraghType *g, int start,int end);
void print_adj(GraghType *g);

void dfs_mat(GraghType* g, int v); 

int main(void){
	GraghType * g;
	g= (GraghType*)malloc(sizeof(GraghType));
	
	init(g);
	
	insert_vertex(g,0);
	insert_vertex(g,1);
	insert_vertex(g,2);
	insert_vertex(g,3);
	insert_vertex(g,4);
	
	
	insert_edge(g,0,1);
	insert_edge(g,1,2);
	insert_edge(g,1,3);
	insert_edge(g,2,4);
	
	print_adj(g);	//인접 행렬의 모양
	
	dfs_mat(g,0);
}

void init(GraghType *g){
	
	int i=0;
	int j=0; 
	
	//vertex개수 초기화
	g->n=0;
	
	//adj_matrix 초기화
	for(i=0;i<MAX_VERTICES;i++){
		for(j=0;j<MAX_VERTICES;j++){
			g->adj_matrix[i][j]=0;
		}
	}
}
void insert_vertex(GraghType *g,int v){
	//아직 key가 없으므로 v는 버림 
	
	if(g->n==MAX_VERTICES){
		printf("삽입 불가\n"); 
		return ;
	}
	g->n++;		//vertex의 인덱스 증가 
}
void insert_edge(GraghType *g, int start,int end){
	if (start>=g->n || end>=g->n){
		printf(" 그런도시 없음\n");
		return;
	}
	g->adj_matrix[start][end]=1;	//간선을 연결함 
	g->adj_matrix[end][start]=1;
	
}
void print_adj(GraghType *g){
	int i=0;
	int j=0; 
	for(i=0;i<g->n;i++){
		for(j=0;j<g->n;j++){
			printf("%d ",g->adj_matrix[i][j]);
		}
		printf("\n");
	}
}

void dfs_mat(GraghType* g, int v){
	int i;
	
	if( v>=g->n){
		printf("그런 노드 없음\n");
		return;
	}
	
	//현재위치에 방문 도장 
	visited[v]=1;
	
	printf("현재 노드는 %d \n",v); 
	
	for(i=0;i<g->n;i++){
		if((g->adj_matrix[v][i]== 1)&&(visited[i]!=1) ){
			//방문하지 않은 인접노드 존재 
			dfs_mat(g,i); 
		}
	}
	return; //더이상 갈 길이 없음 
}

~~~

DFS는 스택을 이용한 그래프 탐색 방법이다. 

한 방향의 끝까지 탐색을 하고 갈림길으로 다시 돌아와 다시 탐색을 시작한다.

직접적으로 스택을 구현하지 않고 재귀를 통하여 묵시적인 스택을 사용한다.

dfs_mat 의 for문 안에서 방문하지 않은 인접노드가 존재하지 않을 때까지 재귀를 통해 탐색을 한다. 

더이상 갈 길이 없으면 i가 증가하여 다른 방향으로 탐색을 한다.

위의 예시에서 

0-> 1->2 하고 방문하지 않은 인접노드인 4까지 방문하면 더이상 방문하지않은 인접노드가 없다. 따라서 1의 시점까지 리턴하고 i를 1 증가하면 3번을 방문하게 된다.

인접 행렬로 구현된 dfs는 O(n^2)의 시간복잡도를 갖는다.