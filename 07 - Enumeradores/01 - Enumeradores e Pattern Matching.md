Enumeradores se referem a _enums_. Eles permitem você a definir um tipo enumerando suas possíveis variantes.

## Definindo um Enum

Um _enum_ te da a possibilidade de dizer que o valor é um do set de valores do enum, por exemplo, talvez podemos querer dizer que um ```Rectangle``` é um set de possibilidades que também incluí ```Circle``` e ```Triangle```, para fazer isso, o Rust nos permite codificar essas possibilidades como um enum.

Por exemplo, vamos criar um _enum_ de um IP Address, que pode ser ipv4 ou ipv6:

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

Agora ```IpAddrKind``` é um tipo customizado que pode ser usado em qualquer lugar do código.


### Valores Enums

Agora podemos criar instancias de cada tipo do _enum_ assim:

```rust
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
```

Também podemos criar uma função que recebe qualquer tipo de ```IpAddrKind```:

```rust
fn route(ip_kind: IpAddrKind) {}
```

E podemos chamar essa função passando qualquer tipo de variante: 

```rust
    route(IpAddrKind::V4);
    route(IpAddrKind::V6);
```

Usar _enums_ tem mais vantagens, pense mais sobre o tipo de endereço ip, no momento não temos uma maneira de guardar os dados, apenas sabemos qual é o tipo dele _v6_ ou _v4_. Podemos enfrentar esse problema usando _structs_:

```rust
    enum IpAddrKind {
        V4,
        V6,
    }

    struct IpAddr {
        kind: IpAddrKind,
        address: String,
    }

    let home = IpAddr {
        kind: IpAddrKind::V4,
        address: String::from("127.0.0.1"),
    };

    let loopback = IpAddr {
        kind: IpAddrKind::V6,
        address: String::from("::1"),
    };

```


Assim criamos uma _struct_ que recebe um tipo do enum ```IpAddrKing``` e também um campo ```adress``` do tipo ```String```, e depois instanciamos eles associando a cada tipo de dado.

Podemos representar esse mesmo tipo apenas um ```enum```, melhor que um ```enum``` dentro de uma ```struct```, podemos colocar os dados diretamente dentro de uma variante do enum: 

```rust
    enum IpAddr {
        V4(String),
        V6(String),
    }

    let home = IpAddr::V4(String::from("127.0.0.1"));

    let loopback = IpAddr::V6(String::from("::1"));
```

Podemos observar que o mesmo nome de cada variante do _enum_ também vira uma função construtura para uma instancia dele, quando chamamos ```IpAddr:V4()``` estamos fazendo uma chamada de função que pega um argumento e retorna uma instancia daquele tipo.

Cada variante de um _enum_ pode ter dados de diferentes tipos e quantidades, como:

```rust
    enum IpAddr {
        V4(u8, u8, u8, u8),
        V6(String),
    }

    let home = IpAddr::V4(127, 0, 0, 1);

    let loopback = IpAddr::V6(String::from("::1"));
```

Criando outros tipos de variedades nas suas variantes:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

- Quit: não tem data associada.
- Move: tem campos nomeados, com chaves.
- Write: tem uma String
- ChangeColor: incluí 3 valores i32.

Definir um enum com variantes é similar a criar diferentes tipos de _structs_, assim:

```rust
struct QuitMessage; // unit struct
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // tuple struct
struct ChangeColorMessage(i32, i32, i32); // tuple struct
```

Mas usando tipos diferentes de _structs_ cada um teria seu próprio tipo, não poderíamos criar facilmente uma função que aceite qualquer tipo dessas mensagens como podemos criar com o enum ```Message```, que é definido como um tipo só.

Também podemos definir métodos:

```rust
    impl Message {
        fn call(&self) {
            // method body would be defined here
        }
    }

    let m = Message::Write(String::from("hello"));
    m.call();
```


### O Enum Option e as Vantages Dele em Valores Nulos

O enum ```Option``` codifica todo cenário comum onde algum valor pode ser algo ou pode ser nada. 

Por exemplo, se você pegar o primeiro item de uma lista não vazia você terá o valor, se ela for vazia, você não vai ter nada. 

O Rust não tem o valor _null_ como a maioria das linguagens, eles podem sempre ser um dos valores: null ou not-null.

Temos um enum especial no Rust que lida com o conceito de um valor presente ou ausente, o ```Option<T>```, e é definido pela biblioteca standard:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

Você não precisa trazer isso explicitamente para o código, podemos usar ```Some``` e ```None``` diretamente sem o prefixo ```Option```, O ```Option<T>``` continua sendo um enum normal e ```None``` e ```Some``` são suas variantes.

A sintaxe ```T``` vai ser conversada mais pra frente, porém por agora você precisa saber que ela representa um tipo genérico, que aceita qualquer tipo de dado. 

Usando valores de ```Option``` para segurar tipos de números e strings:

```rust
    let some_number = Some(5);
    let some_char = Some('e');

    let absent_number: Option<i32> = None;
```

Quando temos um valor em ```Some``` quer dizer que temos algo, quando temos um valor ```None``` significa que temos algo com nulo, não temos um valor válido. 

Resumidamente, porque ```Option<T>``` e ```T``` são tipos diferentes, o compilador não nos deixa usar o valor ```Option<T>``` como um valor valido. 

Por exemplo, esse código não vai compilar:

```rust
    let x: i8 = 5;
    let y: Option<i8> = Some(5);

    let sum = x + y;
```

Isso acontece porque o compilador não entende como adicionar esses dois valores juntos, já que representam tipos diferentes, o compilador sempre vai garantis que temos um valor válido.

Você tem que converter um ```Option<T>``` para um ```T``` antes de fazer operações com ele.

Para usar esse valor o _enum_ tem muitos métodos que você pode encontrar [nessa documentação](https://doc.rust-lang.org/std/option/enum.Option.html).



