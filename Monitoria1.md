# Solucionário das Questões Abordadas em Sala <h1>

* ## Uri 1610 Dudu Faz Serviço

&nbsp;&nbsp;&nbsp; A ideia por trás da questão indicada é de encontrar ciclos em um grafo, já que uma dependencia cíclica dos documentos geraria um serviço infinito para Dudu resultanto em uma impossibilidade de terminar seu serviço.
	
&nbsp;&nbsp;&nbsp; Usando um algoritmo de travessia(bfs/dfs) é possível identificar um ciclo, se após o início de uma travessia,for possível visitar uma das arestas já visitadas durante essa travessia(ou seja revisitar uma aresta visitada em uma travessia anterior não configura um ciclo). Vale ressaltar que como o grafo pode ter mais de um componente(não ser completamente conectado), é necessário considerar todos os componentes já que algum deles pode ter uma travessia cíclica.

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

* ## Uva 336 A Node Too Far
	
<p>&nbsp;&nbsp;&nbsp; Essa questão demanda que dado um ttl máximo o programa determine a quantidade de nós inatingíveis, ou seja, quantas arestas tem distância maior do que o dado ttl de uma aresta X tanto o bfs que pode ser facilmente adaptado para achar os menores caminhos e então contar quantos são maiores do que o ttl quanto um dfs que faça todo esse trabalho podem ser usados.</p>
	
<p>&nbsp;&nbsp;&nbsp; Porém a questão implica algumas dificuldades, como por exemplo, sabem se que serão no máximo 30 nós, porem esses nós podem ter valores muito grandes, o que não permite que um nó 'Ni' seja salvo em um vetor, na forma v[Ni], um segundo problema é desconhecer, a princípio, a quantidade de arestas. Ambos os problemas podem ser solucionados ao mesmo tempo, usando uma combinação de um map e um set, na qual o set permitira saber quantas arestas realmente existem, e com o auxílio de um "mapa de transferência" é possível colocar todos os números em valores que possam ser salvos no formato v[trans[Ni]] já que transf[Ni] sera um número entre 0 e a quantidade de nós.</p>

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
