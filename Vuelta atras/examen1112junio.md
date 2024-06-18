# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [_Problema 2 de junio del 11-12_](#problema-2-de-junio-del-11-12)
* [Solución](#solución)
    * [Algoritmo base](#algoritmo-base)
    * [Algoritmo que se muestra en la solución](#algoritmo-que-se-muestra-en-la-solución)
    * [Salida de la solución](#salida-de-la-solución)
    * [Programa principal (opcional)](#programa-principal-opcional)
    * [Funciones auxiliares (opcional)](#funciones-auxiliares-opcional)
<!-- TOC -->


# Enunciado 

***

### _Problema 2 de junio del 11-12_

Un prolífico escritor tiene que escribir N obras maestras de la
literatura en un tiempo conocido. Cada libro tiene un tiempo de redacción y un beneficio
asociado. Se desea saber cuál es el máximo beneficio alcanzable. Implementar un
algoritmo de vuelta atrás que lo encuentre.

# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

### Algoritmo base

```c
int N; // Se le asigna valor en el main(..)
#define T_MAX 15

void VA(ivector b, ivector t, int i, int* max, int tiempo, int bAct){
    if (i == N && *max < bAct)
        *max = bAct;
    else
        for (; i < N; ++i)
            if (tiempo + t[i] <= T_MAX) 
                VA(b,t,i+1,max,tiempo+t[i],bAct+b[i]);
}
```

### Algoritmo que se muestra en la [solución](##salida-de-la-solución)

```c
#define T_MAX 15

void VA(ivector b, ivector t, int i, int* max, int tiempo, int bAct, ivector sol){
    if (i == N && *max < bAct) {
        *max = bAct;
        mostrarVA(b, bAct, sol);
    } else {
        for (; i < N; ++i) {
            if (tiempo + t[i] <= T_MAX) {
                sol[i] = 1;
                llamadas++;
                VA(b,t,i+1,max,tiempo+t[i],bAct+b[i],sol);
                sol[i] = 0;
            }
            iteraciones++;
        }
    }
}
```

### Salida de la solución

```
2, 6, 7, -, -, -, -, -, -, 1,   MAX:    16
2, 6, -, 5, 3, -, -, -, -, 1,   MAX:    17
2, 6, -, 5, -, 4, -, -, 3, 1,   MAX:    21
2, -, 7, 5, 3, 4, -, -, -, 1,   MAX:    22
-, -, 7, 5, -, 4, 6, -, -, 1,   MAX:    23

El maximo beneficio es de 23 para los 10 libros con 15 T de T_MAX
B:      2       6       7       5       3       4       6       5       3       1
T:      1       7       5       2       4       2       5       5       2       1
Llamadas: 421
Iteraciones: 550
```

### Programa principal (opcional)

**NOTA**: No hay que implementar en el examen esto de aquí.

```c
int main() {

    int b[] =   {2,6,7,5,3,4,6,5,3,1};
    int t[] =   {1,7,5,2,4,2,5,5,2,1};
    int sol[] = {0,0,0,0,0,0,0,0,0,0};

    N = sizeof(t) / sizeof (t[0]);
    int *max = malloc(sizeof(int));
    *max = -9999;

    VA(b,t,0,max,0,0,sol);

    printf("\nEl maximo beneficio es de %i para los %i libros con %i T de T_MAX",*max,N,T_MAX);

    printf("\nB:\t");
    mostrar(b,N,N,N);
    printf("T:\t");
    mostrar(t,N,N,N);
    printf("Llamadas: %i",llamadas);
    printf("\nIteraciones: %i",iteraciones);

    free(max);
    return 0;
}
```

### Funciones auxiliares (opcional)

**NOTA**: No hay que implementar en el examen esto de aquí.

```c
int iteraciones=0, llamadas=0;

void mostrarVA(ivector b, int max, ivector sol){
    for (int i = 0; i < N; ++i)
        printf(sol[i] ? "%i, " : "-, " ,b[i]);
    printf("\tMAX:\t%i\n",max);
}
```

