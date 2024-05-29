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

Sean ``X`` e ``Y`` dos vectores de tamaño ``n`` potencia de 2, ordenados de
forma creciente. Implementar un algoritmo DyV para calcular el
elemento que ocuparía la posición n que resulta de la unión ordenada
de los vectores ``X`` e ``Y``

**NOTA: Para resolver el problema NO se pueden unir los vectores, ordenar
el vector resultante y seleccionar el elemento de la posición ``n``**

> - Caso base: ``n = 2``
>    - Si el segundo elemento de un vector es menor que el primero del
>      otro, se devuelve ese elemento:
>      - X={3, ``5``}; Y={``6``, 9} => 5 
>      - X={``7``, 8}; Y={2, ``4``} => 4 
>    - En caso contrario se devuelve el mayor de los dos primeros:
>      - X={``3``, 8}; Y={``7``, 9} => 7
>      - X={``5``, 6}; Y={``1``, 7} => 5

> - Caso general: ``n > 2``
>   - ``tam = supX - infX + 1;`` 
>   - Se comparan los elementos de la “segunda” posición de ambos 
>     vectores (si el ``tam = 4`` → 2º, si el ``tam = 8`` → 4º, …)
>     - Si coinciden, se devuelve ese elemento:
>       - X={3, ``4``, 7, 8} Y={1, ``4``, 6, 9} => 4
>     - Si no coinciden, se comprueba cuál es el menor de los 2.
>       - Si es el del vector X, se busca recursivamente en la 
>         segunda parte de X y en la primera de Y
>         - X={2, 3, ``4, 8``} Y={``1, 5``, 6, 9} 
>       - En caso contrario, se busca recursivamente en la segunda 
>         parte de Y y en la primera de X
>         - X={``2, 3``, 4, 8} Y={1, 2, ``6, 9``}

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
int centroClasico(ivector v1, int inf1, int sup1, ivector v2, int inf2, int sup2){
    // El supremo de un vector es mayor al ínfimo al del otro vector
    if(v1[sup1] < v2[inf2])     return v1[sup1];
    if(v2[sup2] < v1[inf1])     return v2[sup2];
    // En cualquier otro caso, se devuelve el mayor de los ínfimos de ambos vectores
    return v1[inf1] > v2[inf2] ? v1[inf1] : v2[inf2];
    // Siendo el último caso, no hacen falta más comprobaciones
}

int centro(ivector v1, int inf1, int sup1, ivector v2, int inf2, int sup2) {
    int tam1 = sup1-inf1+1;
    int tam2 = sup2-inf2+1;

    if(tam1 <= UMBRAL && tam2 <= UMBRAL)    // Suficientemente pequeño
        return centroClasico(v1,inf1,sup1,v2,inf2,sup2);
    else{
        int m1 = inf1 + tam1/2, m2 = inf2 + tam2/2;

        // Se divide la búsqueda del centro recursivamente según las posiciones de infimo-supremo
        int centroIzq = centro(v1, inf1, m1-1, v1, m1, sup1);
        int centroDer = centro(v2, inf2, m2-1, v2, m2, sup2);

        // Clasificamos qué tipo puede ser y lo retornamos hacia la llamada anterior
        if (centroIzq > centroDer)          return centro(v1,inf1,m1-1, v2,m2, sup2);
        else if (centroIzq < centroDer)     return centro(v1,m1, sup1,  v2,inf2,m2-1);
        return centroIzq;                   // centroIzq o centroDer, cualquiera de los dos vale
    }
}
```

### Salida de la solución

```
==================== ANTES ====================
(20, 24, 76, 146, 156, 180, 208, 248),
(31, 42, 48, 64, 136, 168, 172, 205),

=================== CENTRO ===================
El elemento central de la union seria 136

==================== COMPROBACION ====================
20, 24, 31, 42, 48, 64, 76, (136), 146, 156, 168, 172, 180, 205, 208, 248,
```