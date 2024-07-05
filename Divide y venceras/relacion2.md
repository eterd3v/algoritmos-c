# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
<!-- TOC -->

# Enunciado

***

Implementar el algoritmo de la búsqueda ternaria sobre un vector de
enteros ordenado de tamaño ``n``.

Dicho algoritmo es similar a la búsqueda binaria, pero en vez de dividir el
vector en dos partes iguales y decidir si se continúa la búsqueda por la parte
izquierda o por la derecha, este nuevo algoritmo divide el vector en tres partes
iguales y decide si tiene que buscar en la parte izquierda, en la central o en la
derecha.
- Implementar en primer lugar el algoritmo de búsqueda secuencial,
  para utilizarlo cuando el tamaño del caso sea menor o igual que el
  umbral.
- ¿Cuál es la eficiencia del algoritmo de búsqueda ternaria
  implementado?
- ¿Cuál es el mejor umbral?
# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

- La primera petición del ejercicio se corresponde con la función ``clasico(..)`` implementado más abajo en esta página.
- Eficiencia de mergesort es:
  - ``a = 1``, por el número de llamadas recursivas máximas que se ejecutan
  - ``b = n / (n/3) = 3``
  - ``r = 0``, ya que la descomposición y unión es de O(n^``0``)
  - ``1 = 3^0`` ==> ``O(n^r·logb(n)) = O(log3(n))``
- El mejor umbral es:
  - ``n^1 = 1 * (n/3)^0 + n^0`` ; <== Igualas O(clasico)  con O(llamada DyV del clásico)
  - ``n = 1 * 1 + 1``
  - ``n = 2``

**NOTA**: El parámetro ``buscar`` es un puntero creado con ``malloc`` y liberado con ``free``, pero también
se puede implementar utilizando un ``ivector`` con de tamaño 1

```c
int clasico(ivector v, int i, int f, int* buscar) {
    for (;i <= f; ++i)
        if (*buscar == v[i])
            return i;
    return -1;
}

int ternaria(ivector v, int i, int f, int* buscar) {
    int t = f - i + 1;                      // Tamaño
    if (t <= UMBRAL) {                      
        return clasico(v, i, f, buscar);
    }else{
        int t1 = i + (t / 3);               // 1er tercio
        int t2 = i + (2 * t / 3);           // 2o tercio

        if      (*buscar <= v[t1])      return ternaria(v,i ,t1, buscar);   // por debajo de t1
        else if (*buscar >  v[t2])      return ternaria(v,t2,f ,buscar);    // por encima de t2
        else                            return ternaria(v,t1,t2,buscar);    // entre t1 y t2
    }
}
```