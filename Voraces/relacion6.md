# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Algoritmo principal](#algoritmo-principal)
    * [Funciones auxilares para el algoritmo principal](#funciones-auxilares-para-el-algoritmo-principal)
    * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Enunciado

***

Se quiere colocar una colección de n libros en una estantería de m baldas.
Los libros son muy dispares y sus pesos difieren mucho unos de otros. Se quiere
repartir el peso lo más uniformemente posible, evitando así que se comben.

Diseñar una heurística voraz que resuelva el problema, _asignando en cada
iteración un libro a la balda que menor peso está soportando_.

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

### Algoritmo principal

Es una primera parte del problema, que es conocer el camino mínimo entre París y Jaén

```c
#define INVALIDO -1
#define L_MAX 9999
#define N 13            // libros
#define M 4             // baldas

/**
 * HEURÍSTICA
 * Coloco el libro más pesado aún no colocado en la balda de menor peso
 * Para ello ordeno primero los libros de mayor a menor
 * Luego, voy colocando cada libro i en la balda de menor peso
 * Como estan ordenados, tras colocar un libro, la balda de menor peso ..
 * .. suele estar muy cerca (la anterior, la misma o la siguiente)
 */
int seleccionBalda(ivector pesoBaldas){
    int balda = INVALIDO;
    int menor = L_MAX;
    for (int i = 0; i < M; ++i)
        if (menor > pesoBaldas[i]) {
            menor = pesoBaldas[i];
            balda = i;
        }
    return balda;
}

ivector baldasGreedy(ivector pesoLibros) {
    
    ivector ordenLibros = icreavector(N);
    ivector pesoBaldas = icreavector(M);
    qsort(pesoLibros,N,sizeof(int), func_comp);     // Ordeno los libros, en descendiente

    for (int i = 0; i < M; ++i)
        pesoBaldas[i] = 0;

    int i=0;
    while (!solucion(i)) {
        int balda = seleccionBalda(pesoBaldas);     // Toma la balda con menor peso
        if (factible(balda)){                       // Los libros ya están ordenados => La balda con menos peso es,
            pesoBaldas[balda] += pesoLibros[i];     // excepto al inicio, la última escogida (baja, sube, baja, ...)
            ordenLibros[i++] = balda;               // El libro i tiene la balda
        }
    }

    ifreevector(&pesoBaldas);
    return ordenLibros;
}
```

### Funciones auxilares para el algoritmo principal

````c
int func_comp(const void *a, const void * b){   // Función de comparación para el quicksort
    int a_ = *((int*)a);
    int b_ = *((int*)b);
    if (a_ < b_) return  1;
    if (a_ > b_) return -1;
    return 0;
}

int solucion (int i){
    return i < 0 || i >= N;
}

int factible (int idx){
    return idx >= 0 && idx < N;
}
````

### Salida de la solución

```
Peso de cada uno de los libros:
(130    34      81      9       74      11      62      65      31      97      43      84      105)

Balda 1:        130     62      11
Total=203

Balda 2:        105     65      34
Total=204

Balda 3:        97      74      31      9
Total=211

Balda 4:        84      81      43
Total=208

Promedio de los libros: 206.50
```