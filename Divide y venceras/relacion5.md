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

En una matriz ``n x n``, con ``n`` potencia de 2, se almacenan una serie de
números enteros positivos de forma desordenada. Se desean conocer,
cuántos números hay en la matriz cuyo valor está en el intervalo cerrado
``[a, b]``, siendo ``a`` y ``b`` dos valores pasados como parámetros.

* Diseñar e implementar el algoritmo clásico 
* Diseñar e implementar el Divide y Vencerás

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

* Eficiencia del clasico: ``O(filas·columnas) = O(f·c) = O(n·n) = O(n^2)``
* Eficiencia del DyV: 
  * ``a = 4``, ``b = 4``, ``r = 0``
  * ``4 > 4^0`` => ``O((f·c)^logb(a)) = O(f·c) = O(n^2)``
* Umbral:
  * ``f·c = 4 * ((f·c)/4)^2 + (f·c)^0``
  * ``f·c = 4 * (f·c)^2/4^2 + 1``
  * ``f·c = (f·c)^2/4 + 1`` => Tomamos ``x = f·c``
  * ``(x^2)/4 - x + 1 = 0`` => [Ecuación de 2º grado](https://es.wikipedia.org/wiki/Ecuaci%C3%B3n_de_segundo_grado#Soluciones_de_la_ecuaci%C3%B3n_de_segundo_grado)
  * Raíces: ``x=2`` y ``x=2``  => Siempre va a ser 2
  * ``x = f·c = 2`` => ``n^2 = 2`` => Tomamos esto último y no la raíz de ``n``

```c
int clasico(imatriz2d mtx, int iMin, int iMax, int jMin, int jMax, int vMin, int vMax){
    int suma = 0;
    int j;
    while (iMin <= iMax){                                           // Se recorre f veces
        j = jMin;
        while (j <= jMax) {
            if ( vMin <= mtx[iMin][j] && mtx[iMin][j] <= vMax)      // Se recorre c veces
                ++suma;
            ++j;
        }
        ++iMin;
    }
    return suma;
}

int numMatrices(imatriz2d mtx, int iMin, int iMax, int jMin, int jMax, int vMin, int vMax){
    int iT = (iMax-iMin+1);
    int jT = (jMax-jMin+1);

    if (iT * jT <= UMBRAL)  
        return clasico(mtx,iMin,iMax,jMin,jMax,vMin,vMax);
    else {
        int v = 0;
        int iMitad = iMin + iT/2;
        int jMitad = jMin + jT/2;
        v += numMatrices(mtx,   iMin,   iMitad-1,   jMin,   jMitad-1,   vMin,   vMax);
        v += numMatrices(mtx,   iMitad, iMax,       jMin,   jMitad-1,   vMin,   vMax);
        v += numMatrices(mtx,   iMin,   iMitad-1,   jMitad, jMax,       vMin,   vMax);
        v += numMatrices(mtx,   iMitad, iMax,       jMitad, jMax,       vMin,   vMax);
        return v;
    }
}
```

### Salida de la solución

```
6       7       22      5         -- Fila 0
23      21      13      29        -- Fila 1
18      20      29      4         -- Fila 2
22      25      25      17        -- Fila 3

Hay 10 elementos entre 6 y 24 (incluidos)
```