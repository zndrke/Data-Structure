재귀를 이용한 power 표현 두가지

1. 

x^n 에서 n+1번(0~n까지) 만큼 재귀가 이루어지는 코드

~~~c
#include <stdio.h>

int power(int x,int n){
	if (n<=0)
		return 1;
	else 
		return x*power(x,n-1);
}


int main(){
	int x,n;
	scanf("%d %d",&x,&n);
	printf("%d",power(x,n));
	return 0;
}
~~~





2. 

x^n에서  *log*2의n번 실행하는 코드

~~~c
#include <stdio.h>

int power(int x,int n){
	if (n<=0)
		return 1;
	
	if(n%2==0)
		return power(x*x,n/2);        //짝수일 경우 제곱을 n/2번 재귀
	else 
		return x*power(x*x,(n-1)/2);  // 홀수일 경우 x를 한번 곱해서 짝수로 만든다음
}                                     // 제곱을 (n-1)/2 번 재귀


int main(){
	int x,n;
	scanf("%d %d",&x,&n);
	printf("%d",power(x,n));
	return 0;
}
~~~



예를 들어 
2^8 = (2^2)^4으로 변환

2^7 = 2*(2^2)^3으로 변환

따라서 (*Log* 2의 n)+1번 재귀가 일어난다. 

