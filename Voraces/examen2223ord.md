# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [Problema 1 de la ordinaria 22-23](#problema-1-de-la-ordinaria-22-23)
* [Solución](#solución)
  * [Algoritmo del problema](#algoritmo-del-problema)
    * [Algoritmo principal](#algoritmo-principal)
    * [Algoritmos auxiliares](#algoritmos-auxiliares)
  * [Salida de la solución](#salida-de-la-solución)
  * [Programa principal (opcional)](#programa-principal-opcional)
  * [Funciones auxiliares (opcional)](#funciones-auxiliares-opcional)
<!-- TOC -->


# Enunciado 

***

### _Problema 1 de la ordinaria 22-23_

Hay una epidemia de fiebre aftosa, enfermedad que se transmite por contacto
directo o indirecto con animales enfermos y es altamente contagiosa. 

Para controlar la infección, las autoridades sanitarias han recabado información de ``m`` granjas, concretamente las ``n`` compras de animales que han tenido lugar entre las distintas granjas
y la fecha (el número de semana) en la que tuvo lugar cada una de ellas.

Sabiendo que para declarar en cuarentena una granja hay que tener en cuenta que una granja es
susceptible de padecer la enfermedad si ha comprado animales de una granja infectada, 
o de una granja susceptible de tener la enfermedad después de que haya podido ser infectada.

Implementar un algoritmo voraz con un orden de complejidad no mayor a ``n^2`` que
determine qué granjas deben ser puestas en cuarentena a partir de los datos aportados
por las autoridades sanitarias.

Ejemplo:

``Granjas = {1, 2, 3, 4, 5, 6};`` 
``Infectadas = {1};`` 
``Cuarentena = {1, 2, 5, 6}``

|  |   |   |    |   |   |   |    |   |   |   |
|---|---|---|----|---|---|---|----|---|---|---|
| Granja que vende  | 5 | 4 | 2  | 3 | 6 | 5 | 2  | 6 | 1 | 3 |
| Granja que compra  | 2 | 1 | 1  | 4 | 4 | 3 | 6  | 5 | 5 | 2 |
| Fecha (nº de semana) | 6 | 1 | 10 | 3 | 4 | 2 | 11 | 8 | 5 | 9 |


# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

Sea ``n`` el número de operaciones de compra-venta y ``m`` el número de granjas. Se puede trabajar con
una tabla (matriz) de dimensión (3 x n), como la que se da en el ejemplo.
- Se ordena la tabla ascendentemente por fecha. ``O(n log n)``.
- Usando conjuntos disjuntos, se crea uno para cada una de las granjas. ``O(m)``
- Se recorre la tabla y se van uniendo conjuntos cuando en una operación de compra-venta el vendedor
sea un infectado o esté en cuarentena. 
  - La complejidad de esta operación es parecida a la del
  algoritmo de Kruskal.
- Por tanto, la complejidad final del algoritmo es de ``O(n log n)``.

## Algoritmo del problema

### Algoritmo principal

```c
#define N 10    // NÚMERO DE COMPRA-VENTA
#define M 6     // NÚMERO DE GRANJAS

#define FILAS 3
#define NULO -1

/** Granja G es susceptible de enfermedades si ha comprado:
 *      a) de una granja infectada
 *      b) de una granja susceptible de tener enfermedad después de que haya podido ser infectada
 */
ivector aftosa(ivector infectadas, imatriz2d compraVenta) {

  ivector cuarentena = icreavector(M);
  for (int i=0; i < M; ++i)
    cuarentena[i] = infectadas[i];
  
  quickSortMtx(compraVenta,2,0,N-1); // Ordenamos por las fechas de compra toda la información en O(n log n)
  
  for (int i = 0; i < N; ++i)
    if (cuarentena[compraVenta[0][i]] != NULO) // Ha vendido una granja pos. infectada
        cuarentena[compraVenta[1][i]] = compraVenta[1][i];

  return cuarentena;
  
}
```

### Algoritmos auxiliares

```c
void swapMtx(imatriz2d mtx, int fila, int i, int j) {
    int temp = mtx[fila][i];
    mtx[fila][i] = mtx[fila][j];
    mtx[fila][j] = temp;
}

void clasicoMtx(imatriz2d mtx, int fila, int i, int f) {
    for (;i <= f;++i) 
        for (int j = i+1; j <= f; ++j)
            if (mtx[fila][i] > mtx[fila][j])    // Se ordena la matriz según una fila objetivo
                for (int k = 0; k < FILAS; ++k) // Como FILAS siempre va a ser 3 en este problema, no altera el orden
                    swapMtx(mtx, k, i, j);
}

int encuentraPivoteMtx(imatriz2d mtx, int fila, int i, int f) { // Encuentra el mayor de los 2 primeros valores diferentes
    for (int k = i+1; k <= f; ++k)
        if (mtx[fila][i] != mtx[fila][k])
            return mtx[fila][i] > mtx[fila][k] ? i : k;
    return NULO;
}

int reordenaMtx(imatriz2d mtx, int fila, int i, int f, int pivote) {
    int k;
    do {
        for (k = 0; k < FILAS; ++k)     swapMtx(mtx,k,i,f);
        while (mtx[fila][i] < pivote)   ++i;
        while (mtx[fila][f] >= pivote)  --f;
    } while (i <= f);
    return i;
}

void quickSortMtx(imatriz2d mtx, int fila, int i, int f) {
    if (f-i+1 <= 2)
        clasicoMtx(mtx,fila,i,f);
    else {
        int k = encuentraPivoteMtx(mtx, fila, i, f);
        if (k != NULO) {
            k = reordenaMtx(mtx, fila, i, f, mtx[fila][k]);
            quickSortMtx(mtx,fila,i,k-1);
            quickSortMtx(mtx,fila,k,f);
        }
    }
}
```

### Salida de la solución

```
         INFORMACION DE LA COMPRA-VENTA
1:      2       4       1       5       2       4       1       1       2       3
2:      6       3       5       1       1       2       5       2       6       2
3:      2       4       9       6       8       1       3       5       7       10

        COMPRA-VENTA ORDENADA
1:      4       2       1       4       1       5       2       2       1       3
2:      2       6       5       3       2       1       6       1       5       2
3:      1       2       3       4       5       6       7       8       9       10

        INFECTADOS
(1      -       -       -       -       -)

        SOLUCION: EN CUARENTENA
(1      2       -       -       5       6)
```

### Programa principal (opcional)

**NOTA**: No hay que implementar en el examen esto de aquí.

```c
#define SEMILLA 29
#define MAX_FECHA 11

int main() {
    imatriz2d mtx = icreamatriz2d(FILAS,N);
    srand(SEMILLA);
    for (int i = 0; i < N; ++i) {
        mtx[0][i] = rand()%M;           // VENTAS
        do {
            mtx[1][i] = rand()%M;       // COMPRAS
        } while(mtx[1][i] == mtx[0][i]);
        mtx[2][i] = rand()%MAX_FECHA;   // FECHAS
    }

    mtx[2][5] = 0;  // Ajustes según para la semilla 29
    mtx[2][6] = 2;
    mtx[2][8] = 6;
    mtx[2][9] = 9;

    mtx[1][6] = 4;  // Infecta a la granja 5
    mtx[1][8] = 5;  // Infecta a la granja 6


    printf("\n\tINFORMACION DE LA COMPRA-VENTA\n");
    mostrarMtx(mtx,FILAS,N);


    ivector infectados = icreavector(M);
    for (int i = 0; i < M; ++i)
        infectados[i] = NULO;
    infectados[0] = 0;

    ivector cuarentena = aftosa(infectados,mtx);

    printf("\n\tCOMPRA-VENTA ORDENADA\n");
    mostrarMtx(mtx,FILAS,N);

    printf("\n\tINFECTADOS\n");
    mostrar(infectados,0,M-1,M);

    printf("\n\tSOLUCION: EN CUARENTENA\n");
    mostrar(cuarentena,0,M-1,M);

    ifreevector(&cuarentena);
    ifreevector(&infectados);
    ifreematriz2d(&mtx);
    return 0;
}
```

### Funciones auxiliares (opcional)

**NOTA**: No hay que implementar en el examen esto de aquí.

```c
void mostrarMtx(imatriz2d mtx, int nf, int nc){
    for (int i = 0; i < nf; ++i) {
      printf("%i:\t",i+1);
      for (int j = 0; j < nc; ++j)
          printf("%i\t", mtx[i][j]+1);   /// Se imprimen por columnas
      printf("\n");
    }
}

void mostrar(ivector v, int i, int f, int tam){
    for (int j = 0; j < tam; ++j) {
        if (j == i)     printf("(");
        if (v[j] != -1) printf("%i",v[j]+1);
        else            printf("-");
        if (j == f)     printf(")");
                        printf("\t");
    }
    printf("\n");
}
```

