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

**[ No se tiene el enunciado original de este problema ]**

Se busca guardar de un vector ``v`` de ``N`` elementos, los ``M`` números impares más grandes que contenga.

_Por ejemplo_:

(8,  ``9``,  3,  1,  6,  ``7``,  ``5``,  3)

Los 3 impares mas grandes que hay en ``v`` son ``9``, ``5`` y ``7``

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

* Eficiencia del clasico: ``O(n·M) = O(n)``, ya que ``M`` es constante y pequeño
* Eficiencia del DyV:
  * ``a = 2``, ``b = 2``, ``r = 0``
  * ``2 > 2^0`` => ``O(n^logb(a)) = O(n)``
  * Umbral:                                     
    * ``n = 2·(n/2) + c·n^0``
    * ``n = n + c``
    * ``c = 0`` => Significa que el umbral puede ser cualquier valor
    * => Pero empíricamente, no todos son iguales. Tomar el que de mejores resultados
```c
#define IMPARES 3
#define UMBRAL 3                                // Personalmente tomo 3, como el número IMPARES
#define LIBRE -1                                // En un principio, el vector de impares está libre al completo

void clasico(ivector v, int i, int f, ivector aux) {
    int  minOdd, k, j;                          // Para guardar el minimo de los impares y su posicion
    for (;i <= f; ++i)
        if (v[i]%2==1) {                        // Si el valor es impar
            minOdd = aux[0]; k = 0;             // Inicializo el impar mínimo de aux al primero
            for (j = 0; j < IMPARES; ++j)
                if (aux[j] == LIBRE) {          // Compruebo que todavía no haya posiciones libres
                    aux[j] = v[i];              // Si hay una libre, la asigno y corto el bucle
                    break;
                } else 
                    if (aux[j] < minOdd) {      // Si la posición está ocupada, busco el menor
                        minOdd = aux[j];        // para cuando haya que sustituir un número en aux
                        k = j;                  // Guardo la posición para sustituirla luego
                    }

            if (j == IMPARES)                   // Si está al completo
                if (aux[k] < v[i])              // y si el impar de v supera al más pequeño de aux
                    aux[k] = v[i];
        }
}

void impares(ivector v, int i, int f, ivector aux) {
    int t = f-i+1;
    if (t <= UMBRAL) {
        clasico(v,i,f,aux);
    } else {
        int m =  i + t/2;
        impares(v, i, m - 1, aux);
        impares(v, m, f, aux);
    }
}
```

### Salida de la solución
```
(5,  26,  23,  33,  20,  18,  29,  2,  31,  35,  24,  37,  6,  17,  13,  9,  15,  11)

Los 3 impares mas grandes que hay son (desordenado):
aux[0] = 35
aux[1] = 37
aux[2] = 33

Llamadas al clasico: 8
```