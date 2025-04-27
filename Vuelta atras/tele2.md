# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
    * [A tener en cuenta](#a-tener-en-cuenta)
    * [Una mejor del algoritmo](#una-mejora-del-algoritmo)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Enunciado

***

Los transportistas siguen en huelga. Para tener más visibilidad en una ciudad, la Unión de Transportistas ha decidido que cada uno de los ``n`` transportistas vaya a uno de los ``n destinos diferentes``.

Los litros de combustible que gasta el transportista ``i`` si va al destino ``j`` se han almacenado en la celda ``(i, j)`` de una matriz de ``nxn`` (número de transportistas x número destinos)

Implementa un algoritmo de Vuelta Atrás para decidir el lugar al que debe ir cada transportista de manera que se minimice el número total de litros de diésel consumidos.

### A tener en cuenta

- Representación de la solución: Vector S={s1, s2, …, sn}. Donde se representa el destino asignado al transportista ``i``.
- Empezando por el primer transportista, en cada etapa el algoritmo irá avanzando en la construcción de la solución, comprobando siempre que el nuevo valor añadido a ella es compatible con los valores anteriores.
- Por cada solución que encuentre anotará su coste (litros consumidos) y lo comparará con el coste de la mejor solución encontrada hasta el momento

### Una mejora del algoritmo

Realizar podas en el árbol implícito eliminando aquellos nodos que no van a llevar a la solución óptima.

Para ello la función de factibilidad debe hacer uso de una cota que almacene los litros consumidos en la mejor solución hasta el momento, y además llevar la cuenta en cada nodo de los litros acumulados hasta ese momento.

Si este valor es mayor que el valor de la cota, no merece la pena continuar explorando los hijos de ese nodo, pues nunca nos llevarán a una solución mejor de la que tenemos.

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:
```c

```

### Salida de la solución

```
abc
```