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

Implementar el algoritmo de Dijstra.

# Solución

La solución también aparece en el [ejercicio 5 de la relación](./relacion5.md)

Aquí pondré solo la solución más simple

```c
#define N 7
#define INF 99999

int min (int a , int b) {
    return a < b ? a : b;
}

ivector dijstra(imatriz2d L) {
    int C[N-1];
    ivector D = icreavector(N-1);
    for (int i=0; i < N-1; ++i)     // Se repite desde nodo '2' hasta el 'N' => (de 0 a N-2)
        C[i] = 1,                   // Todos los nodos están por considerar
        D[i] = L[0][i+1];           // Distancia mínima inicial

    int i, j, k=0, m;
    for (i=0; i < N-1; ++i) {              // N-2 veces porque la distancia al último nodo no cambia

        for (j=0, m=INF; j < N - 1; ++j)   // Buscas la distancia mínima de los aún no considerados
            if (C[j] && D[j] < m)          // Metodo 1: busqueda lineal => El algortimo ya es O(n^2)
                m = D[j],
                k = j;

        C[k] = 0;                           // Das por considerado el nodo k

        for (j=0; j < N-1; ++j)
            if (C[j])                       // Actualizas la nueva distancia mínima de los no considerados
                D[j] = min(D[j], D[k] + L[k][j]);   // Grafo denso -> Compruebas con nodos adyacentes -> O(n^2)

    }

    return D;
}
```

### Salida de la solución

Se ha aplicado sobre el siguiente grafo:

![](./grafo.png)

```
        1       2       3       4       5       6       7
1:              1       -       4       -       -       -
2:                      2       6       4       -       -
3:                              -       5       5       -
4:                                      3       -       4
5:                                              8       7
6:                                                      3
7:

DIJSTRA
               (1       2       4       5       6       9)
```