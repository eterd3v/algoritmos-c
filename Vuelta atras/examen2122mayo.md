# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [_Problema 1 de mayo del 21-22_](#problema-1-de-mayo-del-21-22)
* [Solución](#solución)
    * [Algoritmo principal](#algoritmo-principal)
    * [Salida de la solución](#salida-de-la-solución)
    * [Funciones auxiliares (opcional)](#funciones-auxiliares-opcional)
<!-- TOC -->


# Enunciado 

***

### _Problema 1 de mayo del 21-22_

El juego de las 31 usa cartas de la baraja española con las siguientes puntuaciones: 
1, 2, 3, 4, 5, 6, 7, 10 (sota), 11 (caballo) y 12 (rey) con 4 palos: oros (O), copas (C), espadas (E) y
bastos (B).

Diseñar un algoritmo basado en la técnica de la Vuelta Atrás que imprima en pantalla
todas las posibles formas de obtener 31 puntos utilizando siempre 4 cartas y como mucho dos
palos distintos en cada combinación.

**Ejemplos:**

 - Sota_O + Caballo_O + 7_C + 3_B -> Incorrecta por tener 3 palos
 - Sota_O + Caballo_O + 7_C + 3_C -> Correcta

# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

### Algoritmo principal

```c
#define MAX_CARTAS 4
#define MAX_PALOS 2
#define N_CARTAS 10
#define N_PALOS 4
#define PUNTOS 31
#define TAM N_CARTAS * N_PALOS
#define USADA -2

int MINV, MAXV; // Inicializados en main

void VA(ivector v, int cartas, int palos, int i, int puntaje, ivector vPalos) {
    if (puntaje == PUNTOS && cartas == MAX_CARTAS) {
        mostrarSolucion(v);
    }else if (cartas + 1 <= MAX_CARTAS) {
        if (puntaje + MINV                      <= PUNTOS) // Poda: Si no se cuela del objetivo sumando lo mínimo
        if (puntaje + (MAX_CARTAS-cartas)*MAXV  >= PUNTOS) // Poda: Si no se queda corto, sumando lo máximo posible
        for (int k, pUsado=0, aux; i < TAM; ++i) {
            k = i / N_CARTAS;
            aux = v[i];

            if (vPalos[k]) {
                vPalos[k]--;
                palos++;
                pUsado=1;
            }

            if (palos <= MAX_PALOS) {                       // Poda: Número de palos es válido
                v[i] = USADA;
                VA(v, cartas+1, palos, i+1, puntaje+aux, vPalos);
                v[i] = aux;
            }

            if (pUsado) {
                vPalos[k]++;
                palos--;
                pUsado = 0;
            }
        }

    }
}
```

### Salida de la solución

```
1_O     6_O     12_O    12_C
1_O     6_O     12_O    12_E
1_O     6_O     12_O    12_B
1_O     7_O     11_O    12_O
1_O     7_O     11_O    12_C
1_O     7_O     11_O    12_E

...

2_B     7_B     10_B    12_B
3_B     5_B     11_B    12_B
3_B     6_B     10_B    12_B
3_B     7_B     10_B    11_B
4_B     5_B     10_B    12_B
4_B     6_B     10_B    11_B

Soluciones: 1016
Llamadas: 14642
Iteraciones: 34442
```

### Funciones auxiliares (opcional)

**NOTA**: No hay que implementar en el examen esto de aquí.

```c
void palos(int x){
    switch (x) {
        case 0: printf("O\t"); break;
        case 1: printf("C\t"); break;
        case 2: printf("E\t"); break;
        case 3: printf("B\t"); break;
        default: break;
    }
}

void mostrarSolucion(ivector v) {
    printf("\n");
    for (int j = 0; j < TAM; ++j){
        if (v[j] == USADA) {
            int k = 1 + j%N_CARTAS;
            if (k >= 8) k+=2; // No sumo 3 por el 1 de la linea anterior
            printf("%i_", k);
            palos(j/N_CARTAS);
        }
    }
}
```

