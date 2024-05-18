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

Implementa el algoritmo de ordenación Mergesort sobre vectores de
enteros de tamaño ``n``.
- Implementar en primer lugar el algoritmo de ordenación de inserción,
  para utilizarlo cuando el tamaño del caso sea menor o igual que el umbral.
- ¿Cuál es la eficiencia del algoritmo mergesort implementado?
- ¿Cuál es el mejor umbral?

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

- La primera petición del ejercicio se corresponde con la función ``clasico(..)`` implementado más abajo en esta página.
- Eficiencia de mergesort es:
  - ``a = 2``, por el número de llamadas recursivas máximas que se ejecutan
  - ``b = n / (n/2) = 2``
  - ``r = 1``, ya que la descomposición y unión es de n^``1``
  - ``2 = 2^1`` ==> ``O(n^r·logb(n)) = O(n·log2(n))``
- El mejor umbral es:
  - ``n^2 = 2 * (n/2)^2 + n^1`` ; <== Igualas O(clasico)  con O(llamada antes del clásico)
  - ``n^2 = 2 * (n^2)/4 + n`` ;
  - ``n = n/2 + 1`` ;
  - ``n/2 = 1``
  - ``n = 2``

```c
#define UMBRAL 2

void clasico(ivector v, int i, int f) {
    for (;i <= f;++i)
        for (int j = i+1; j <= f; ++j)
            if (v[i] > v[f]) {
                int aux = v[i];
                v[i] = v[f];
                v[f] = aux;
            }
}

void merge(ivector v, int i, int m, int f, int t) {
    ivector vAux = icreavector(t);                  // Vector auxiliar
    int k = 0, i_ = i, j_ = m;
    while (i_ < m && j_ <= f)   // En vAux se meten ordenados, avanzando 1 de 2 índices x iteración
        vAux[k++] = v[i_] < v[j_] ? v[i_++] : v[j_++];
    while (i_ <  m)
        vAux[k++] = v[i_++];                        // Restante de la mitad izquierda
    while (j_ <= f)  
        vAux[k++] = v[j_++];                        // Restante de la mitad izquierda
    
    for (k=0; k < t; k++)   
        v[i + k] = vAux[k];                         // Sustituye por el vector ya ordenado
    ifreevector(&vAux);
}

void mergesort(ivector v, int i, int f) {
    int t = f - i + 1;              /// tamaño
    if (t <= UMBRAL) 
        clasico(v,i,f);
    else{
        int m = i + (t/2);          /// mitad (incluida)
        mergesort(v,i,m-1);
        mergesort(v,m,f);
        if (v[m-1] >= v[m])         /// Si no está ordenado
            merge(v,i,m,f,t);
    }
}   
```

### Salida de la solución

```
(16, 15, 14, 13, 12, 11, 10, 9, 149, 125, 290, 116, 231, 34, 202, 471),          j: 0,  k: 15
(9, 10, 11, 12, 13, 14, 15, 16, 34, 116, 125, 149, 202, 231, 290, 471),          j: 0,  k: 15
```