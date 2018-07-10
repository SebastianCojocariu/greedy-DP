Sebastian Cojocariu 321CB UPB ACS						                        
                                    
                                    TEMA 1
						                         -PA-

1)frati.cpp(greedy)
	Se observa imediat ca alegerea unui concurs de catre unul dintre 
frati contribuie nu doar la cresterea numarului de obiecte preferate,dar
si posibilitatea celuilalt de a-si creste numarul de obiecte preferate.
Asadar,alegerea unui concurs creeaza o discrepanta de fix suma dintre nr 
de jocuri si nr de benzi desenate a concursului ales.
Algoritm:
	Vom sorta vectorul crescator dupa suma dintre nr de jocuri si nr 
de benzi desenate al fiecarui concurs,iar in caz de egalitate se va alege 
concursul cu de nr_benzi_desenate mai mare.
	La fiecare pas,jon va lua de la capatul vectorului ultimul concurs
ramas disponibil (neales de niciunul dintre frati),iar sam va lua acel
concurs cu suma componentelor egala cu cu ultimul concurs ramas disponibil
dupa alegerea lui jon,alegandu-l pe cel cu nr_benzi_desenate mai mare.

Complexitate temporala : sortarea(n*log(n) + parcurgerea(2*n) ->O(nlog(n))
Complexitate spatiala : O(n)
	
2)ursi.cpp(DP)
	Notam cu dp[i][j] = nr de posibilitati de a obtine fete zambitoare 
pentru cuvantul de pe primele i+1 pozitii (adica de la 0 la i inclusiv),
avand exact j fete neacoperite (de tipul ^ sau ^_  _ _ ,etc)
Obtinem relatiile :
	dp[n][nr_ochi+1],unde n = strlen(cuvant).
Obs:Daca nr_ochi e impar sau cuvantul incepe cu '_' returnam 0;
Caz initial: dp[0][j] = 0 pentru orice j !=1, si 1 altfel.
	Daca v[i] = '_',atunci dp[i][j] = j*dp[i-1][j](putem adauga la 
fiecare fata  neacoperita din cele j ale lui dp[i-1][j] acel '_' 
reprezentat de v[i]).
	Daca v[i] = '^' atunci dp[i][j] = (j+1)dp[i-1][j+1] + dp[i-1][j-1]
(unde trebuie avut grija la indici pentru a nu iesi din matrice.In cazul 
in care s-ar iesi,se va pune 0.De ex in loc de dp[n][j+1] se va pune 0).

Observatie: Se observa ca dependenta se rezuma la linia anterioara.Astfel,
in loc de alocare unei matrici vom folosi doar 2 vectori:unul pentru linia 
anterioara din matrice si unul pentru linia curenta.La fiecare completare a 
linii curente actualizam linia anterioara.  

Complexitate temporala : Big O(n*(nr_ochi+1)) (deoarece se poate termina 
si n O(1) cand avem un nr impar de ochi sau incepe cu '_')  
Complexitate spatiala : O(nr_ochi+1).		

3)planificare.cpp(DP)
	Notam cu :
		a[i] = costul minim pentru elementele 0,...,i elemente 
din vector,in care se scade si ce ramane la sfarsit 
		b[i] = costul minim pentru elementele 0,...,i elemente 
din vector,in care nu scade si ce ramane la sfarsit
		nr_a[i] nr de concursuri pentru a realiza a[i]
		nr_b[i] nr de concursuri pentru a realiza b[i]  
Recurenta: La pasul i:
	Calculam acel indice maximal j pana la care am putea forma un concurs
cu ultimele j,j+1,...,i durate.
	Notam cu concurs(m,n) = durata pentru a realiza un concurs intre indicii 
m si n din vectorul de durate,luandu-se in calcul pauza dintre oricare 2.
	a[i] = min((durata_concurs - concurs(k,i))^3 + a[k-1],a[i]),unde k=j,i;
	b[i] = min(a[k-1],b[i]),k=j,i
(ideea consta in a analiza cu care dintre ultimele concursuri putem grupa 
ultimul concurs)
	nr_a[i] = 1+ nr_a[k],unde k e acel indice pentru care se realizeaza 
minimul pentru a[i]
	nr_b[i] = 1+ nr_b[k],unde k e acel indice pentru care se realizeaza 
minimul pentru b[i]

Complexitate temporala: Big O (n*n) deoarece la fiecare pas trebuie gasit indicele
pana la care putem forma un concurs valid,iar in cazul in care durata_concurs e
suficient de mare cat sa cuprinda toate durate intr-un sg concurs,vom face n^2/2 
operatii.
Complexitate spatiala : O(n) 
	
4)numaratoare.cpp(DP)
	dp[i][j][k] = in cate moduri pot scrie pe i ca suma de k termeni mai 
mici sau egali cu j,in care ordinea nu conteaza(1+2 si 2+1 sunt considerate 
aceeasi posibilitate)
	dp[sum][j][k] = dp[sum][j-1][k] (nu iau niciun j in scriere) + 
dp[sum-j][j][k-1] (iau un j in scriere).
La sfarsit,pentru a obtine scrierea corecta procedam astfel:
Observam imediat ca dp[sum][n][k] = dp[sum-n][n][k-1]+dp[sum-(n-1)][n-1][k-1]+
...+dp[sum-1][1][k-1]
Tot ce mai trebuie sa facem este sa gasim astfel "intervalul" in care s-ar 
incadra pozitia,lucru vizibil din cod.

Complexitate temporala: O((sum+1)*(n+1)*(k+1)) + O(n*k)(construirea vectorului 
rezultat) => O((sum+1)*(n+1)*(k+1))
Complexitae spatiala : O((sum+1)*(n+1)*(k+1)) + O(k)(vectorul rezultat)
=>O((sum+1)*(n+1)*(k+1))
