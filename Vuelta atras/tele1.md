# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
    * [Nota 1](#nota-1)
    * [Nota 2](#nota-2)
    * [Guía](#guía)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Enunciado

***

Dado un valor entero ``n``, mostrar todos los números de ``n`` cifras que se pueden formar que cumplan la siguiente condición: 

_“la suma de los dígitos que ocupan las posiciones pares es igual a la de los que ocupan las posiciones impares”_

# Nota 1
Para simplificar el problema, considerar que solamente se pueden utilizar los dígitos del 1 al 5. Ejemplos:
 - Si n=2, los posibles números serían: 11, 22, 33, 44 y 55
 - Si n=3, los posibles números serían: 121, 132, 143, 154, 231, 242, 253, 341, 352 y 451
 - Si n=4, algunos de los posibles números serían: 1111, 1122, 1133, 1144, 1155, 1221, 1232, 1243, 1331, 1342, 1441, 1551, etc.

# Nota 2
Se puede trabajar con un vector para almacenar cada uno de los dígitos y, antes de dar la salida, convertirlo a un valor numérico.


# Guía

La función de factibilidad debe tener en cuenta:
- La posición del dígito que se va completando en cada momento
- La suma de los dígitos de las posiciones pares y la de los impares

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
#define N_CIFRAS 5
#define CIFRA_MIN 1 // No deja cifras inferiores a 1
#define CIFRA_MAX 5 // No deja cifras inferiores a 5

int numPermutaciones = 0;

int factible(int diferencia){
    if (diferencia < 0)
        diferencia = -diferencia;           // Valor absoluto de la diferencia
    return diferencia <= CIFRA_MAX;         // Si los pares difieren más de lo posible con los impares, no da soluciones.
}

void cifras(ivector vector, int n, int puestas, int diferencia){
    if (puestas == n) {
        if (diferencia == 0) {                           // Permutación encontrada
            mostrar(vector, 0, n-1, n);
            numPermutaciones++;
        }
    } else {
        for (int i = CIFRA_MIN; i <= CIFRA_MAX; ++i) {   // Bucle para recorrer todas las cifras posibles (decimal)
            vector[puestas] = i;                         // Marcaje
            int nuevaDif = diferencia;
            nuevaDif += puestas%2 == 0 ? i : -i ;        // Si ya es par no lo toco; Si es negativo le resto
            if (factible(nuevaDif))
                cifras(vector, n, puestas + 1,nuevaDif); // Recursividad. Cambia el índice del vector => puestas + 1
        }
    }
}
```

### Salida de la solución

```
abc
```