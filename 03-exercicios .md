# Lista de exercícios - Loops 
1 - Crie uma função que imprima todos os números entre 1 até 100 em sequencia. 
Ex: "1, 2, 3, ..., 100"

##
2 - Crie uma função que imprima todos os números pares entre 1 até 100 em sequencia. 
Ex: "2, 4, 6, ..., 100"

##
3 - Crie uma função que imprima todos os números impares entre 1 até 100 em sequencia. 
Ex: "1, 3, 5, ..., 99"

##
4 - Crie uma função que receba um parâmetro do tipo `string` que pode ser "impar", "par" ou "todos" e imprima todos os números impares, pares ou todos entre 1 até 100 
em sequencia dependendo do que estiver no parâmetro da função. 
Ex: 
``` kotlin
fun main() {
   imprimeSequencia("impares")
}

//saída: 
1, 3, 5, ..., 99
```

## 
5 - Crie uma função que recebe um parâmetro do tipo `Array` de inteiros (`Array<Int>`) e que retorna o maior número contido neste array.
``` kotlin
fun main() {
  val numeros: Array<Int> = arrayOf(100,20,300,40,55)
  val maior = encontraOMaior(numeros)
  print(maior)
}

//saída: 
300
```

## 
6 - Crie uma função que recebe um parâmetro do tipo `Array` de inteiros (`Array<Int>`) e que retorna a soma de todos os número contido neste array.
``` kotlin
fun main() {
  val numeros: Array<Int> = arrayOf(100,20,300,40,55)
  val soma = somaTotal(numeros)
  print(soma)
}

//saída: 
515
```
##
7 - Crie uma função que recebe um parâmetro do tipo `Array` de inteiros (`Array<Int>`) e que calcule e mostra a 
quantidade de números pares e a quantidade de números ímpares contidos no array.
``` kotlin
fun main() {
  val numeros: Array<Int> = arrayOf(100,20,300,40,55)
  contadorDeParesEImpares(numeros)
}

//saída: 
4 números pares e 1 número ímpar.
```
