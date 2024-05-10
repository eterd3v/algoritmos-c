# Índice del problema

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado-)
    * [Problema 2 de la ordinaria 22-23](#problema-2-de-la-ordinaria-22-23)
* [Solución](#solución)
  * [Algoritmo del problema](#algoritmo-del-problema)
  * [Salida de la solución](#salida-de-la-solución)
  * [Programa principal (opcional)](#programa-principal-opcional)
  * [Funciones auxiliares (opcional)](#funciones-auxiliares-opcional)
<!-- TOC -->


# Enunciado 

***

### Problema 2 de la ordinaria 22-23

Se desea hacer traducciones de textos entre varios idiomas. Se dispone de varios
traductores automáticos que permiten la traducción bidireccional entre dos idiomas. 
No se dispone de diccionarios para cada par de idiomas por lo que es preciso encadenar
varias traducciones. 

Dados ``N`` idiomas y ``M`` traductores, implementa un algoritmo basado
en Programación Dinámica que devuelva la cadena de traducciones de longitud mínima
entre dos idiomas dados, si es posible.


**Ejemplo**: Traducir del español al griego disponiendo de los siguientes
diccionarios: español-inglés, inglés-francés, francés-italiano, italiano-griego,
francés-griego. 

La **solución** sería: español-inglés, inglés-francés, francés-griego,
realizando 3 traducciones.

# Solución

***

[Este problema](#enunciado-) se puede resolver en C de la siguiente forma:

Algoritmo de floyd. En el que los nodos son los idiomas y las traducciones son las aristas. El peso de una
arista (i, j) es el siguiente:
- **arista(i,j) = 1**, si existe traducción directa entre el lenguaje i y el lenguaje j.
- **arista(i,j) = ∞**, en caso contrario.

De esta forma obtenemos “la longitud del camino mínimo” de la traducción entre cada par de lenguajes.
La complejidad del algoritmo: O(n^3).

## Algoritmo del problema

```c
int min(int a, int b) {
    return a < b ? a : b;
}

void buscarTraduccion(imatriz2d traducciones, imatriz2d S) {
    int i,j,k;
    for (i=0; i < N; ++i)
        for (j=0; j < N; ++j)
            S[i][j] = traducciones[i][j];

    for (k=0; k < N; ++k)           // Se busca traducción entre N idiomas intermedios (nodos)
        for (i=0; i < N; ++i)
            for (j=0; j < N; ++j)
                S[i][j] = min(S[i][j], S[i][k] + S[k][j] ); // Las traducciones son las aristas
}
```

## Salida de la solución

```
        DICCIONARIOS DISPONIBLES
        ESP     ING     ITA     FRA     GRI
ESP     -       x       -       -       -
ING     -       -       x       -       -
ITA     -       -       -       x       x
FRA     -       -       -       -       x
GRI     -       -       -       -       -

        TRADUCCIONES MINIMAS ENTRE IDIOMAS
        ESP     ING     ITA     FRA     GRI
ESP     -       1       2       3       3
ING     -       -       1       2       2
ITA     -       -       -       1       1
FRA     -       -       -       -       1
GRI     -       -       -       -       -
```

## Programa principal (opcional)

**NO HAY QUE IMPLEMENTAR EN EL EXAMEN ESTO DE AQUÍ**

```c
#include <stdio.h>
#include "imatriz2d.h"

#define INF 9999
#define N 5 // NODOS: ESP, ING, FRA, ITA, GRI
//#define M 5 // ARISTAS: ESP-ING, ING-FRA, ... (NO SE LLEGA A USAR)

int main() {
    imatriz2d diccionarios = icreamatriz2d(N, N);
    imatriz2d traduMinimas = icreamatriz2d(N, N);

    for (int i = 0; i < N; ++i)
        for (int j = 0; j < M; ++j)
            diccionarios[i][j] = INF;   // Suponemos que no hay traducción directa entre idiomas
            // traduMinimas[i][j] = 0;  // NO INICIALIZAR. El algortimo ya te lo sobreescribe

    // Estas son las M aristas. Valen 1 si existe una traducción entre idioma i, e idioma j
    diccionarios[0][1] = 1; // ESP-ING
    diccionarios[1][2] = 1; // ING-FRA
    diccionarios[2][3] = 1; // FRA-ITA
    diccionarios[2][4] = 1; // FRA-GRI
    diccionarios[3][4] = 1; // ITA-GRI
    // La matriz no es simétrica: "No hay dicc. para cada par de idiomas", "encadenar traducciones"

    printf("\n\tDICCIONARIOS DISPONIBLES\n");
    mostrarMatriz(diccionarios,0);

    buscarTraduccion(diccionarios, traduMinimas);

    printf("\n\tTRADUCCIONES MINIMAS ENTRE IDIOMAS\n");
    mostrarMatriz(traduMinimas,1);                // Mostrar resultado por pantalla

    ifreematriz2d(&diccionarios);
    ifreematriz2d(&traduMinimas);
    return 0;
}
```

## Funciones auxiliares (opcional)

**NO HAY QUE IMPLEMENTAR EN EL EXAMEN ESTO DE AQUÍ**

```c
void idiom(int i, int saltoLinea) {
    if (saltoLinea)
        printf("\n");
    switch (i) {
        case 0:     printf("ESP\t"); break;
        case 1:     printf("ING\t"); break;
        case 3:     printf("FRA\t"); break;
        case 2:     printf("ITA\t"); break;
        case 4:     printf("GRI\t"); break;
        default:    break;
    }
}

void mostrarMatriz(imatriz2d mtx, int numerico) {
    printf("\t");
    for (int i=0; i < N; ++i)
        idiom(i,0);                   // Muestro primera fila con los idiomas
    for (int i=0, j; i < N; ++i) {
        idiom(i,1);
        for (j=0; j < N; ++j)
            if (numerico)   printf(mtx[i][j] < INF ? "%i\t" : "-\t", mtx[i][j]);
            else            printf(mtx[i][j]%INF!=0 ? "x\t" : "-\t" ); // Valores nulos (+INF, -INF) y normales
    }
    printf("\n");
}
```

