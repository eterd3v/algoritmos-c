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

Sea ``T`` un vector desordenado de números enteros de tamaño ``n``. Se desea
calcular la suma de todos sus elementos a excepción del mayor y del
menor.

* Diseñar e implementar el algoritmo clásico
* Diseñar e implementar el Divide y Vencerás
* ¿Cuál es el mejor umbral?

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

> **NOTA**: vAux es un vector de 3 elementos: 
> 1. El mínimo, inicializado a un número alto (+999999, por ej.)
> 2. El máximo, inicializado a un número bajo (-999999, por ej.)
> 3. La suma de los elementos menos el mínimo y el máximo, inicializado a 0

- Eficiencia de este algoritmo es:
  - ``a = 2``, por el número de llamadas recursivas máximas que se ejecutan
  - ``b = n / (n/2) = 2``
  - ``r = 0``, ya que la descomposición y unión es de O(n^``0``)
  - ``2 > 2^0`` ==> ``O(n^logb(a)) = O(n^log2(2)) = O(n)``
- El mejor umbral es:
  - ``n^1 = 2 * (n/2)^0 + n^0`` ; <== Igualas O(clasico)  con O(llamada DyV del clásico)
  - ``n = 2 * 1 + 1``
  - ``n = 3``

```c
void clasico(ivector v, int i, int f, ivector vAux) {
    while ( i <= f ) {
        vAux[2] += v[i];               // Se añade al sumatorio

        if (vAux[0] > v[i]) {          // Si va a ser un nuevo mínimo
            if (vAux[0] != minIni)     // Excepto el valor de inicio, que no cuenta
                vAux[2] += vAux[0];    // Restaura la suma, por ser mínimo incorrecto
            vAux[0] = v[i];            // Actualiza el mínimo
            vAux[2] -= v[i];           // Le quita a la suma el nuevo mínimo

        } else if (vAux[1] < v[i]) {   // Si va a ser un nuevo máximo
            if (vAux[1] != maxIni)     // Excepto el valor de inicio, que no cuenta
                vAux[2] += vAux[1];    // Restaura la suma, por ser máximo incorrecto
            vAux[1] = v[i];            // Actualiza el máximo
            vAux[2] -= v[i];           // Le quita a la suma el nuevo máximo
        }

        ++i;
    }
}

void sumaElementos(ivector v, int i, int f, ivector vAux){
    int t = f-i+1;
    if ( t <= UMBRAL )
        clasico(v,i,f,vAux);
    else {
        int m = i + (t/2);
        sumaElementos(v, i, m - 1,vAux);
        sumaElementos(v, m, f, vAux);
    }
}
```

### Salida de la solución

```
(81, 9, 61, 88, 244, 30, 348, 252, 154, 4, 441, 436, 520, 176, 95, 561, 272, 708, 625, 615)
Suma -MAX -min = 5008
Llamadas al clasico = 8
Cambios del minimo = 3
Cambios del maximo = 8
```

**NOTA:** El vector de 20 elementos se ha creado con:
> ```
>   for (int i = 0; i < n; ++i) {
>     int i_ = i + 13;
>     v[i] = rand() % (i_ * i_);    // La semilla es 65
>   }
> ```