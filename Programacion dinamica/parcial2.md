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

Grupo 5 del curso 22-23

```
F(p,q) =
1                                   si p == 0
0                                   si q == 0
1                                   si p == q == 0
F(p-1,q-1)                          si q <= v[p]
max( F(p-1,q-v[p]) +1 , F(p,q-1) )  e.o.c

El vector v debe tener p+1 elementos
```

# Solución
***

[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
int max(int a, int b) {
    return a < b ? b : a;
}

int funcion(int p, int q, ivector vector) {
    if (p==q && !p)  return 1; //Ambos coinciden y son 0
    if (!p)          return 1; //p es 0
    if (!q)          return 0; //q es 0

    int i, j;
    imatriz2d mtx = icreamatriz2d(p+1,q+1);
    for (i = 0; i <= p ; ++i)  mtx[i][0] = 1;
    for (i = 1; i <= q ; ++i)  mtx[0][i] = 0;

    for (i = 1; i <= p; ++i)
        for (j = 1; j <= q; ++j) {
            mtx[i][j] = mtx[i-1][j-1];
            if (j >= vector[i])
                mtx[i][j] = max(mtx[i-1][j-vector[i]] + 1,  mtx[i][j-1]);
        }

    i = mtx[p][q];
    ifreematriz2d(&mtx);
    return i;
}
```


