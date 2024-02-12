# Tipos de Data

Todo valor em Rust tem um tipo de data, isso nos ajuda a saber como trabalhar com esse tipo, vamos ver dois tipos de data: escalável e composta.

O Rust é uma linguagem tipada estática, o que quer dizer que o compilador precisa saber o tipo de todas as variáveis na hora de compilar, o compilador geralmente consegue inferir o tipo de data que queremos usar baseado no valor atribuído e como usamos isso.

Aqui, se não especificássemos que queríamos fazer o parse para um número, o compilador iria gerar um erro, pois ele precisa saber que tipo é para usar:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

## Tipos escaláveis

Representa um valor único, o Rust tem quatro tipos primários escaláveis: Integers, Floating numbers, Booleans, e Characters.

#### Inteiros 
|Length|Signed|Unsigned|
|---|---|---|
|8-bit|`i8`|`u8`|
|16-bit|`i16`|`u16`|
|32-bit|`i32`|`u32`|
|64-bit|`i64`|`u64`|
|128-bit|`i128`|`u128`|
|arch|`isize`|`usize`|

#### Floating

Em rust os tipos flutuantes são ```f32``` e ```f64``` para sistemas x32 e x64 bits respectivamente. O tipo padrão é ```f64``` para ter números com mais precisão.

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```


### Operações Numéricas

Rust também suporta operações básicas de matemática: 

```rust
fn main() {
    // Adição
    let sum = 5 + 10;

    // Subtração
    let difference = 95.5 - 4.3;

    // Multiplicação
    let product = 4 * 30;

    // Divisão
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // Resulta -1

    // Restante
    let remainder = 43 % 5;
}
```

### Booleanos

O Rust só aceita ```true``` e ```false``` para valores booleanos, e eles tem apenas um byte de tamanho:

```rust
fn main() {
    let t = true;

    let f: bool = false;
}
```

### Character 

No Rust, ```char``` são com single quotes ('), ao contrário de strings que usam double quotes ("). Ele é um tipo com quatro bytes em tamanho e representa um Valor Escalável Unicode. Quer dizer que aceita mais do que apenas ASCII, como letras chinesas, japonesas, coreanas, emojis, etc..

```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; 
    let heart_eyed_cat = '😻';
}
```



## Tipos Compostos

Tipos compostos podem agrupar múltiplos valores dentro de um tipo, o Rust tem dois tipos primitivos de tipos compostos: Tuplas e Arrays.

### Tipo Tupla

Geralmente é uma forma de agrupar vários valores com uma variedade de tipos, além disso eles tem tamanho fixo, uma vez declarados, não podem crescer ou diminuir de tamanho.

Quando criamos uma tupla cada posição dela terá um tipo:

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

Para desestruturar tuplas podemos fazer em ordem, assim:

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
}
```

Também podemos acessar um valor diretamente usando . e o índice que queremos, ex: ```x.0```: 

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

Como a maioria das linguagens, o primeiro índice da tupla é o 0.



### Tipo Array

O tipo array, diferente da tupla, tem que ter todos os elementos do mesmo tipo, e diferente de arrays em outras linguagens, arrays no Rust também devem ter tamanho fixo:

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}
}```

Uma array não é flexível como um tipo _vetor_. Um _vetor_ é similar a uma coleção de tipos, provida da biblioteca standard, que permite crescer e diminuir de tamanho, vamos discutir mais a frente.

Arrays são mais úteis quando sabemos que o número de elementos não vai mudar, como por exemplo, os meses de um ano: 

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

Podemos também criar um array especificando o tipo de elemento que ele vai conter e o seu tamanho:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];

// ou também o valor exato de cada elemento

let b: [3, 5] = [3, 3, 3, 3, 3];
```

Podemos usar o index para acessar elementos de um array, como:

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

Se tentarmos acessar um index inexistente o Rust previne problemas futuros e lança um erro na tela, depois termina o programa.



