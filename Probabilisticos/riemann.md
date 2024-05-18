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

Se realiza el siguiente experimento:
- Un jugador inexperto lanza `n` dardos en una diana redonda inscrita en un cuadrado
- Su “habilidad” es tal, que cada punto del cuadrado ¡¡es equiprobable!!
- De un total de `n` dardos ¿cuántos caen dentro del círculo?


# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

- De un total de n dardos ¿cuántos caen dentro del círculo?
  - El cuadrado tiene de lado `2r`, el círculo tendrá una superficie de `π·r^2`, y la superficie del cuadrado será de `4·r^2` 
  - La relación de dardos que caen dentro y fuera será de `π·r^2 / 4·r^2 = π/4 `
  - En lugar de fijarnos en calcular el valor de `k`, estimemos en valor de `π`:
  - Si de cada `4` dardos, caen dentro `π`, por cada `n` caerán `k` (Regla de Tres)
  - `π ≈ 4·k/n`
- Cuanto más grande sea el número de lanzamientos, más exacto será el valor de `π`

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

#define N 1000000
#define SEMILLA 113
#define MAX_RAND 512

float rand01() {
    return (float)(rand()%MAX_RAND) / (float)MAX_RAND;
}

float funcion(float x) {
    return cos(x);                  // Podría ser cualquier función
}

float riemann(long n){
    float k = 0.0f, x, y;
    for (long i = 0; i < n; ++i) {
        x = rand01();               // Tirar a la diana
        y = rand01();               // Tirar a la diana
        if (y <= funcion(x))
            k += 1.0f;
    }
    return k / (float)n;
}

int main() {
    srand(SEMILLA);
    printf("La integral de f(x) entre 0 y 1 es aprox. %f usando %i dardos\n", riemann(N), N);
    return 0;
}
```

### Salida de la solución
```
La integral de f(x) entre 0 y 1 es aprox. 0.843181 usando 1000000 dardos
```