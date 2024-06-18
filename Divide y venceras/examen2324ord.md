# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [Problema 1 de la ordinaria 23-24](#problema-1-de-la-ordinaria-23-24)
* [Solución](#solución)
  * [Algoritmo del problema](#algoritmo-del-problema)
  * [Salida de la solución](#salida-de-la-solución)
  * [Programa principal (opcional)](#programa-principal-opcional)
<!-- TOC -->


# Enunciado 

***

### Problema 1 de la ordinaria 23-24

Llamamos vector rotado a aquel vector de números enteros que, 
estando ordenado ascendentemente, se le ha hecho una rotación 
de sus últimos k elementos. 

**Ejemplo**: 

``V = {5 6 7 8 9 10 11 12 13 14 15 16 1 2 3 4}``

Diseñar un algoritmo basado en la técnica DyV que, dado un vector rotado, devuelva su elemento menor con una compelida menor que O(n)

# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

## Algoritmo del problema

```c
#define UMBRAL 1

int clasico(ivector v, int i, int f){
    int temp = v[i++];
    for (; i <= f; ++i)
        if (temp < v[i])
            temp = v[i];
    return temp;
}

/**
 *  a = 1,
 *  b = 2
 *  r = 0   =>  1 == 2^0
 *          =>  O( (n^r) * (log b (n) ) = O(log2(n))
 */
int funcionDyV(ivector v, int i, int f) {
    if (f-i < UMBRAL)
        return clasico(v,i,f);
    else {
        int medio = (i + f)/2;
        if (v[medio] > v[f])                    // Como se rotan los últimos elementos, basta
            return funcionDyV(v, medio+1, f);   // comprobar el valor del final con la mitad 
        return funcionDyV(v, i, medio);
    }
}
```

## Salida de la solución

```
El minimo elemento en el vector rotado es: 0
(3, 4, 5, 6, 7, 8, 9, 0, 1, 2)
```

## Programa principal (opcional)

**NO HAY QUE IMPLEMENTAR EN EL EXAMEN ESTO DE AQUÍ**

```c
int main() {
    int arr[] = {3, 4, 5, 6, 7, 8, 9, 0, 1, 2};
//    int arr[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};   // Con esta variante también funciona
//    int arr[] = {9,8,7,6,5,4,3,2,1,0};               // Con esta variante también funciona
    int longitud = sizeof(arr) / sizeof(arr[0]);
    int minimo = funcionDyV(arr, 0, longitud-1);
    printf("El minimo elemento en el vector rotado es: %d\n", minimo);
    mostrar(arr,longitud,0,longitud-1);
    return 0;
}
```
