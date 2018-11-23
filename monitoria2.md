# Monitoria PPC 

## *Code Forces 455 A

  <p>Ao escolher um número para retirar e ganhar seus pontos, a melhor sequencia será então remover todas as cópias dele,
    logo os pontos ganhos por escolher retirar um número são A quantidade de vezes que ele aparece(m[st])*o valor dele(st), 
    para evitar chamadas bidirecionais ak+1 e ak-1 começa se a dp removendo o maior núumero existente na sequencia, dessa forma
    a melhor solução pode ser descrita como a máxima entre pegar todas as cópias de um número e prosseguir excluindo seu vizinho
    menor{m[st]*st + dp(st-2)}, ou prosseguir supondo que o número que virá a seguir será solução melhor do que o número 
    analisado{dp(st-1)}<p>
    
~~~ c++
#include<bits/stdc++.h>

using namespace std;

using ll = long long;
using vi = vector<ll>;
using ii = pair<ll, ll>;

const int Maxn = 1e5 + 3;
ll n;
ll m[Maxn]; // histograma, maraca a frequência de cada número
ll ans[Maxn]; // respostas parciais
bool aux[Maxn]; // quais foram calculados


ll dp(ll st){
    if(st<=0) return 0; // Caso base, se não há copias desse número, ele não pode ser pego
    if(aux[st]) return ans[st];//caso o estado ja tenha sido calculado, apenas se retorna seu valor

    aux[st] = true;//marca os estado como calculado

    return ans[st] = max(dp(st-1), m[st]*st + dp(st-2));
}

int main(){
    ios::sync_with_stdio(false);
    cin >> n;
    ll temp;
    for(ll i=0; i<n; i++){
        cin >> temp;
        m[temp]++;
    }

    cout << dp(Maxn-1) << endl;

    return 0;
}
    
~~~~

## * Code Forces 782 B

  <p>
  Para resolver a questão  proposta, suponha que haja uma função f(t) que dado t retorne verdadeiro caso seja possível encontrar
  todos os pontos no mesmo lugar, e falso caso contrário, usando então a ideia de busca binária procura se o ponto aonde essa função
  muda de verdadeiro para falso.
  <p>
  <p>
  No intuito de escrever então a f(t) uma ideia interessante é desenhar em um os conjuntos nos cais cada ponto pode alcançar com um 
  determinado t por exemplo 1, nota se que caso a intersecção de todos os exista, então eles se encontram assim a f(t) precisa apenas testar 
  a existencia das intersecções
  <p>
    
~~~ c++

#include<bits/stdc++.h>
using namespace std;
vector<long double> x;
vector<long double> v;

bool f(long double t){
	double R,L;
		L=x[0]+v[0]*t;
		R=x[0]-v[0]*t;

	for(int i=1;i<x.size();i++){
		if((x[i]-v[i]*t) < L) L=x[i]-v[i]*t;
		if((x[i]+v[i]*t) > R) R=x[i]+v[i]*t;
	}
	if(R<=L) return true;
	else return false;
}

int main(){
	int n;
	long double aux=0;
  cin >> n;
	for(int i=0;i<n;i++){
		scanf("%llf",&aux);
		x.push_back(aux);
	}

	for(int i=0;i<n;i++){
		scanf("%llf",&aux);
		v.push_back(aux);
	}

	long double end=1000000005.0;
	long double begin=0.0;

	for(int i=0;i<10000;i++){
		long double on_try=(begin+end)/2.0;
		if(f(on_try) == true) end=on_try;
		else begin=on_try;
	}

	printf("%.12llf\n",((end+begin)/2.0));

	return 0;
}
~~~~
    
    
    
    
