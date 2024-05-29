# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Algoritmo principal](#algoritmo-principal)
    * [Funciones auxilares del algoritmo principal](#funciones-auxilares-del-algoritmo-principal)
    * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Enunciado

***

Una sala ha de estar constantemente iluminada por un mínimo de ``M``
bombillas. Para ello, disponemos de un conjunto finito de ``N`` bombillas. 

Cada bombilla ``i`` tiene un tiempo de duración ``t_i`` días. Una vez conectada, no se puede
apagar, continuando encendida hasta que se gaste. Se trata de maximizar el
número de días que podemos tener la sala iluminada. Diseñe una función basada
en una heurística voraz que devuelva el orden en el que se tienen que ir
encendiendo las bombillas.


_Ejemplo: Pongamos una heurística de selección aleatoria. Es mala, por eso
se pone como ejemplo, no se puede usar._

| Bombillas   | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9  | 10 |
|-------------|---|---|---|---|---|---|---|---|----|----|
| Duración    | 1 | 4 | 2 | 3 | 1 | 1 | 3 | 1 | 5  | 2  |
| Orden (M=3) | 6 | 9 | 3 | 4 | 7 | 2 | 1 | 8 | 10 | 5  |


_Con la elección de 3 bombillas necesarias y la selección aleatoria sale un
total de 7 días._

| Días        | 1 | 2       | 3       | 4       | 5       | 6       | 7        | ``8``     |
|-------------|---|---------|---------|---------|---------|---------|----------|-------|
| Bombillas   | 6, 9, 3 | 4, 9, 3 | 4, 9, 7 | 4, 9, 7 | 2, 9, 7 | 2, 1, 8 | 2, 10, 5 | ``2, 10`` |

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

### Algoritmo principal

```c
#define M 3
#define N 10
#define INVALIDO -1
#define L_MIN -99999

/**
 * HEURÍSTICA:
 * Buscar la posición de la duración máxima aún no utilizada
 */
int seleccion(ivector duracion, ivector sinUsar) {
    int idx = INVALIDO;
    int max = L_MIN;
    for (int i = 0; i < N; ++i)
        if (sinUsar[i] && max <= duracion[i]) {
            max = duracion[i];
            idx = i;
        }
    return idx;
}

ivector bombillas(ivector duracion){
    ivector orden = icreavector(N);
    ivector sinUsar = icreavector(N);
    for (int i = 0; i < N; ++i)
        sinUsar[i] = 1;

    int ordenados = 0;
    while(!solucion(ordenados)){
        int idx = seleccion(duracion, sinUsar);
        if (factible(idx)){
            orden[ordenados++] = idx;
            sinUsar[idx] = 0;
        }
    }

    ifreevector(&sinUsar);
    return orden;
}
```

### Funciones auxilares del algoritmo principal

````c
int factible(int idx) {
    return idx != INVALIDO;
}

int solucion(int i) {
    return i < 0 || i >= N;
}
````

### Salida de la solución

```
Bombillas:      (0      1       2       3       4       5       6       7       8       9
Duracion:       (3      2       4       3       5       5       1       4       4       5)
===     ===     ===     ===     ===     ===     ===     ===     ===     ===     ===     ===
Orden:          (9      5       4       8       7       2       3       0       1       6)
Duracion:       (5      5       5       4       4       4       3       3       2       1

===     COMPROBACION    ===
t(1):   9, 5, 4,
t(2):   9, 5, 4,
t(3):   9, 5, 4,
t(4):   9, 5, 4,
t(5):   9, 5, 4,
t(6):   8, 7, 2,
t(7):   8, 7, 2,
t(8):   8, 7, 2,
t(9):   8, 7, 2,
t(10):  3, 0, 1,
t(11):  3, 0, 1,
t(12):  3, 0, 6,
```