# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [_Problema 2 de la ordinaria 15-16_](#problema-2-de-la-ordinaria-15-16)
* [Solución](#solución)
    * [Salida de la solución (1)](#salida-de-la-solución-1)
    * [Salida de la solución (2)](#salida-de-la-solución-2)
<!-- TOC -->


# Enunciado 

***

### _Problema 2 de la ordinaria 15-16_

Diseña una función Voraz que dado un grafo no dirigido devuelva si es o no conexo.

# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

```c
int gConexo(imatriz2d ady){                                 // O(n^2)
    int resultado = 1, nodoConexo, i, j;
    for (j=1; j < N; ++j) {                                 // Miro la parte superior de la matriz (no dirigido)
        nodoConexo = 0;                                     // y solo de nodos diferentes
        for (i=0; i < j; ++i)                               // Es conexo si hay valores válidos en las columnas
            if (ady[i][j] != INF && ady[i][j] != -INF) {    // Solo aristas de peso válidos
                nodoConexo=1;
                break;                                      // Con comprobar una arista válida es suficiente
            }
        resultado *= nodoConexo;                            // Operación AND
    }
    return resultado;
}
```

### Salida de la solución (1)

```
1:              12      10      INF     INF     INF
2:                      INF     INF     INF     INF
3:                              INF     7       INF
4:                                      11      INF
5:                                              1
6:

El grafo no dirigido NO es conexo
```

### Salida de la solución (2)

```
1:              12      10      INF     INF     INF
2:                      INF     4       INF     INF
3:                              INF     7       INF
4:                                      11      INF
5:                                              1
6:

El grafo no dirigido ES conexo
```