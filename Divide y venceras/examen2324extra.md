# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [Problema 1 de la extraordinaria 23-24](#problema-1-de-la-extraordinaria-23-24)
* [Solución](#solución)
  * [Algoritmo del problema](#algoritmo-del-problema)
  * [Funciones auxiliares](#funciones-auxiliares)
  * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->


# Enunciado 

***

### Problema 1 de la extraordinaria 23-24

Encontrar el la diferencia mínima de dos elementos de todo el vector. 
De forma clásica tomaría O(n^2)

**Ejemplo**: 
>
> ``V = {239, 390, 18, 411, 220, 137, 447, 476}``
> 
> Aquí la diferencia sería de |239 - 220| = 19

Diseñar un algoritmo DyV que devuelva dicha diferencia únicamente con la llamada recursiva y que mejore el tiempo del clásico.

# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

## Algoritmo del problema

```c
#UMBRAL 2

void diferencias(ivector v, int i, int f, int* d) { // O(n)
    while (i < f)                       // No puede ser i==f o *d valdría cero
        *d = min(*d, abs(v[f]-v[i])),   // Mirar Funciones auxiliares 
        ++i;
}

void clasico(ivector v, int i, int f, int* d) { // O(n * (2n)) = O(2n^2) = O(n)
    int origen = i;
    while (i <= f) {
        for (int j = i+1; j <= f; ++j)              // 1º ordenas desde i hasta j
            if (v[i] > v[j])
                swap(v,i,j);
        diferencias(v, origen, i, d);               // 2º dif. mín. entre origen y la que acabas de ordenar
        ++i;
    }
}

int encuentraPivote(ivector v, int i, int j){       // O(n)
    for (int k = i+1; k <= j; ++k)                  // Pivote: índice que deja valores los menores a su izquierda
        if (v[i] != v[k])
            return v[i] < v[k] ? k : i;             // El mayor de dos valores diferentes
    return -1;                                      // Si todos son iguales, no hace falta procesar más
}

int reordena(ivector v, int i, int j, int pivote){  // Inicio: pivote = v[i] ó pivote = v[k] (ver arriba)
    do{                                             // Se fueza a: menores que pivote, a la izquierda. Mayores que pivote, a la derecha
        swap(v,i,j);                                // v[i] >= pivote && pivote < v[j], intercambia
        while (v[i] < pivote)   i++;                // Hace una 'pinza': llega al primero que v[i] >= pivote (menores a la izq.)
        while (v[j] >= pivote)  j--;                // Hace una 'pinza': llega al primero que v[j] < pivote (mayores o = a la der.)
    }while(i <= j);                                 // Como mucho es n elementos => O(n)
    return i;
}                                                   // O(n)

void diferenciasDyV(ivector v, int i, int f, int *d){
    if (f-i+1 <= UMBRAL)
        clasico(v,i,f,d);
    else {
        int pivote = encuentraPivote(v,i,f);
        if (pivote != -1) {
            int k = reordena(v,i,f,v[pivote]);
            diferenciasDyV(v, i, k - 1, d);
            diferenciasDyV(v, k, f, d);
        }
        diferencias(v,i,f,d); // Cuando ordenas desde i hasta f, actualizas *d
    }   // a=2, b=2 y r=1 ya que la descomposición es O(n + n + n) = O(n^1)
}       // a=b^r => 2=2 => El DyV está en O(n·log2(n))
```

## Funciones auxiliares

```c
void swap(ivector v, int i, int j){
    int aux = v[i];
    v[i] = v[j];
    v[j] = aux;
}

int min (int a, int b) {
    return a < b ? a : b;
}

int abs(int a) {
    return a < 0 ? -a : a;
}
```

## Salida de la solución

```
152     212     (134)   90      32      245     107     40      93      252     (133)   114     176     184     136     16
16      32      40      90      93      107     114     (133    134)    136     152     176     184     212     245     252

Dif. minima: 1
Iteraciones de diferencias(v,i,f,*d): 60
Diferencias con el método clásico:    120
```
