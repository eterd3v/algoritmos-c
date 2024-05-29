# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Algoritmo principal](#algoritmo-principal)
    * [Funciones auxilares para mostrar la solución](#funciones-auxilares-para-mostrar-la-solución)
    * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Enunciado

***

Un camionero conduce desde París a Jaén siguiendo una ruta dada y
llevando un camión que le permite, con el tanque de gasolina lleno, recorrer ``n``
kilómetros sin parar. 

El camionero dispone de un mapa de carreteras que le indica
las distancias entre las gasolineras que hay en su ruta. 

Como va con prisa, el camionero desea pararse a repostar el menor número de veces posible. 

Diseñar un algoritmo voraz para determinar en qué gasolineras tiene que parar y
demostrar que el algoritmo encuentra siempre la solución óptima

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

### Algoritmo principal

Es una primera parte del problema, que es conocer el camino mínimo entre París y Jaén

```c
#define N 7
#define L_MAX 9999
#define INF L_MAX

int minUltimo;                          // Guarda la distancia mínima del nodo origen al nodo N-1
int caminoUltimo;                       // Guarda el nodo previo para llegar al nodo N-1

void djistra(imatriz2d dist, ivector distMin, ivector precedente) {

    ivector sinConsiderar = icreavector(N - 1);
    for (int i = 0; i < N-1; ++i) {
        distMin[i]       = dist[0][i];
        sinConsiderar[i] = 1;            // Todos los vértices se consideran válidos al principio
        precedente[i]    = 0;            // Suponemos que el origen (0) ya conecta con distMin mínima al resto de nodos
    }

    int v, min;
    for (int i = 0; i < N-2; ++i) {      // Es N-2 porque distMin no varía en la última iteración al
                                         // quitar el último elemento (sinConsiderar[v]=0)
        min = L_MAX;
        for (int k = 0; k < N-1; ++k)
            if (sinConsiderar[k])        // Si el nodo k aún no se ha considerado ...
                if (min > distMin[k]){   // .. se comprueba que k minimice la distancia
                    min = distMin[k];
                    v = k;
                }

        sinConsiderar[v] = 0;            // Una vez tenemos k, lo marcamos como considerado (booleano)

        for (int w = 0; w < N-1; ++w)
            if (sinConsiderar[w])
                if (distMin[w] > distMin[v] + dist[v][w]){
                    distMin[w] = distMin[v] + dist[v][w];
                    precedente[w] = v;
                }
    }

    // El camino minimo al último nodo (el N-1) no está en distMin => hay que calcularlo
    minUltimo    = dist[0][N - 1];                       // Inicializado como camino directo a N-1 ..
    caminoUltimo = 0;                                    // .. desde el origen (0)

    for (int i = 0; i < N - 1; ++i)
        if (minUltimo > distMin[i] + dist[i][N - 1]) {   // Al último se llega: desde 0 (directo) o desde el resto de nodos (distMin[i])
            minUltimo = distMin[i] + dist[i][N - 1];     // Buscamos el más cercano (mínimo) al último nodo
            caminoUltimo = i;
        }

    ifreevector(&sinConsiderar);
}
```

### Funciones auxilares para mostrar la solución

````c
int rastro(imatriz2d mtx, int v, int w, int distancia, int final) {
    distancia -= mtx[v][w];
    if (distancia == 0 && v == 0){                                      // Inicio del recorrido (es origen y distancia es 0)
        printf(w == final ? "0, %i" : "0, ", w);                        // Puede que sea directo (el nodo final es el destino)
        return 1;
    } else if (distancia > 0)                                           // Hay recursividad mientras la distancia recorrida es positiva
        for (int i = 0; i < N; ++i)                                     // Partimos de la arista [v,w] ( que son [origen,destino] )
            if (rastro(mtx, i, v, distancia, final)){                   // Buscamos  una  arista [i,v] ( en algún momento se tomo v como destino )
                printf(w == final ? "%i, %i" : "%i, ", v , w);
                return 1;
            }
    return 0;
}

void seguirCaminos(imatriz2d L, ivector P, ivector D) {
    printf("\n\n===\t===\t CAMINO A CADA NODO \t===\t===\t===");
    for (int i = 0; i < N; ++i) {
        printf("\n0->%i:\t",i);
        if (i != N-1)   rastro(L,P[i],i,D[i],i);                        // Imprimimos el rastro de nodos
        else            rastro(L, caminoUltimo, N-1, minUltimo, N-1);   // Una vez impreso el rastro que llega hasta el anterior a N-1
    }
}
````

### Salida de la solución

```
        0       1       2       3       4       5       6
0:      -       45      85      69      22      29      100
1:      61      -       81      61      87      37      25
2:      33      1       -       61      57      12      9
3:      26      21      55      -       38      34      49
4:      1       41      49      2       -       1       79
5:      25      51      25      76      25      -       77
6:      11      25      37      17      83      49      -

===     ===      CAMINO A CADA NODO     ===     ===     ===
0->0:   0, 4, 0
0->1:   0, 1
0->2:   0, 4, 5, 2
0->3:   0, 4, 3
0->4:   0, 4
0->5:   0, 4, 5
0->6:   0, 4, 5, 2, 6

Elige el nodo donde estara Jaen: numero entero entre 0 y 6 incluidos
2

Elige el repostaje disponible: numero entero mayor que 0
60

Sobra 12 de repostaje para ir desde 0 (Paris) hasta 2 (Jaen)
[0,2]=48km      0, 4, 5, 2
```