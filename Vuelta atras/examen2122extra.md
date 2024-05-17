# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [Problema 1 de la extraordinaria 21-22](#problema-1-de-la-extraordinaria-21-22)
* [Solución](#solución)
    * [Algoritmo principal](#algoritmo-principal)
  * [Salida de la solución](#salida-de-la-solución)
  * [Programa principal (opcional)](#programa-principal-opcional)
  * [Funciones auxiliares (opcional)](#funciones-auxiliares-opcional)
<!-- TOC -->


# Enunciado 

***

### Problema 1 de la extraordinaria 21-22

El juego de ***Tresminó*** es igual que el dominó, pero
las ``fichas son solo 10``: desde la blanca doble a la tres doble. Es decir:
- ``Blanca-Doble, Blanca-Uno, Blanca-Dos, Blanca-Tres, Uno-Doble,
Uno-Dos, Uno-Tres, Dos-Doble, Dos-Tres y Tres-Doble``

Implementa un algoritmo de Vuelta Atrás que imprima todas las posibles
partidas que empiecen con la ficha Tres-Doble y coloquen todas las fichas.

# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

### Algoritmo principal

```c
#define N 4
#define LIBRE -1
#define NULO -2

void va (int resto, imatriz2d mtx, int i, int j) {
    if (mtx[i][j] == LIBRE)             // El resto de casillas son NULO
        if (resto == 1) {
            mtx[i][j] = 1;              // Indico el 1 para mostrarlo a continuación
            mostrarMtx(mtx,N,N);        // Mostrar la solución por pantalla
            mtx[i][j] = LIBRE;          // Vuelvo a dejar libre dicha ficha
            ++formas;                   // Aumento el recuento
        } else {                        // Aquí se sabe que resto > 1
            mtx[i][j] = resto;          // Marco la ficha y ya no está libre
            if (i+1 < N)                va (resto-1, mtx, i+1,    j);       // Se puede mover
            if (j+1 < N)                va (resto-1, mtx, i,      j+1);     // a cualquier 
            if (i-1 >= 0)               va (resto-1, mtx, i-1,    j);       // casilla colindante
            if (j-1 >= 0)               va (resto-1, mtx, i,      j-1);
            if (i+1 < N && j+1 < N)     va (resto-1, mtx, i+1,    j+1);     // Mov. especial
            mtx[i][j] = LIBRE;          // Desmarco la ficha para que esté libre de nuevo         
        }
}

```

## Salida de la solución

**Nota**: orden descendiente desde la ficha 10 hasta la 1
**Nota**: ``DB`` es doble, ``BL`` es Blanca

```
        DB      3       2       1
3       10      -       -       -
2       9       4       -       -
1       8       5       3       -
BL      7       6       2       1

        DB      3       2       1
3       10      -       -       -
2       9       4       -       -
1       8       5       3       -
BL      7       6       1       2

... (más matrices)

        DB      3       2       1
3       10      -       -       -
2       8       9       -       -
1       6       7       2       -
BL      5       4       3       1

        DB      3       2       1
3       10      -       -       -
2       2       9       -       -
1       3       1       8       -
BL      4       5       6       7

Hay 17 formas de acabar el tresminó con 10 fichas
```

## Programa principal (opcional)

**NO HAY QUE IMPLEMENTAR EN EL EXAMEN ESTO DE AQUÍ**

```c
#define FICHAS 10

int main() {
    imatriz2d mtx = icreamatriz2d(N,N);

    for (int i = 0; i < N; ++i)
        for (int j = 0; j < N; ++j)
            mtx[i][j] = j <= i ? LIBRE : NULO;

    va(FICHAS,mtx,0,0);

    printf("\n\nHay %i formas de acabar el tresmino con %i fichas", formas, FICHAS);

    ifreematriz2d(&mtx);
    return 0;
}
```

## Funciones auxiliares (opcional)

**NO HAY QUE IMPLEMENTAR EN EL EXAMEN ESTO DE AQUÍ**

```c
void fichaI(int x){
    printf("\n");
    switch (x) {
        case 0: printf("3\t"); break;
        case 1: printf("2\t"); break;
        case 2: printf("1\t"); break;
        case 3: printf("BL\t"); break;
        default: break;
    }
}

void fichaJ(int x){
    switch (x) {
        case 0: printf("DB\t"); break;
        case 1: printf("3\t"); break;
        case 2: printf("2\t"); break;
        case 3: printf("1\t"); break;
        default: break;
    }
}

void mostrarMtx(imatriz2d mtx, int nf, int nc){
    printf("\n\n\t");
    for (int i = 0; i < nf; ++i)
        fichaJ(i);
    for (int i = 0; i < nf; ++i) {
        fichaI(i);
        for (int j = 0; j < nc; ++j)
            if (mtx[i][j] == NULO)  printf("-\t");
            else                    printf("%i\t",mtx[i][j]);   /// Se imprimen por columnas
    }
}
```

