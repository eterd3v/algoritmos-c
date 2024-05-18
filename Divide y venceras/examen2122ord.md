# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [Problema 2 de la ordinaria 21-22](#problema-2-de-la-ordinaria-21-22)
* [Solución](#solución)
  * [Algoritmo del problema](#algoritmo-del-problema)
  * [Salida de la solución](#salida-de-la-solución)
  * [Programa principal (opcional)](#programa-principal-opcional)
  * [Funciones auxiliares (opcional)](#funciones-auxiliares-opcional)
<!-- TOC -->


# Enunciado 

***

### Problema 2 de la ordinaria 21-22

Un mínimo local de un array ``A[0...n-1]`` es un elemento A(k) que satisface que:
- ``A(k-1) ≥ A(k) ≤ A(k+1)``
- Como mínimo, el vector tiene 3 elementos y ``A(0) ≥ A(1)`` y ``A(n-2) ≤ A(n-1)`` 

Estas condiciones garantizan la existencia de algún mínimo local.

Diseña un algoritmo basado en la técnica DyV que encuentre algún mínimo local de A
que sea más rápido que el evidente de ``O(n)`` en el peor caso.

# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

## Algoritmo del problema

```c
#define UMBRAL 3                               

int clasico (ivector v, int i, int f) {         // Como mucho se recorre desde i hasta f => O(n)
    while ( i <= f ) {
        while ( v[i] > v[i+1] && i+1 < f) ++i;  // Mientras haya un número más pequeño, crecer   ++i
        while ( v[f] > v[f-1] && f-1 > i) --f;  // Mientras haya un número más grande, decrecer  --f
        if ( v[i] == v[f] ) i++;                // Mientras sean iguales, cambiar el índice
        else break;                             // Si son diferentes... (tras el break)
    }
    return v[i] > v[f] ? f : i;                 // ...se devuelve el menor de los dos
}

int minimoLocal (ivector v, int i, int f) {    // a=1, b=2, r=0 => O(log2(n))
    int t = f-i+1;
    if (t <= UMBRAL) {
        return clasico(v, i, f);
    } else {
        int m = i + t/2;                       // Buscas según que subvector sea menor
        if (v[m-1] <= v[m])     return minimoLocal(v,i,m-1);
        else                    return minimoLocal(v,m,f);
    }
}
```

## Salida de la solución

```
53, 53, 5, 5, 23, 23, 5, 69, 7, (5), 47, 65, 201, 50, 95, 95,

Un minimo local es 5 en v[9]
```

## Programa principal (opcional)

**NO HAY QUE IMPLEMENTAR EN EL EXAMEN ESTO DE AQUÍ**

```c
#define N 16
#define SEMILLA 654897

int main() {
    ivector v = icreavector(N);
    srand(SEMILLA);

    for (int i = 0; i < N; ++i)
        v[i] = 5 + (rand()%N)*(i+1);
    for (int i = 0; i < N; ++i)
        swap(v, rand()%N, rand()%N);

    /// CONDICIONES INICIALES DEL PROBLEMA A(0)≥A(1) y A(n-2)≤A(n-1)
    v[1] = v[0];
    v[N-2] = v[N-1];

    int idx = minimoLocal(v, 0, N-1);
    mostrar(v,N,idx,idx);

    if (idx != -1)     printf("\nUn minimo local es %i en v[%i]", v[idx],idx);
    else               printf("\nNo se ha encontrado minimo local");

    ifreevector(&v);
    return 0;
}
```

## Funciones auxiliares (opcional)

**NO HAY QUE IMPLEMENTAR EN EL EXAMEN ESTO DE AQUÍ**

```c
void swap(ivector v, int idx1, int idx2){
    int aux = v[idx1];
    v[idx1] = v[idx2];
    v[idx2] = aux;
}

void mostrar(ivector v, int t, int i, int f){
    for (int j = 0; j < t; ++j) {
        if (j==i)   printf("(");
                    printf("%i",v[j]);
        if (j==f)   printf(")");
                    printf(", ");
    }
    printf("\n");
}
```

