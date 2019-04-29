## Knapsack Bottom-Up

```C++

#include<bits/stdc++.h>
#define MAX_N 10002
using namespace std;

int memo[MAX_N][MAX_N];
int wt[MAX_N]; //peso do item
int val[MAX_N]; //valor do item


int knapsack(int n, int W){//n== Numero de itens W==Capacidade máxima
        for(int i=0; i <= n; i++){
            for(int j=0; j <= W; j++){
                if(i == 0 || j == 0){
                    memo[i][j] = 0;
                    continue;
                }
                if(j - wt[i-1] >= 0){
                    memo[i][j] = max(memo[i-1][j], memo[i-1][j-wt[i-1]] + val[i-1]);
                }else{
                    memo[i][j] = memo[i-1][j];
                }
            }
        }
    return memo[n][W];
}

int main(){

 memset(memo, -1, sizeof(memo));


	return 0;
}

```
