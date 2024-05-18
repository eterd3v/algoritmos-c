
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

[El problema del caballo](https://es.wikipedia.org/wiki/Problema_del_caballo) es un antiguo problema matemático en el que se pide
que, teniendo una cuadrícula de `N x N` casillas y un caballo de ajedrez colocado 
en una posición `(X, Y)`, el caballo pase por todas las casillas y una sola vez.

Realmente este problema tiene [33.439.123.484.294 soluciones](https://soymatematicas.com/problema-del-caballo/)
en un tablero 8x8.

Dar para un tablero `6 x 6` o `7 x 7` una única solución (de todas las posibles) en cualquier coordenada.

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
#define N 7 // TAMAÑO DEL TABLERO. UN 8x8 YA OCUPA TODA LA PILA DE LLAMADAS
#define LIBRE -1
#define X 3
#define Y 5

int encontrado = 0;

/**
 * Encuentra una sola solución al problema de los caballos (hay demasiadas soluciones)
 */
void saltoCaballos(imatriz2d mtx, int x, int y, int resto){
    if (mtx[x][y] == LIBRE && !encontrado){
        mtx[x][y] = resto;
        if (resto == 1) {
            mostrarMtx(mtx,N,N);
            ++encontrado;
        } else {
            // Verticales
            if (x+1 <  N && y + 2 <  N) saltoCaballos(mtx, x + 1, y + 2, resto - 1);
            if (x-1 >= 0 && y + 2 <  N) saltoCaballos(mtx, x - 1, y + 2, resto - 1);
            if (x+1 <  N && y - 2 >= 0) saltoCaballos(mtx, x + 1, y - 2, resto - 1);
            if (x-1 >= 0 && y - 2 >= 0) saltoCaballos(mtx, x - 1, y - 2, resto - 1);
            // Horizontales
            if (y+1 <  N && x + 2 <  N) saltoCaballos(mtx, x + 2, y + 1, resto - 1);
            if (y-1 >= 0 && x + 2 <  N) saltoCaballos(mtx, x + 2, y - 1, resto - 1);
            if (y+1 <  N && x - 2 >= 0) saltoCaballos(mtx, x - 2, y + 1, resto - 1);
            if (y-1 >= 0 && x - 2 >= 0) saltoCaballos(mtx, x - 2, y - 1, resto - 1);
        }
        mtx[x][y] = LIBRE;
    }
}
```

### Salida de la solución

```
        1       2       3       4       5       6       7
1:      21      24      5       12      7       26      29
2:      4       13      22      25      28      11      8
3:      23      20      15      6       9       30      27
4:      14      3       44      31      16      49      10
5:      19      38      17      48      43      32      35
6:      2       45      40      37      34      47      42
7:      39      18      1       46      41      36      33

Se puede pasar por las celdas con caballos en tablero de 7x7 empezando en x:3 y:5
```