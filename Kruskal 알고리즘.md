Kruskal 알고리즘

~~~c
#include <stdio.h>
#include <stdlib.h>

#define TRUE 1
#define FALSE 0

#define MAX_VERTICES 100
#define INF 1000

int parent[MAX_VERTICES];		// 부모 노드 	찾는 배열 
	
	// 초기화
void set_init(int n)
{
	int i; 
	for ( i = 0; i<n; i++)  
		parent[i] = -1;			//인덱스를 -1로 초기화함 
}
// curr가 속하는 집합을 반환한다.
int set_find(int curr)
{
	if (parent[curr] == -1)				//-1이면 자신을 리턴 
		return curr; 			// 루트 
	while (parent[curr] != -1) curr = parent[curr];		//-1이 아니면 -1을 찾아서 리턴 
	return curr;
}

// 두개의 원소가 속한 집합을 합친다.
void set_union(int a, int b)
{
	int root1 = set_find(a);	// 노드 a의 루트를 찾는다. 
	int root2 = set_find(b);	// 노드 b의 루트를 찾는다. 
	if (root1 != root2) 	// 합한다. 
		parent[root1] = root2;	
}

struct Edge {			// 간선을 나타내는 구조체
	int start, end, weight;		//weight는 간선의 길이	
};

typedef struct GraphType {
	int n;	// 간선의 개수
	struct Edge edges[2 * MAX_VERTICES];
} GraphType;

// 그래프 초기화 
void graph_init(GraphType* g)
{
	int i;
	g->n = 0;
	for ( i = 0; i < 2 * MAX_VERTICES; i++) {
		g->edges[i].start = 0;
		g->edges[i].end = 0;
		g->edges[i].weight = INF;
	}
}
// 간선 삽입 연산
void insert_edge(GraphType* g, int start, int end, int w)
{
	g->edges[g->n].start = start;
	g->edges[g->n].end = end;
	g->edges[g->n].weight = w;
	g->n++;
}
// qsort()에 사용되는 함수
int compare(const void* a, const void* b)
{
	struct Edge* x = (struct Edge*)a;
	struct Edge* y = (struct Edge*)b;
	return (x->weight - y->weight);
}

// kruskal의 최소 비용 신장 트리 프로그램
void kruskal(GraphType *g)
{
	int edge_accepted = 0;	// 현재까지 선택된 간선의 수	
	int uset, vset;			// 정점 u와 정점 v의 집합 번호
	struct Edge e;

	set_init(g->n);				// 집합 초기화
	qsort(g->edges, g->n, sizeof(struct Edge), compare);
	//시간복잡도 nlogn 

	printf("크루스칼 최소 신장 트리 알고리즘 \n");
	int i = 0;
	
	//nlogn 정도의 시간복잡도 
	while (edge_accepted < (g->n - 1))	// 간선의 수 < (n-1)
	{
		e = g->edges[i];
		uset = set_find(e.start);		// 정점 u의 집합 번호 	logn
		vset = set_find(e.end);		// 정점 v의 집합 번호		logn
		if (uset != vset) {			// 서로 속한 집합이 다르면 		==사이클이 생기지 않는다면 
			printf("간선 (%d,%d) %d 선택\n", e.start, e.end, e.weight);
			edge_accepted++;
			set_union(uset, vset);	// 두개의 집합을 합친다.	
		}
		i++;
	}
}
int main(void)
{
	GraphType *g;
	g = (GraphType *)malloc(sizeof(GraphType));
	graph_init(g);

	insert_edge(g, 0, 1, 29);
	insert_edge(g, 1, 2, 16);
	insert_edge(g, 2, 3, 12);
	insert_edge(g, 3, 4, 22);
	insert_edge(g, 4, 5, 27);
	insert_edge(g, 5, 0, 10);
	insert_edge(g, 6, 1, 15);
	insert_edge(g, 7, 3, 18);
	insert_edge(g, 8, 4, 25);

	kruskal(g);
	free(g);
	return 0;
}
~~~



그래프를 모두 연결하는 **최소비용**을 구하는 알고리즘(Minimum Spanning Tree)이다

항상 최소비용을 선택하는 **greedy 알고리즘**이 사용된다. 

**Union set**을 활용하여 사이클이 생기는지 확인한다. 

서로 같은 루트를 가질 경우 사이클이 생긴다. 

(총 노드 개수 - 1)개의 간선을 선택하면 선택을 완료한다

