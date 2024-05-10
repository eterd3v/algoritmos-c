# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [Problema 2 de la ordinaria 22-23](#problema-2-de-la-ordinaria-22-23)
* [Solución](#solución)
  * [Algoritmo del problema](#algoritmo-del-problema)
  * [Salida de la solución](#salida-de-la-solución)
  * [Programa principal (opcional)](#programa-principal-opcional)
  * [Funciones auxiliares (opcional)](#funciones-auxiliares-opcional)
<!-- TOC -->


# Enunciado 

***

### Problema 2 de la ordinaria 21-22



# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

Algoritmo de floyd. En el que los nodos son los idiomas y las traducciones son las aristas. El peso de una
arista (i, j) es el siguiente:
- **arista(i,j) = 1**, si existe traducción directa entre el lenguaje i y el lenguaje j.
- **arista(i,j) = ∞**, en caso contrario.

De esta forma obtenemos “la longitud del camino mínimo” de la traducción entre cada par de lenguajes.
La complejidad del algoritmo: O(n^3).

## Algoritmo del problema

```c
int clasico (ivector v, int i, int f) {          // Como mucho se recorre desde i hasta f => O(n)
    while ( i <= f ) {
        while ( v[i] > v[i+1] && i+1 < f) ++i;  // Mientras haya un número más pequeño, crecer   ++i
        while ( v[f] > v[f-1] && f-1 > i) --f;  // Mientras haya un número más grande, decrecer  --f
        if ( v[i] == v[f] ) i++;                // Mientras sean iguales, cambiar el índice
        else break;                             // Si son diferentes... (tras el break)
    }
    return v[i] > v[f] ? f : i;                 // ...se devuelve el menor de los dos
}

int minimoLocal (ivector v, int i, int f){       // a=1, b=2, r=0 => O(log2(n))
    int t = f-i+1;
    if (t <= UMBRAL) {
        return clasico(v, i, f);
    } else {
        int m = i + t/2;
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
void swap(ivector v, int idx1, int idx2){
    int aux = v[idx1];
    v[idx1] = v[idx2];
    v[idx2] = aux;
}

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

```

