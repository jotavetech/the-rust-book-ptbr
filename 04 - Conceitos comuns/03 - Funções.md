# Funções

Já vimos a função mais importante do Rust: a ```main``` que é a porta de entrada para todos os programas em Rust. Também já vimos a palavra ```fn``` que nos permite criar novas funções.

Rust usa snake_case em nomes de funções e de variáveis:

```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```

Definimos uma nova função em Rust usando ```fn``` e () depois do nome dela, as {} indicam o corpo da função.

O Rust não se importa com a ordem de declaração da função, então você pode chamar ela antes de declarar, só tem que estar no escopo acessível. 

### Parâmetros

Nós podemos definir funções com parâmetros, que são variáveis especiais que fazem par te da função, quando ela receber parâmetros podemos enviar os valores deles na chamada da função, chamamos os valores passados de argumentos:

```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```

Você deve definir o tipo de cada parâmetro na hora de criar a função, ai definimos que x é do tipo ```i32```.


### Declarações (Statements) e Expressões

- **Declarações**: instruções que performam alguma ação e não retornam nenhum valor.
- **Expressões**: Retornam algum valor.

Criar uma variável e associar ela a um valor é uma declaração.

```rust
fn main() {
    let y = 6;
}
```

Declarações não retornam nenhum valor, então você não pode associar uma declaração ```let``` para outra variável, você vai ter um erro se fizer isso:

```rust
fn main() {
    let x = (let y = 6);
}
```

o ```let y = 6``` não retorna nenhum valor, então não tem nada para associar a variável x.

Expressões podem ser parte de declarações, o ```6``` em ```let y = 6``` é uma expressão que resulta no valor 6, chamar uma função é uma expressão, chamar um macro é uma expressão, e um novo escopo em um bloco com ```{}``` também é uma expressão:

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}
```

A expressão:

```rust
{
    let x = 3;
    x + 1
}
```

- É um boco que vai resultar no valor 4. 
- Expressões não usam ```;``` no final para retornar, se você usar ```;``` no valor a ser retornado você vai criar uma declaração e não uma expressão que retorna algo.


### Funções Com Valor de Retorno

Funções podem retornar valores e podemos definir o tipo do valor a ser retornado com ```->```.

Você pode retornar valores em uma função com a palavra ```return``` mas a maioria das funções retornam a última expressão implícita, que não pode ter ```;```.

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}"); // X é 5.
}
```

A função ```five``` acima retorna o valor ```5```, depois associamos esse valor retornado a variável ```x```.

Se tentarmos fazer uma função e no final na última linha que deveria ser retornada colocarmos ```;``` assim:

```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    x + 1;
}
```

Vai gerar um erro, porque transformamos uma expressão em uma declaração.

