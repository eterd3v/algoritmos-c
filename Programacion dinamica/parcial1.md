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

Grupo 5 del curso 23-24

```
F(p,q) =
1                                       si p == 0
0                                       si q == 0
1                                       si p == q == 0
F(p,q-1)                                si q <= v[p]
max( F(p,q-v[p]-1) + 1 , F(p-1,q) )     e.o.c

El vector v debe tener p+1 elementos
```


# Solución

***

[Este problema](#enunciado) se puede resolver en C de la siguiente forma:


```c
int max(int a, int b) {
    return a < b ? b : a;
}

int funcion(int p, int q, ivector v) {
    if (!p && !q) return 1;
    if (!p) return 1;
    if (!q) return 0;

    imatriz2d mtx = icreamatriz2d(p+1,q+1);

    int i,j ;
    for (i = 0; i <= q; ++i)    mtx[0][i] = 0;
    for (i = 0; i <= p; ++i)    mtx[i][0] = 1;      // Aseguro que [0][0] = 1

    for (i = 1; i <= p; ++i)
        for (j = 1; j <= q; ++j) {
            mtx[i][j] = mtx[i][j-1];                // Supongo este valor por defecto
            if (j > v[i])
                mtx[i][j] = max(mtx[i][j-1-v[i]] + 1, mtx[i-1][j]);
        }

    i = mtx[p][q];
    ifreematriz2d(&mtx);
    return i;
```


