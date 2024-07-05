# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Solución recursiva](#solución-recursiva)
    * [Solución iterativa](#solución-iterativa)
    * [Programa principal](#programa-principal)
    * [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Enunciado

***

Dados un vector ``V[1..n]`` y un numero natural ``k`` entre ``1`` y ``n-1``
diseñar un algoritmo eficiente que transponga los primeros
``k`` elementos de V con los elementos en las ``n-k`` últimas posiciones
sin hacer uso de vector auxiliar

**EJEMPLO**: Si ``n``=10 y ``k``=3

|         |                     |
|---------|---------------------|
| Antes   | A B C D E F G H I J |
| Después | D E F G H I J A B C | 

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

En el peor de los casos sería en O(n). Esto ocurre cuando K=0 o K=N-1

* Eficiencia del DyV:
  * ``a = 1``, ``b = 2``, ``r = 1``
  * ``1 < 2^1`` => ``O(n^r) = O(n)``
  * Umbral:
    * ``n = 2·(n/2) + n^1``
    * ``n = n + c``
    * ``c = 0`` => Significa que el umbral puede ser cualquier valor
    * => Pero empíricamente, no todos son iguales. Tomar el que de mejores resultados


### Solución recursiva

```c
#define K 2 // Desde el elemento 1 (comienza en pos. 0) hasta el N-1
int N;

void intercambiarRango(ivector v, int i, int j, int t) {
    for (int l = 0; l < t; ++l)
        swap(v , i + l, j + l);
}

void transposicionDyV (ivector v, int i, int m, int f) {
    int izq = m-i, der = f-m+1;                 // Elementos a la izquierda y derecha inclusive de m
    if (izq == der) {                           // Nº de elementos a transponer coincide. Solución directa
        intercambiarRango(v, i, m, izq);        // (A) B {(C)} D      =>    C D {A} B
    } else if (izq < der) {                     // Se transponen los  k  primeros con los n-k últimos
        intercambiarRango(v, i, f-izq+1, izq);  // (A) B {c} (D) E    =>    D E {c} A B
        transposicionDyV (v, i, m, f - izq);    // Se transpone el resto:   D E {c},    ya que {A B} está bien situado
    } else { /* izq > der */                    // Se transponen los n-k primeros con los n-k últimos
        intercambiarRango(v, i, m, der);        // (A) B c {(D)} E    =>    D E c {A} B
        transposicionDyV (v, i + der, m, f);    // Se transpone el resto:   D E {c},    ya que {A B} está bien situado
    }                                           // {}: m,   (): i, j
}
```

### Solución iterativa

```c
void transposicionIte(ivector v, int k) {
    int ini = 0,            mid = k+1,          fin = N-1;
    int izq = mid - ini,    der = fin - mid + 1;

    while (izq != der) {
        if (izq < der){
            intercambiarRango(v, ini, fin - izq + 1, izq);
            fin -= izq;
            der -= izq;
        } else {
            intercambiarRango(v, ini, mid, der);
            ini += der;
            izq -= der;
        }
    }
    intercambiarRango(v, ini, mid, izq);
}
```

### Programa principal

```c
int main() {
    int v[] = {97, 98,99,100,101,102,103,104,105,106};
    N = sizeof(v) / sizeof(v[0]);
    transposicionDyV(v, 0, K+1, N - 1);
    //transposicionIte(v,K);
    return 0;
}
```


### Salida de la solución
```
[97     98      99      100     101     102     103     104     105     106]

[104    105     106     100     101     102     103]    97      98      99
[101    102     103     100]    104     105     106     97      98      99
100     [102    103     101]    104     105     106     97      98      99
100     101     [103    102]    104     105     106     97      98      99

[100    101     102     103     104     105     106     97      98      99]
```

