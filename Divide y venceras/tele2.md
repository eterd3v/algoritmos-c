# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Salida de la solución 1](#salida-de-la-solución-1)
    * [Salida de la solución 2](#salida-de-la-solución-2)
<!-- TOC -->

# Enunciado

***

Los profesores de la asignatura de Diseño de Algoritmos desean
que sus alumnos mejoren la calificación final de la asignatura.
Para ello les proponen una serie de test diarios voluntarios.

Como las preguntas mal contestadas restan, la calificación final
de cada test puede ser un valor negativo. La nota va a estar en el rango [-100, 100].

Para obtener la calificación final de los test, se puede elegir un
rango de valores, diciendo el primero y el último de los días
entre los que se van a tener en cuenta las notas. **(Solamente se
computarán los valores de las notas obtenidas entre el día inicial
y el día final elegidos)**

Diseñar un algoritmo DyV que, sabiendo las calificaciones
obtenidas en todos los test, devuelva el rango de días en el que
se obtendría la nota máxima.

> _Por ejemplo_, un alumno ha realizado los test y ha obtenido las
> siguientes calificaciones, almacenadas en un vector: (Primer
> valor: 29, corresponde al día 1 y el último: 1, del día 28)
>
> |     |     |     |     |     |     |    |
> |-----|-----|-----|-----|-----|-----|----|
> | 29  | -7  | 14  | 21  | 30  | -47 | 10 |
> | 7   | -39 | 23  | -20 | -36 | -41 | 27 |
> | -34 | ``7``   | ``48``  | ``35``  | ``-46`` | ``-16`` | ``32`` |
> | ``18``  | ``5``   | ``-33`` | ``27``  | ``28``  | -22 | 1  |
> * Si se suman todas las calificaciones obtenidas se obtiene una
nota de 12.
>
> * Si se consideran las notas entre el día 16 y el 26 (valores en
formato de código), la calificación sería de 105

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
void maximaClasico(ivector v, int inf, int sup, ivector vAux) { // O(n^2)
    int suma;
    while (inf <= sup) {
        suma = 0;                           /// El sumatorio se limpia cada vez que cambia el inicio i
        for (int j = inf; j <= sup; ++j) {  /// Con j se pasa a comprobar todos los subvectores de v[i] hasta v[j]
            suma += v[j];
            if (suma > vAux[0]){            /// Si supera el máximo conseguido hasta ahora ...
                vAux[0] = suma;             /// ... se actualiza al nuevo máximo
                vAux[1] = inf;
                vAux[2] = j;
            }
        }
        inf++;
    }
}

void maximaDyV(ivector v, int inf, int sup, ivector vAux) { // a=2, b=2, r=1 --> O(n*log2(n))
    if ( sup-inf+1 <= UMBRAL )
        maximaClasico(v, inf, sup, vAux);
    else {

        int mitad = (inf + sup + 1)/2;

        maximaDyV(v, inf,   mitad - 1,  vAux);
        maximaDyV(v, mitad, sup,        vAux);

        // Se comprueba una tercera región: el centro del vector
        // La idea es que en este subvector, el centro DEBE estar en la suma del subvector
        // Por lo que siempre es una combinación tal que: subvector va desde i hasta c y desde c hasta j
        // Se aprovecha que el subvector pasa por el centro para hallar la suma izquierda y derecha desde el centro
        // para hacer una búsqueda lineal en las dos partes, y así conseguir que no sea de orden cuadrático

        int sumaIzq = 0, sumaDer = 0;
        int centroIni = mitad, centroFin = mitad-1;

        int i = mitad-1, j = mitad, maxIzq=0, maxDer=0;
        while (inf <= i || j <= sup) {          // En el peor de los casos, O(n)

            if (inf <= i) {                     // Recorre como mucho n/2 elementos (empieza en la mitad)
                maxIzq += v[i];                 
                if ( maxIzq > sumaIzq ){
                    sumaIzq = maxIzq;
                    centroIni = i;
                }
                i--;
            }

            if (j <= sup) {                     // Recorre como mucho otros n/2 elementos (empieza en la mitad)
                maxDer += v[j];
                if ( maxDer > sumaDer ){
                    sumaDer = maxDer;
                    centroFin = j;
                }
                j++;
            }

        }

        if ( sumaIzq + sumaDer > vAux[0] && centroIni < centroFin ) {   // Indica si la suma del centro es el máximo
            vAux[0] = sumaIzq + sumaDer;                                // y si es válido tal suma se actualiza
            vAux[1] = centroIni;                                        // al nuevo máximo
            vAux[2] = centroFin;
        }

    }
}
```

### Salida de la solución 1

```
                ========== Vector ==========
-30     -79     9       82      11      -49     -40     93
                ========== Clasico ==========
-30     -79     [9      82      11      -49     -40     93]     Total:  106
                ========== DyV ==========
-30     -79     [9      82      11      -49     -40     93]     Total:  106
```

### Salida de la solución 2
También puede ocurrir que haya varias soluciones posibles dentro de un mismo vector
````
                ========== Vector ==========
-62     -87     5       59      15      -56     -87     79
                ========== Clasico ==========
-62     -87     [5      59      15]     -56     -87     79      Total:  79
                ========== DyV ==========
-62     -87     5       59      15      -56     -87     [79]    Total:  79
````