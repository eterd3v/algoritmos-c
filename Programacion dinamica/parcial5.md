# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
<!-- TOC -->

# Enunciado

Se desconoce el enunciado original. Del curso 21-22.

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
int max(int a, int b) {
    return a < b ? b : a;
}

int funcion(int n, int m) {
    if(n==0 && m==0)    return 2;
    if(m==0)            return 1;
    if(n==0)            return 0;

    imatriz2d mat = icreamatriz2d(n+1, m + 1);
    int i, j;
                                                // Casos base
    for(i=0; i <= n; i++) mat[i][0]=0;          // Primera columna
    for(j=0; j <= m; j++) mat[0][j]=1;          // Primera fila
    mat[0][0]=2;                                // Asignar el valor especial cuando m=0 y n=0

    for (i=1; i <= n; i++)
        for (j=1; j <= m; j++) {
            mat[i][j] = mat[i][j-1];
            if(i > j)
                mat[i][j] = max( mat[i][j-1], mat[i-1][j] + 2 );
        }

    i = mat[n][m];                              // Reuso la variable i
    ifreematriz2d(&mat);
    return i;
}
```

