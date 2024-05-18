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

La idea principal de los algoritmos Sherwood es tomar una decisión aleatoria
sin que perjudique la exactitud de la solución disminuyendo el tiempo esperado de ejecución, 
incluso del peor caso.

Un ejemplo lo encontramos en el QuickSort:
> Para el pivote se toma el mayor de los dos primeros elementos distintos,
> empezando por la izquierda:
> - `O(n^2)` para el peor caso

> Tomando un pivote aleatorio en `[1..n]` mejora el tiempo del peor caso: 
>  - El tiempo esperado es de ``O(n log(n))``

El algoritmo principal no se ve modificado, sigue funcionando bien en todos los casos, pero ahora se ha mejorado el peor


# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
int pivoteSherwood(int i, int tam) {
    return i + rand()%tam;
}

void quickSherwood(ivector v, int i, int f) {
    int t = f-i+1;
    if (t <= UMBRAL) {
        clasico(v,i,f);                 // No se ha modificado del quicksort original
    }else{
        int k = pivoteSherwood(i, t);
        k = reordena(v,i,f,v[k]);       // No se ha modificado del quicksort original
        quickSherwood(v,i,k-1);
        quickSherwood(v,k,f);
    }
}
```

### Salida de la solución
```
DESORDENADO
(140    441     52      368     416     406     404     421     70      279     245     283     344     194)

ORDENADO
(52     70      140     194     245     279     283     344     368     404     406     416     421     441)
```