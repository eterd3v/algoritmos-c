# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Algoritmo principal](#algoritmo-principal)
    * [Funciones auxiliares del algoritmo principal](#funciones-auxiliares-del-algoritmo-principal)
    * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Enunciado

***

El repartidor de Uber tiene que entregar ``N`` pedidos de diferente 
valor teniendo que hacerlo antes de lo indicado en la tabla. 

- En un instante determinado solamente puede entregar un pedido.
- Si entrega tarde un pedido, al cliente le sale gratis.

| Pedido        | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  |
|---------------|----|----|----|----|----|----|----|----|----|
| Tiempo límite | 1  | 5  | 5  | 6  | 6  | 4  | 4  | 2  | 2  |
| A cobrar      | 60 | 70 | 80 | 20 | 20 | 30 | 50 | 50 | 90 |



Construye una heurística de tu invención y un algoritmo Voraz basado en lo explicado en el libro de G. Brassard,
P. Bratley en la sección de “Planificación con plazo fijo” (página 233 y siguientes,
en la edición de 1997), que le indique el orden que debe seguir en el reparto para
obtener el máximo beneficio. 

Aplícalo al ejemplo detallando TODOS los pasos.

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

### Algoritmo principal

```c
#define TAM 14

ivector uberGreedy(ivector vTiempo, ivector vCobrar, int cols){
    ivector vSol = icreavector(TAM);

    ivector sinUsar = icreavector(TAM); // Vector auxiliar
    for (int i = 0; i < TAM; ++i) sinUsar[i] = 1;

    while(!solucion(cols)){
        int idx = seleccion(vTiempo, vCobrar, sinUsar, cols + 1);
        if (factible(idx)) {
            vSol[cols++] = idx;
            sinUsar[idx] = 0;
        }
    }

    ifreevector(&sinUsar);
    return vSol;
}
```

### Funciones auxiliares del algoritmo principal

```c
int solucion(int cols) {
    return TAM <= cols ? 1 : 0;
}

int factible(int idx){
    return idx != -1 ? 1 : 0;
}

/**
 *  HEURISTICA:
 *  Tomar el índice que esté más cerca de poder ser usado tal que
 *  maximice el coste. Tienen prioridad pedidos a tiempo, mientras
 *  que los que no lleguen a tiempo se busca simplemente el máximo
 */
int seleccion(ivector vTiempo, ivector vCobrar, ivector sinUsar, int cont) {
    int sel = -1,       selAtiempo = -1;
    float max = 0.f,    maxAtiempo = 0.f;
    float p;
    for (int i = 0; i < TAM; ++i) {

        // (cont / vT) * coste de i * se ha usado i (vale 0 o 1)
        p  = (float)(cont * vCobrar[i] * sinUsar[i]) / (float) vTiempo[i] ;

        if (cont <= vTiempo[i] && p > maxAtiempo) {  // Como puede estar desordenado
            selAtiempo = i;                     // mantengo la búsqueda tanto si
            maxAtiempo = p;                     // entra a vTiempo como si no
        } else if (p > max) {
            sel = i;
            max = p;
        }
    }
    return selAtiempo != -1 ? selAtiempo : sel;
}


```

### Salida de la solución

```
(3      3       4       1       6       4       5       2       1       4       6       1       2       2)
(90     80      120     110     100     60      70      20      80      130     60      50      70      30)

-------------    PLANIFICACION, IZQ A DER =>   -------------
 1      2       4       4       6       6       1       1       3       3       4       2       5       2
 110    70      130     120     100     60      0       0       0       0       0       0       0       0
 
 Total: 590
```