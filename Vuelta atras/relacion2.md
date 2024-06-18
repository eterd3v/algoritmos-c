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

Dado un conjunto de números enteros X= {x1, x2, ..., xn}, encontrar todos los
subconjuntos que sumen una cantidad ``M``. 

**Ejemplo**: 

si ``X = {1, 2, 3, 4, 5}`` las diferentes formas de sumar 6 son:

``{1, 2, 3}``, ``{2, 4}``, ``{1, 5}``

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
#define M 8
#define TAM 6
#define SEMILLA 0

int formas = 0, llamadas=0, iteraciones=0;

void mostrarSeq(ivector v, ivector comb) {
    printf("%i:\t",++formas);

    for (int i = 0; i < TAM; ++i)
        if (comb[i])
            printf("%i, ", v[i]);

    printf("\n");
}

void va(ivector v, ivector  comb, int suma, int i) {
    if (suma == M) {
        mostrarSeq(v,comb);
    } else {
        while (i < TAM) {                       // Poda: No entra si i >= TAM; ya ha sido revisado (combinatorio)
            if (!comb[i] && suma + v[i] <= M){  // Poda: La suma del elemento i, aún no usado, es factible
                comb[i] = 1;                    // Marcaje: Marcamos como en uso (1) el elemento i

                llamadas++;
                va(v, comb, suma+v[i], i+1);    // Llamada recursiva

                comb[i] = 0;                    // Marcaje: Marcamos como libre (0) o el elemento i
            }
            ++i;
            ++iteraciones;
        }
    }
}
```


### Salida de la solución

```
(3      4       5       2       6       1)

---FORMAS---
1:      3, 4, 1,
2:      3, 5,
3:      5, 2, 1,
4:      2, 6,

Hay 4 formas para dar 8
Llamadas recursivas: 21
Iteraciones totales: 31
```

**NOTA**: Con fuerza bruta serían 6! = 720 combinaciones. 
La mejora, en términos de iteraciones, es de un 95,694%