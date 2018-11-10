# Solucionário das Questões Abordadas em Sala <h1>

* <h2>Uri 1610 Dudu Faz Serviço<h2>

~~~~C++
#include <bits/stdc++.h>

using namespace std;

vector<vector<int>> v;
vector<int> visited;
bool cicle;

void dfs(int n){
  visited[n] = 1;
  if (cicle) return;
  for (auto x: v[n]){
    if (visited[x] == 1){
      cicle = true;
      return;
    }
    else if (visited[x] == 0){
      dfs(x);
    }
  } 
  visited[n] = 2;
}

int main (){
  int N;
  int documentos, conexoes;
  int u,w;
  
  cin >> N;
  while(N--){
    cin >> documentos >> conexoes;
    v.assign(documentos, vector<int> ());
    visited.assign(documentos,0);
    
    for (int i = 0; i < conexoes; i++){
      cin >> u >> w;
      v[u - 1].push_back(w - 1);
    }
    cicle = false;
    for (int i = 0;i<documentos; i++){
      if (!visited[i])
        dfs(i);
      if (cicle) break;
    }
    if (cicle) cout << "SIM\n";
    else cout << "NAO\n";
    visited.clear();
    v.clear();
  }

  return 0;
}
~~~~

* <h2>Uva 336 A Node Too Far<h2>

~~~~C++
#include<bits/stdc++.h>

using namespace std;
bool visited[100];
vector<long long int> v[100];
long long int visitedcont=0;
int mindist[100];
void dfs(long long int n, long long int ttl){
	visited[n]=true;
	visitedcont++;
	ttl--;
	mindist[n]=ttl;

  if(!ttl) return;

	for(auto x:v[n]){
		if(!visited[x]){
      		dfs(x,ttl);
		}

    	else if(mindist[x]<ttl-1){
      		dfs(x,ttl);
      		visitedcont--;
    	}
	}

}

int main(){
	
	long long int caso=0;


	while(1){

		long long int N;
		cin >> N;
		if(!N) break;
		
		for(long long int i=0;i<100;i++){
			v[i].clear();
		}
		visitedcont=0;

		long long int a,b;
		set<long long int> v_count;
		map<long long int,int> transf;
		int h=1;

		while(N--){
			cin >> a >> b;
			
			h=v_count.size();
			v_count.insert(a);
			
			if(h!=v_count.size()){
				transf[a]=h;
			}


			h=v_count.size();
			v_count.insert(b);
			
			if(h!=v_count.size()){
				transf[b]=h;
			}
			v[transf[a]].push_back(transf[b]);
			v[transf[b]].push_back(transf[a]);
		}

		while(1){
			cin >> a >> b;
			if(a==0 && b==0) break;
			visitedcont=0;
    

      		memset(visited,0,sizeof visited);
      		if(v_count.count(a) != 0){
  		  	dfs(transf[a],b+1);
      		}
			
			printf("Case %d: %d nodes not reachable from node %d with TTL = %d.\n",++caso,v_count.size()-visitedcont,a,b);
		}


	}
	return 0;
}
~~~~
