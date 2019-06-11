BFS(Breadth First Search) - 인접 행렬로 구현

~~~c
#include <stdio.h>
#include <stdlib.h>
#define MAX_VERTICES 100
#define MAX_QUEUE_SIZE 10

typedef struct GraghType{
	//노드 안에 정보를 넣고싶다면 int nodes[];
	int n; 			//현재 vertex의 수 
	int adj_matrix[MAX_VERTICES][MAX_VERTICES]; 	
	
}GraghType;

typedef int element;

typedef struct QueueType{
	element queue[MAX_QUEUE_SIZE];
	int front,rear;
}QueueType;

void enqueue(QueueType *q,element i);
element dequeue(QueueType *q);

int visited[MAX_VERTICES];	//방문했는지 확인하기 위한 배열 

void init(GraghType *g);
void insert_vertex(GraghType *g,int v);
void insert_edge(GraghType *g, int start,int end);
void print_adj(GraghType *g);
 
void bfs_mat(GraghType *g,int v);

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
	
	print_adj(g);
	
	bfs_mat(g,0);
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
		return ;
	}
	g->n++;
}
void insert_edge(GraghType *g, int start,int end){
	if (start>=g->n || end>=g->n){
		printf(" 그런도시 없음\n");
		return;
	}
	g->adj_matrix[start][end]=1;
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


void enqueue(QueueType *q,element i){
	q->rear++;
	q->queue[q->rear]=i;
}
element dequeue(QueueType *q){
	q->front++;
	return q->queue[q->front];
}

void bfs_mat(GraghType *g,int v){
	
	QueueType *q;
	q=(QueueType*)malloc(sizeof(QueueType));	//큐생성
	
	int i=0;
    int j;
	q->front=0;		//큐 초기화
	q->rear=0;
	
	visited[v]=1;	//시작노드에 도장찍음
	printf("현재 노드는 %d\n",v);
	enqueue(q,v);		//현재 노드를 큐에 저장
	while(q->front!=q->rear){	
		v=dequeue(q);
        j++;
		for(i=0;i<g->n;i++){	
            //v노드에 인접한 모든 노드에 대해서 
			if((g->adj_matrix[v][i]==1) && ( visited[i]!=1) )
                //방문하지 않은 인접노드를 큐에 저장
            {
				printf("현재 노드는 %d\n",i);
                printf("시작 노드와의 거리 %d\n",j);
				visited[i]=1;
				enqueue(q,i);
			}
		}
	} 
}
~~~

BFS는 큐를 사용한 그래프 탐색 기법이다.

시작 노드를 큐에 저장하고 디큐 하면서 아직 방문하지 않은 인접노드를 큐에 저장한다. 따라서 첫번째로 디큐한 노드(시작노드)에서 큐에 저장하는 노드들은 시작노드로부터 1엣지만큼 떨어진 노드 들이다. 

노드를 방문할 때마다 방문하지 않은 다음 노드들은 큐에 저장한다.

BFS를 사용할 경우 현재 노드가 시작 노드로부터 얼마나 떨어졌는지 확인이 가능하다.