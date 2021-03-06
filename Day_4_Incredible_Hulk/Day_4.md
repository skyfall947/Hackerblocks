
# [Day 004 Incredible Hulk](https://hack.codingblocks.com/app/practice/6/1038/problem)

Descripción del problema:
El planeta Tierra está bajo la amenaza de los extraterrestres del espacio exterior y el equipo de los Vengadores está ocupado luchando contra ellos. Mientras tanto, Hulk tiene que derrotar a un enemigo que está a N pasos por encima del nivel en el que está parado (asuma el nivel de Hulk como el nivel 0). Hulk, debido a su increíble capacidad de salto, puede saltar en **potencias de dos**. Para derrotar al enemigo lo más rápido posible, tiene que alcanzar el enésimo paso con el mínimo de movimientos posibles. Ayuda a Hulk a encontrar lo mismo y contribuye a salvar a la Tierra.

#### Formato de entrada 

La primera línea contiene el número de casos de prueba T. 

Los siguientes casos de prueba T: La primera línea de cada caso de prueba contiene un número N. 

#### Restricciones

1 <= T <= 10  
1 <= N <= 10⁵
#### Formato de salida 

Líneas T de salida, que contienen el número mínimo de movimientos requeridos por Hulk para alcanzar el enésimo paso.

#### Entrada de muestra
> 3

>3

>4

>5
#### Salida de muestra
> 2

> 1

> 2
#### Explicación

Deje que los pasos totales sean n, encuentre el número entero más cercano que sea de potencia 2 y menor que n. deje que se sepa que los pasos restantes para cubrir son n-k y resultado = 1 + pasos min para los pasos restantes (n-k).



### Enfoque del problema
Para poder solucionar este ejercicio se utilizará un enfoque de **Operar a nivel de bits Bitwise**. 
![Geekforgeeks](https://www.geeksforgeeks.org/wp-content/uploads/Operators-In-C.png)
##### Count set bits in an integer
Un ejemplo de su utilidad es:

Dado un numero N, encontrar el número de bits "encendidos"(1) en la representación binaria del mismo:
![Geekforgeeks](https://www.geeksforgeeks.org/wp-content/uploads/setbit.png)
####  Primer Método:
Este metodo se basa en que si nosotros aplicamos la operacion de AND entre N y 1 podriamos saber que el numero al final de N es `1 o 0`  y guardamos esa operacion en nuestro contador, luego necesitamos desplazar en 1bit nuestro N y actualizar su valor: 
```
Aplicamos la operacion AND:
12 = 1100
 1 = 0001 AND
 -------------
 1 = 0001
```
```
Dezplazamos en 1 bit
12 = 1100>>1 = 6 -> El nuevo valor de N
```
```c++
#include <iostream>
using namespace std;
int main(){
    int N;
    int c = 0;
    scanf("%d", &N);
    while(N>0){
        c += (N&1); // bitwise operation
        N = N>>1;   // bitwise operation 
    }    
    printf("Cantidad de 1's es: %d\n", c);
    // Para n=15: Cantidad de 1's es: 4
    return 0;
}
```
#### Analisis de la complejidad:

Según el primer dígito de la izquierda, por ejemplo:  `16 = 10000` tendremos que iterar hasta que el numero binario se convierta en `0` seria 5 veces. Entonces dado N tendríamos que iterar máximo `O(log2(n))` en la practica seria: `log2(n) +1` bits.  
> La razón de por que es logaritmo de base 2 de N es porque los binarios son potencias de 2.

Por ejemplo: 

Para n = 7: `log2(7) = 2 + 1 = 3`  iteraciones.

Para n = 8: `log2(8) = 3 + 1 = 4`  iteraciones.

Para n = 15: `log2(15) = 3 + 1 = 4`  iteraciones.

Para n = 16: `log2(16) = 4 + 1 = 5 `  iteraciones.

En c++ un `int` puede llegar a tener 2 · 10⁹ entonces seria `log2(2·10⁹) = 30` iteraciones.
En c++ un `long long` puede llegar a tener 9 · 10¹⁸ entonces seria `log2(9·10¹⁸) = 62` iteraciones.
Esto a pesar de ser bastante optimo frente a un *Naive approach* para numeros mas grandes puede llegar a ser algo lento.
### Segundo Método
En este metodo realizaremos solo una operacion por cada bit *encendido* (1) realizando la siguiente operacion:
```
n = 9 -> 1001
n = 8 -> 1000 AND
-----------------
n = 8 -> 1000 El nuevo valor de N en la primera iteracion.
n = 7 -> 0111 AND
-----------------
n = 0  -> 0000 El nuevo valor de N en la segunda iteracion.
```
Como se puede observar solo debemos acumular la cantidad de veces que se repite esto hasta que `N = 0`
```c++
#include <iostream>
using namespace std;
int main(){
    int N;
    int c = 0;
    scanf("%d", &N);
    while(N>0){
        N = N & (N - 1);
        c++;
    }    
    printf("Cantidad de 1's es: %d\n", c);
    return 0;
}
```
#### Análisis de complejidad

La operación AND que realizamos elimina de derecha a izquierda los conjuntos de bits, por lo tanto la complejidad de este algoritmo es: `O(numero de conjunto de bits)` 
### Tercer Método
Es siguiente método es llamado: [**__builtin__ popcount(x)**](https://www.geeksforgeeks.org/builtin-functions-gcc-compiler/) 
```c++
int main(){
    int N;
    cin>> N;
    cout<<"La cantidad de 1's es: "<<__builtin_popcount(N)<<endl;
} 
```


## Solución al problema:
Como vimos el ejercicio nos pide la mínima cantidad de salto para que Hulk llegue a su objetivo, veamos por ejemplo para N = 13 su representación binaria es: `1101` del mismo podemos ver que existen 3 conjuntos: `1, 101, 1101` cada `1` representa una potencia de `2` es decir `2⁰, 2¹, 2², 2³, 2⁴.....` entonces si aplicamos cualquiera de los metodos usados arriba tendremos la solucion:
```c++
#include <iostream>
using namespace std;
int main(){
    int N, T;
    cin>> T;
    while(T--){
        int c = 0;
        cin>>N;
        while(N>0){
            N = N & (N - 1);
            c++;
        }    
        cout<<c<<endl;
    }
    return 0;
}

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbODkzMjk4MjQ2LDEzNzcxMjAzODEsMTg1Nj
MwOTA0MCwxMzcyNDk1Nzk3LC0xMTU2MTY1OTMwLDE3MzAxMTYz
OTAsLTcxOTQ5OTEwMywtMTYyMTUzMzkyMCw2NTUzODgwMjQsLT
gwNjY2ODA1MywtMjEzODI4OTEyNiwxNjUyMjQxNDA3LC00NTAx
ODgyMCwtMTc1NTg4MTg3OSwxNDk0MDY0MTY2LDEzMTk4ODQwOD
gsLTE1NzA1Mzc2NTYsLTEyMDY5MDEyOTUsNzMwOTk4MTE2XX0=

-->