# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [_Problema 3 del modelo A de junio del 14-15_](#problema-3-del-modelo-a-de-junio-del-14-15)
* [Solución](#solución)
* [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->


# Enunciado 

***

### _Problema 3 del modelo A de junio del 14-15_

La partición de un número positivo ``N`` consiste en escribir ese número
como la suma de ``k`` números positivos, por supuesto distintos del original. 

Por ejemplo:

* 12 = 1 + 1 + 2 + 3 + 5 
* 12 = 1 + 2 + 4 + 5 
* 12 = 1 + 2 + 2 + 2 + 5 
* 12 = 3 + 9
* ...

Construya un programa basado en la técnica de Vuelta Atrás que genere todas las
posibles particiones de un número natural dado

# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

```c
#define N 12
#define LIBRE -1

void VA(ivector v, int paso, int suma, int i) {
    if (suma == N) {
        mostrarV(v,0,paso-1,N);
    }else if (paso + 1 <= N) {
        if (suma + 1 <= N)                  // No se pasa sumando lo mínimo
        if (suma + (N-1)*(N-paso) >= N) {   // No se queda corto sumando lo máximo
            for (; i < N; ++i) {
                v[paso] = i;
                VA(v,paso+1,suma+i,i);      // Se permiten valores repetidos: i+0
            }
            v[paso] = LIBRE;                // Marcaje: Se deja el valor tal y como estaba
        }
    }
}

```

# Salida de la solución

```
(1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1)
(1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2)
(1, 1, 1, 1, 1, 1, 1, 1, 1, 3)
(1, 1, 1, 1, 1, 1, 1, 1, 2, 2)
(1, 1, 1, 1, 1, 1, 1, 1, 4)
(1, 1, 1, 1, 1, 1, 1, 2, 3)
(1, 1, 1, 1, 1, 1, 1, 5)
(1, 1, 1, 1, 1, 1, 2, 2, 2)
(1, 1, 1, 1, 1, 1, 2, 4)
(1, 1, 1, 1, 1, 1, 3, 3)
(1, 1, 1, 1, 1, 1, 6)
(1, 1, 1, 1, 1, 2, 2, 3)
(1, 1, 1, 1, 1, 2, 5)
(1, 1, 1, 1, 1, 3, 4)
(1, 1, 1, 1, 1, 7)
(1, 1, 1, 1, 2, 2, 2, 2)
(1, 1, 1, 1, 2, 2, 4)
(1, 1, 1, 1, 2, 3, 3)
(1, 1, 1, 1, 2, 6)
(1, 1, 1, 1, 3, 5)
(1, 1, 1, 1, 4, 4)
(1, 1, 1, 1, 8)
(1, 1, 1, 2, 2, 2, 3)
(1, 1, 1, 2, 2, 5)
(1, 1, 1, 2, 3, 4)
(1, 1, 1, 2, 7)
(1, 1, 1, 3, 3, 3)
(1, 1, 1, 3, 6)
(1, 1, 1, 4, 5)
(1, 1, 1, 9)
(1, 1, 2, 2, 2, 2, 2)
(1, 1, 2, 2, 2, 4)
(1, 1, 2, 2, 3, 3)
(1, 1, 2, 2, 6)
(1, 1, 2, 3, 5)
(1, 1, 2, 4, 4)
(1, 1, 2, 8)
(1, 1, 3, 3, 4)
(1, 1, 3, 7)
(1, 1, 4, 6)
(1, 1, 5, 5)
(1, 1, 10)
(1, 2, 2, 2, 2, 3)
(1, 2, 2, 2, 5)
(1, 2, 2, 3, 4)
(1, 2, 2, 7)
(1, 2, 3, 3, 3)
(1, 2, 3, 6)
(1, 2, 4, 5)
(1, 2, 9)
(1, 3, 3, 5)
(1, 3, 4, 4)
(1, 3, 8)
(1, 4, 7)
(1, 5, 6)
(1, 11)
(2, 2, 2, 2, 2, 2)
(2, 2, 2, 2, 4)
(2, 2, 2, 3, 3)
(2, 2, 2, 6)
(2, 2, 3, 5)
(2, 2, 4, 4)
(2, 2, 8)
(2, 3, 3, 4)
(2, 3, 7)
(2, 4, 6)
(2, 5, 5)
(2, 10)
(3, 3, 3, 3)
(3, 3, 6)
(3, 4, 5)
(3, 9)
(4, 4, 4)
(4, 8)
(5, 7)
(6, 6)

Hay 76 combinaciones diferentes de sacar 12
Llamadas: 1527
Iteraciones: 1527
```