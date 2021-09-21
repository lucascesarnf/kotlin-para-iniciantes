# Sintaxe básica

> Para uma melhor fixação dos conceitos aqui descritos, recomendamos que todos os tópicos sejam testados e alterados no [Kotlin Playground](https://play.kotlinlang.org/). 

## Definição de pacote e importações
As especificaçoes dos pacotes precisam estar no topo do arquivo.
```kotlin
package my.demo

import kotlin.text.*

// ...
```
## Ponto de partida do código
A função `main` serve como o ponto de partida para a execução do programa. Em geral, ela controla a execução direcionando as chamadas para outras funções no programa.
```kotlin
fun main() {
    println("Hello world!")
}
```
## Imprimir na saída padrão 
A função `print`, basicamente, exibe mensagens na tela de saída. 
```kotlin
print("Hello ")
print("world!")

/*
saída: 
Hello world!
*/
```
A função `println`, faz o mesmo que a função print, porém, pula uma linha logo após o texto. 
```kotlin
println("Hello ")
println("world!")

/*
saída: 
Hello 
world!
*/
```
## Funções
Uma função nada mais é do que uma sub-rotina usada em um programa.
Neste exemplo temos uma função que recebe dois parâmetros do tipo `Int` e retorna um tipo `Int`:

> Entenda melhor sobre os [tipos](02-tipos-de-dados-primitivos.md) em Kotlin
```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}

fun main() {
    print("sum of 3 and 5 is: ")
    println(sum(3, 5))
}

/*
saída: 
sum of 3 and 5 is: 8
*/
```
O corpo de uma função pode ser uma expressão, que retorna o tipo através de uma [inferência de tipo](https://kotlinlang.org/spec/type-inference.html).

```kotlin
fun sum(a: Int, b: Int) = a + b


fun main() {
    println("sum of 19 and 23 is ${sum(19, 23)}")
}

/*
saída: 
sum of 19 and 23 is 42
*/
```

Uma função pode não retornar nenhum valor significativo.

```kotlin

fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}

fun main() {
    printSum(-1, 8)
}

/*
saída: 
sum of -1 and 8 is 7
*/
```

Você ainda pode omitir o retorno do tipo`Unit` .

```kotlin
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}

fun main() {
    printSum(-1, 8)
}

/*
saída: 
sum of -1 and 8 is 7
*/
```

## Variáveis
Variáveis locais que são definidas apenas para leitura são declaradas utilizando a palavra `val`. Variáveis exclusivas para leitura(Read-only) não poderão sofrer alterações após sua declaração. 

```kotlin
fun main() {
    val a: Int = 1  // atribuição imediata
    val b = 2   // tipo `Int` inferido
    val c: Int  // é necessário declarar o tipo da varoável quando nenhum inicializador é fornecido
    c = 3       // atribuiçao feita posteriormente
    println("a = $a, b = $b, c = $c")
}
/*
saída: 
a = 1, b = 2, c = 3
*/
```
Variáveis que podem ser alteradas durante a execução do programa utilizam a palavra `var`.

```kotlin
fun main() {
    var x = 5 // `Int` type is inferred
    x += 1
    println("x = $x")
}
/*
saída: 
x = 6
*/
```
Você pode declarar variáveis de escopo superior.
```kotlin
val PI = 3.14
var x = 0

fun incrementX() { 
    x += 1 
}

fun main() {
    println("x = $x; PI = $PI")
    incrementX()
    println("incrementX()")
    println("x = $x; PI = $PI")
}

/*
saída: 
x = 0; PI = 3.14
incrementX()
x = 1; PI = 3.14
*/
```
