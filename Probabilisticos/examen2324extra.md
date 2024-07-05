# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Problema 2 de la extraordinaria 23-24](#problema-2-de-la-extraordinaria-23-24)
* [Solución](#solución)
* [Salida de la solución](#salida-de-la-solución)
<!-- TOC -->

# Problema 2 de la extraordinaria 23-24

***

Implementar el problema del laberinto con un algoritmo Las Vegas

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
int generarX(int x, int i){
    if (i == 1) return x+1;
    if (i == 2) return x-1;
    return x;
}

int generarY(int y, int i){
    if (i == 3) return y + 1;
    if (i == 4) return y - 1;
    return y;
}

int factible(imatriz2d mtx, int i, int j){
    return mtx[i][j] == LIBRE || mtx[i][j] == EXIT;
}

int laberintoVegas(imatriz2d lab, int x, int y){
    int x_, y_, mx, my;
    int hayLibre, encontrado=0;
    do{
        hayLibre = 0;
        for (int i = 1; i <= 4; ++i) {
            x_ = generarX(x,i), y_ = generarY(y,i);     // x_,y_: para la iteración de la matriz
            if (encontrado==0 && factible(lab,x_,y_)) {
                hayLibre++;
                if (1 + rand()%hayLibre == 1) 
                    mx = x_, my = y_,                   // mx,my: para la memorización
                    encontrado = lab[mx][my] == EXIT;   // Si es EXIT, ha encontrado salida
            }
        }

        if (hayLibre > 0)
            x = mx, y=my,                               // X e Y no cambian hasta considerar los movs. posibles
            lab[x][y] = VISIT;

    }while(hayLibre > 0 && encontrado == 0);
    return hayLibre > 0;
}
```

# Salida de la solución

El laberinto aquí usado es el de la [resolución de la práctica 4 de VA](../Vuelta%20atras/relacion4.md), 
con el fin de poder comparar también los algoritmos implicados.

Es recomendable partir de este último para hacer este problema.

Lo más usual es usar un LV cuando existen un montón de soluciones disponibles

A continuación, una ejecución que ``SÍ ha conseguido llegar a una solución``

```
ORIGINAL:       ======  ======
1:      M   M   M   M   M   M   M   M   M   M
2:      M               e                   M
3:      M       M   M   M   M   M   M       M
4:      M               M                   M
5:      M                       M   M   M   M
6:      M                           M   x   M
7:      M           M   M   M   M           M
8:      M       M               M       M   M
9:      M               M               M   M
10:     M   M   M   M   M   M   M   M   M   M

LAS VEGAS:      ======  ======
1:      M   M   M   M   M   M   M   M   M   M
2:      M               e  [-]  -   -   -   M
3:      M       M   M   M   M   M   M   -   M
4:      M               M   -   -   -   -   M
5:      M   -   -   -   -   -   M   M   M   M
6:      M   -                       M  [-]  M
7:      M   -       M   M   M   M   -   -   M
8:      M   -   M   -   -   -   M   -   M   M
9:      M   -   -   -   M   -   -   -   M   M
10:     M   M   M   M   M   M   M   M   M   M
```

La ruta que ha tomado esta última solución ha sido la siguiente:
```
LAS VEGAS:   ======  ======
1:      M   M   M   M   M   M   M   M   M   M
2:      M               e  [0]  1   2   3   M
3:      M       M   M   M   M   M   M   4   M
4:      M               M   8   7   6   5   M
5:      M   13  12  11  10  9   M   M   M   M
6:      M   14                      M  [29] M
7:      M   15      M   M   M   M   27  28  M
8:      M   16  M   20  21  22  M   26  M   M
9:      M   17  18  19  M   23  24  25  M   M
10:     M   M   M   M   M   M   M   M   M   M
```

A continuación, una ejecución que ``NO ha conseguido llegar a una solución``
```
LAS VEGAS:   ======  ======
1:      M    M    M    M    M    M    M    M    M    M
2:      M    2    1   [0]   e   [20]  19   18   17   M
3:      M    3    M    M    M    M    M    M    16   M
4:      M    4    5    6    M    12   13   14   15   M
5:      M              7         11   M    M    M    M
6:      M              8    9    10        M    x    M
7:      M              M    M    M    M              M
8:      M         M                   M         M    M
9:      M                   M                   M    M
10:     M    M    M    M    M    M    M    M    M    M
```