# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Salida de la solución](#salida-de-la-solución)
    * [Funciones auxiliares](#funciones-auxiliares)
    * [Programa principal](#programa-principal)
<!-- TOC -->

# Enunciado

***

Un laberinto se puede representar mediante una matriz bidimensional en la que
cada casilla puede estar marcada mediante un 0, un 1, un 2, un 3 ó un 4: 0 para
indicar que la casilla está libre, 1 para indicar que la casilla es un obstáculo, 2
para indicar que ya se ha pasado por esa casilla, 3 para indicar la entrada al
laberinto y 4 para indicar que la casilla representa la salida.

Diseñar un algoritmo basado en la técnica de Vuelta Atrás que, dada una matriz
que representa un laberinto, devuelva **el recorrido más corto** (camino con
menos casillas) desde la entrada hasta la salida.

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
#define N 10
#define LIBRE 0
#define OBSTA 1
#define VISIT 2
#define ENTER 3
#define EXIT  4

void va(imatriz2d lab, int x, int y, int* min, int pasos, imatriz2d sol){
    if (lab[x][y] == EXIT) {
        if (pasos < *min) {                     // Actualiza los mínimos y sobreescribe la solución
            *min = pasos;
            for (int i = 1; i < N-1; ++i)
                for (int j = 1; j < N-1; ++j)
                    if (lab[i][j] != OBSTA)
                        sol[i][j] = lab[i][j];
        }
    } else {
        if (lab[x][y] != OBSTA && lab[x][y] != VISIT) {
            lab[x][y] = VISIT;
            va(lab,x+1,y,min,pasos+1,sol);
            va(lab,x-1,y,min,pasos+1,sol);
            va(lab,x,y+1,min,pasos+1,sol);
            va(lab,x,y-1,min,pasos+1,sol);
            lab[x][y] = LIBRE;
        }
    }
}
```

### Salida de la solución

```
ORIGINAL:       ======  ======
1:      M M M M M M M M M M
2:      M       e         M
3:      M   M M M M M M   M
4:      M       M         M
5:      M           M M M M
6:      M             M x M
7:      M     M M M M     M
8:      M   M       M   M M
9:      M       M       M M
10:     M M M M M M M M M M

RESUELTO: 22 pasos son los necesarios para llegar a la salida
1:      M M M M M M M M M M
2:      M - - - -         M
3:      M - M M M M M M   M
4:      M -     M         M
5:      M -         M M M M
6:      M -           M x M
7:      M -   M M M M - - M
8:      M - M - - - M - M M
9:      M - - - M - - - M M
10:     M M M M M M M M M M
```

### Funciones auxiliares

````c
void mostrarLab(imatriz2d mtx, int nf, int nc) {
    for (int i = 0; i < nf; ++i) {
        printf("%i:\t",i+1);
        for (int j = 0; j < nc; ++j){
            char pantalla = '?';
            switch (mtx[i][j]) {
                case LIBRE: pantalla = ' '; break;
                case OBSTA: pantalla = 'M'; break;
                case VISIT: pantalla = '-'; break;
                case ENTER: pantalla = 'e'; break;
                case EXIT:  pantalla = 'x'; break;
            }
            printf(pantalla == '?' ? "%i " : "%c ", pantalla == '?' ? mtx[i][j] : pantalla);   /// Se imprimen por columnas
        }
        printf("\n");
    }
}
````

### Programa principal

````c
#define SEMILLA 14

int main() {
    srand(SEMILLA);
    imatriz2d mtx = icreamatriz2d(N,N);
    int i,j,k;

    for (i = 0; i < N; ++i) {
        mtx[0][i] = mtx[N-1][i] = OBSTA; // Filas
        mtx[i][0] = mtx[i][N-1] = OBSTA; // Columnas
    }
    for (i = 1; i < N-1; ++i)
        for ( j = 1; j < N - 1; ++j)
            mtx[i][j] = LIBRE;

    for (k = 1; k < N - 1; ++k)
        for ( i = 1 + (rand()%N-2); i < N-1; ++i) {
            mtx[k][i] = OBSTA;
            if (rand()%2 == 0)
                break;
        }

    mtx[5][8] = EXIT;
    mtx[1][4] = ENTER;
    mtx[7][7] = mtx[1][3] = LIBRE;
    mtx[2][7] = mtx[7][2] = mtx[8][4] = OBSTA;

    imatriz2d sol = icreamatriz2d(N,N);
    for (i = 0; i < N; ++i)                     // Copio solo los obstáculos finales
        for (j = 0; j < N; ++j)
            if (mtx[i][j] == OBSTA)
                sol[i][j] = OBSTA;

    int* min = malloc(sizeof (int));            // Preparo el mínimo para las distintas llamadas
    *min = (N-1)*(N-1);
    
    printf("\nORIGINAL:\t======\t======\n");
    mostrarLab(mtx,N,N);

    va(mtx, 1, 4, min, 0, sol);

    printf("\nRESUELTO: %i pasos son necesarios para llegar a la salida\n", *min);
    mostrarLab(sol,N,N);

    free(min);
    ifreematriz2d(&mtx);
    ifreematriz2d(&sol);
    return 0;
````