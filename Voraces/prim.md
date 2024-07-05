# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Enunciado

***

Implementar el algoritmo de Prim.

# Solución

La solución es muy similar a [Dijstra](./dijstra.md)

```c
#define N 7
#define INF 99999

imatriz2d prim(imatriz2d L) {               // Matriz es nº de nodos x nº de nodos
    ivector minDist = icreavector(N-1), masCerca = icreavector(N - 1);
    imatriz2d T = icreamatriz2d(2,N-1);
    for (int i = 0; i < N-1; ++i)           // Para los nodos desde '2' hasta el 'N-1' ..
        masCerca[i] = 0,                    // .. suponemos que el nodo más cercano es el '1'
        minDist[i] = L[0][i+1];             // Distancia mínima inicial: las del nodo '1'

    int min, j, k;
    for (int i = 0; i < N-1; ++i) {         // Repetir N-1 veces, la distancia al último nodo no cambia

        min = INF;
        for (j = 0; j < N - 1; ++j)         // Se busca la distancia mínima de cada nodo => O(n^2) con matriz de adyacencias
            if (0 <= minDist[j] && minDist[j] < min )
                min = minDist[j],
                k = j;

        T[0][i] = masCerca[k];              // Añadimos a T la arista
        T[1][i] = k+1;                      // +1 porque el nodo que se toma nunca es el nodo '1'
        minDist[k] = -1;                    // Indicamos la distancia mínima como usada => inválida

        for (j=0; j < N-1; ++j)
            if (L[k+1][j+1] < minDist[j])   // Actualizamos las distancias mínimas por aquellas que pasan por k
                minDist[j] = L[k+1][j+1],   // La distancia es +1, +1 ya que ni k ni j deben considerar el nodo 0 y ..
                masCerca[j] = k+1;          // .. deben llegar al nodo N-1 de la matriz

    }

    return T;
}
```

### Salida de la solución

Se ha aplicado sobre el siguiente grafo:

![](./grafo.png)

```
PRIM     1       2       3       4       5       6       7
1:              [1]     -       [4]     -       -       -
2:                      [2]     6       4       -       -
3:                              -       5       5       -
4:                                      [3]     -       [4]
5:                                              8       7
6:                                                      [3]
7:

Aristas del AGM
(1,2) = 1
(2,3) = 2
(1,4) = 4
(4,5) = 3
(4,7) = 4
(7,6) = 3
```