[relacion3.md](relacion4.md)# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Enunciado

***

Dado un conjunto ``C`` de ``n`` elementos (no necesariamente ordenables) se
dice que un elemento ``x`` es mayoritario en ``C`` cuando el número de veces que
aparece ``x`` en ``C`` es ``estrictamente mayor que n/2``. Dado un conjunto de elementos
se desea saber si un conjunto contiene un elemento mayoritario y devuelva tal
elemento cuando exista.

* ¿Cómo sería el algoritmo ``clásico`` que se podría diseñar para resolver
este problema? ¿Cuál sería su eficiencia? 
* Diseñar e implementar un algoritmo basado en la técnica Divide y
Vencerás para solucionar este problema. ¿Cuál sería su eficiencia?
# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

- Eficiencia del clásico: ``O(n^2)``
- Eficiencia del DyV: ``O(n·log2(n))``
- Umbral: 
  - ``n^2 = 2 * (n/2)^2 + n^1``
  - ``n^2 = (n^2)/2 + n``
  - ``n = n/2 + 1``
  - ``n/2 = 1``
  - ``n = 2``

```c
int clasico(ivector v, int i, int f, int t) {
    if (t > 2){
        int cant, j;
        for (; i <= f; ++i){                            // Por cada elemento
            cant = 0;
            for (j = i; j <= f; ++j)                    // Se cuenta cuantos hay dentro del vector
                if (v[j] == v[i])
                    ++cant;
            if (2*cant > t)
                return v[i];                            // Si es mayoritario (solo hay uno), se corta el bucle
        }
        return INVALIDO;                                // No hay mayoritario en el vector
    }
    return v[i] == v[f] ? v[i] : INVALIDO;              // Con 2 elementos o menos, basta mirar si son iguales
}

int mayoritario(ivector v, int ini, int fin) {          // Eficiencia O(n·log2(n))
    int tam = fin-ini+1;
    if ( tam <= UMBRAL ) {
        return clasico(v,ini,fin,tam);
    } else {                                            // a=2, b=2, r=1
        int mid   = ini + (tam/2);
        int may1  = mayoritario(v,ini,mid-1);           // Mayoritario de la mitad izquierda
        int may2  = mayoritario(v,mid,fin);             // Mayoritario de la mitad derecha

        if (may1 == may2) return may1;                  // Si son iguales, devolver cualquiera, pues
                                                        // valdrá invalido o may1 o may2 en cualquier caso
        int cont1 = 0, cont2 = 0;
        while (ini <= fin) {                            // Se recorre n veces en el peor de los casos
            if (v[ini  ] == may1) ++cont1;
            if (v[ini++] == may2) ++cont2;              // ini++ para avanzar el bucle tras la consulta del if
        }

        if ( 2 * cont1 > tam ) return may1;             // Como solo puede haber un  mayoritario, se pregunta a los 2 sin problema
        if ( 2 * cont2 > tam ) return may2;
        return INVALIDO;                                // No existe mayoritario, aún teniendo en cuenta las mitades
    }
}
```

### Salida de la solución

```
(2, 3, 2, 1, 3, 1, 1, 1, 2, 1, 1, 0, 2, 1, 1, 1, 1, 1, 1, 0, 0, 3, 3, 2, 1, 3, 1, 1, 1, 2)
El mayoritario es 1 (aparece 16 de 30 veces)
```