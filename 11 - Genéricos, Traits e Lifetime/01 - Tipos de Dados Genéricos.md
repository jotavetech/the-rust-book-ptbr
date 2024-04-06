# Tipos de Dados Genéricos

Usamos genéricos para criar assinaturas para funções ou estruturas, onde podemos usar com diversos tipos de dados.

### Em Definições de Funções

Quando definimos uma função que usa genéricos, nos colocamos os genéricos na assinatura da função, onde nos normalmente especificamos os tipos dos parâmetros e do retorno. Fazendo isso deixamos nosso código mais flexível e prevenimos repetição de código. 

Veja o código abaixo, duas funções iguais que encontram o maior valor num pedaço, vamos combinar essas duas funções usando genéricos.

```rust
fn largest_i32(list: &[i32]) -> &i32 {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn largest_char(list: &[char]) -> &char {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest_i32(&number_list);
    println!("The largest number is {}", result);

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result = largest_char(&char_list);
    println!("The largest char is {}", result);
}
```

Para transformar isso em uma única função, nomeamos o tipo do parâmetro, você pode usar qualquer nome que preferir, mas agora vamos usar ```T``` por convenção.  

```rust 
fn largest<T>(list: &[T]) -> &T {
```

A função ```largest```  é um genérico sobre o tipo ```T```, que recebe uma vetor de ```T``` e retorna o mesmo tipo ```T```.

Esse código, agora, não funciona, mas vamos consertar mais tarde:

```rust
fn largest<T>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {}", result);

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result = largest(&char_list);
    println!("The largest char is {}", result);
}
```

Por enquanto, saiba que o erro ocorre porque tentamos comparar valores do tipo ```T```, mas só podemos fazer isso em valores que possam ser ordenados. Mais tarde, vamos consertar isso com traits.

### Em Estruturas

Também podemos usar um parâmetro genérico em um ou mais campos usando a sintaxe ```<>```:

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
}
```

Note que isso diz que os dois campos x e y são do mesmo tipo. Não funciona se tentarmos colocar um tipo diferente para cada.

Para criarmos uma estrutura onde ```x``` e ```y``` são genéricos mas de diferentes tipos, podemos usar múltiplos parâmetros, assim: 

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}
```

### Em Enumeradores

Como fizemos em estruturas, podemos definir que os enumeradores tenham genéricos nas suas variantes, como funciona o enum ```Option<T>```. Que retorna o tipo genérico ou nada. 

```rust
enum Option<T> {
    Some(T),
    None,
}
```

Quando você perceber situações no seu código com múltiplas definições de estruturas que apenas diferem nos tipos que elas seguram, você pode evitar a duplicação com tipos genéricos. 

### Em Métodos

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let p = Point { x: 5, y: 10 };

    println!("p.x = {}", p.x());
}
```

Definimos um método chamado x que retorna uma referência aos dados do campo x. 