# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución base](#solución-base)
* [Solución más completa](#solución-más-completa)
<!-- TOC -->

# Enunciado

***

Un archipiélago está formado por varias islas. Existen una serie de puentes de dirección
única que unen ciertos pares de islas entre sí. Para cada puente se conoce su anchura, que
siempre es mayor que 0.

La anchura de un camino de varios puentes es la anchura mínima de todos los puentes que lo forman.

Diseñar un algoritmo basado en Programación Dinámica para saber, si existe, cuál
es el camino de anchura máxima entre todo par de islas.

# Solución base

***

[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
int min(int a, int b) { 
    return a < b ? a : b; 
}

int max(int a, int b){
    return a < b ? b : a; 
}

void anchuraMax (imatriz2d anchuras, imatriz2d A) {
    int i, j, k, temp;
    for (i=0; i < N; ++i)                            // Inicialización para k==0
        for (j=0; j < N; ++j)                        // No hay que pasar por puentes intermedios,
            A[i][j] = anchuras[i][j];                // por lo que en la etapa 0, de i a j se va directamente

    for (k=0; k < N; ++k)
        for (i=0; i < N; ++i)
            for (j=0; j < N; ++j) {
                temp = min(A[i][k], A[k][j]);        // La anchura de varios puentes es el MÍNIMO de los puentes que lo forman
                A[i][j] = max(A[i][j], temp);        // Se busca el camino de anchura máxima y se actualiza
            }

}
```

```c
#define N 6
#define INF 9999

int main() {
    imatriz2d anchuras = icreamatriz2d(N, N);
    imatriz2d A = icreamatriz2d(N, N);

    for (int i = 0; i < N; ++i)
        for (int j = 0; j < N; ++j) {
            anchuras[i][j] = ...    /* Cualquier anchura entre isla i y j */
            if (i==j) anchuras[i][j] = -INF;
        }

    int cantidadQuitar = ...        /* Cualquier cantidad de puentes a quitar */
    for (int i = 0; i < cantidadQuitar; ++i) {
        int f = /* Cualquier fila */,  c = /* Cualquier columna */
        if (f!=c)
            anchuras[f][c] = INF;   // ESTO INDICA QUE NO HAY PUENTE ENTRE LA ISLA f Y c
    }
    
    anchuraMax(anchuras, A, camino);
    mostrarMatriz(A);               // Mostrar resultado por pantalla
    
    ifreematriz2d(&anchuras);
    ifreematriz2d(&A);
    return 0;
}
```

# Solución más completa

***

[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
int min(int a, int b) { 
    return a < b ? a : b; 
}

void anchuraMax(imatriz2d anchuras, imatriz2d A, imatriz2d camino){
    int i, j, k, temp;
    for (i=0; i < N; ++i)                           // Inicialización para k==0
        for (j=0; j < N; ++j)                       // No hay que pasar por puentes intermedios,
            A[i][j] = anchuras[i][j];               // por lo que en la etapa 0, de i a j se va directamente

    for (k=0; k < N; ++k)
        for (i=0; i < N; ++i)
            for (j=0; j < N; ++j) {
                temp = min(A[i][k],A[k][j]);        // La anchura de varios puentes es el MÍNIMO de los puentes que lo forman
                if (temp > A[i][j]) {               // Se busca el camino de anchura máxima
                    A[i][j] = temp;                 // Se actualiza a dicha anchura
                    camino[i][j] = k+1;             // NUEVO: guardo la etapa para cada camino que se haya escogido
                }
            }
}
```

```c
#define N 6
#define INF 9999

int main() {
    imatriz2d anchuras = icreamatriz2d(N, N);
    imatriz2d A = icreamatriz2d(N, N);
    imatriz2d camino = icreamatriz2d(N, N);

    for (int i = 0; i < N; ++i)
        for (int j = 0; j < N; ++j) {
            anchuras[i][j] = ...    /* Cualquier anchura entre isla i y j */
            camino[i][j] = ...      /* Valor por defecto */
            if (i==j) anchuras[i][j] = -INF;
        }

    int cantidadQuitar = ...        /* Cualquier cantsoidad de puentes a quitar */
    for (int i = 0; i < cantidadQuitar; ++i) {
        int f = /* Cualquier fila */,  c = /* Cualquier columna */
        if (f!=c)
            anchuras[f][c] = INF;   // ESTO INDICA QUE NO HAY PUENTE ENTRE LA ISLA f Y c
    }
    
    mostrarMtx(anchuras);
    anchuraMax(anchuras, A, camino);
    mostrarMtx(A);
    mostrarMtx(camino);
    
    ifreematriz2d(&anchuras);
    ifreematriz2d(&camino);
    ifreematriz2d(&A);
    return 0;
}
```

