# Índice del problema

***

**Para volver a la lista haz clic [aquí](./Index.md)**

<!-- TOC -->
* [Índice del problema](#índice-del-problema)
* [Enunciado](#enunciado)
* [Solución](#solución)
    * [Salida de la solución](#salida-de-la-solución)
    * [Funciones auxiliares](#funciones-auxiliares)
<!-- TOC -->

# Enunciado

***

Un documento secreto está codificado con el abecedario de 8 símbolos ∑ = {1,
2, 3, A, E, I, O, U}. Se sabe que en él se encuentra encriptada la información
buscada en palabras de longitud no mayor de 5 que contienen exactamente 2
vocales.

Escribe un algoritmo de vuelta Atrás que genere el listado del vocabulario, es
decir, de todas las palabras que podrían encontrarse en dicho documento

**Guía**: Debe observarse que la palabra “AE” es solución. También lo son “AE1”,
“AE123” y “AE232”.

# Solución
[Este problema](#enunciado) se puede resolver en C de la siguiente forma:

```c
#define SEMILLA 14
#define N 5
#define LIBRE -1
#define VOCALES 2
#define VALORES 8

void va(ivector v, int paso, int vocales) {
    if (vocales == VOCALES){
        mostrarC(v);
    }else if (paso+1 <= N) {
        if (vocales + 1 <= VOCALES) {
            for (int i=1, aux; i <= VALORES; ++i) {
                aux = i > 3 ? 1 : 0;
                v[paso] = i;
                va(v,paso+1,vocales+aux);
                v[paso] = LIBRE;
            }

        }
    }
}
```


### Salida de la solución

```
1       1       1       A       A
1       1       1       A       E
1       1       1       A       I
1       1       1       A       O
1       1       1       A       U
1       1       1       E       A
1       1       1       E       E
1       1       1       E       I

...

U       3       I
U       3       O
U       3       U
U       A
U       E
U       I
U       O
U       U

Hay 3550 soluciones con 6648 llamadas
```

### Funciones auxiliares

```c
void salidaPantalla(int a){
    if ( a > 3 ) {
        switch (a) {
            case 4: printf("A"); break;
            case 5: printf("E"); break;
            case 6: printf("I"); break;
            case 7: printf("O"); break;
            case 8: printf("U"); break;
            default: break;
        }
    }else
        printf("%i",a);
}

void mostrarC(ivector v){
    printf("\n");
    for (int i = 0; i < N; ++i) {
        if (v[i]==LIBRE)    printf(" ");
        else salidaPantalla(v[i]);
        printf("\t");
    }
}
```