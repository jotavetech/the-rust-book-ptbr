# Referências e Empréstimos

Para passarmos uma variável para uma função, sem mover perder ela, podemos usar uma referência, uma referência é como um ponteiro com um endereço, assim podemos acessar os dados armazenados nesse endereço. Diferente de um ponteiro, a referência garante que estará sempre apontando para um valor valido de um tipo específico.

Usando a referência a um objeto:

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

A variável ```s``` de ```calculate_length``` aponta para o endereço de ```s1``` que aponta para o endereço com o valor na memória.

O contrário de uma referência ```&``` é a desreferencia, usando o operador ```*```.

A sintaxe ```&s1``` nos permite criar uma referência que se refere ao valor de ```s1``` mas não vira dono dela, os valores que está apontando não vão ser droppados quando a referência deixar de ser usada.

A assinatura da função usa ```&``` para indicar que o tipo do parâmetro ```s``` é uma referência. 

```rust
fn calculate_length(s: &String) -> usize { // s é uma referência a uma String
    s.len()
} // Aqui s pode sair do escopo, porque s é uma referência, ele não vai ser
  // droppado.
```

Quando fazemos uma referência estamos fazendo um **empréstimo**, então a função que pega a referência não vira a nova dona dela, logo não tem como você pegar de volta, pois continua sendo seu. Se você, por exemplo, empresta sua caneta para uma pessoa, a pessoa vai usar a caneta para a função que ela precisa e depois vai voltar pra você, você não deixou de ser dono.

Em um empréstimo não podemos modificar algo que só temos a referência, vai gerar erro:

```rust
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}
```



### Referências Mutáveis

Podemos arrumar o código acima e permitir a modificação de um valor emprestado, usando referências mutáveis:

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

Agora a referência que criamos vai mudar também a própria variável, no caso, ```s``` agora tem o valor de uma string "_hello, world_".

Uma referência mutável só pode referenciar a um valor, esse código tentando criar duas referências mutáveis vai falhar:

```rust
    let mut s = String::from("hello");

    let r1 = &mut s;
    let r2 = &mut s;

    println!("{}, {}", r1, r2);
```

O rust previne corridas de dados na hora de compilar, isso acontece quando esses comportamentos ocorrem:

- Dois ou mais ponteiros acessam o mesma data ao mesmo tempo.
- Pelo menos um desses ponteiros está sendo usado para escrever na data.
- Não tem nenhum mecanismo sendo usado para sincronizar o acesso a data.

Seguindo a regra das variáveis serem droppadas quando saírem do escopo podemos fazer mais de uma referência assim:

```rust
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 sai do escopo e pode ser referênciado novamente sem problemas.

    let r2 = &mut s;
```

Também não podemos ter referências mutáveis enquanto tivemos alguma referência imutável para o mesmo valor assim:

```rust
    let mut s = String::from("hello");

    let r1 = &s; // sem problema
    let r2 = &s; // sem problema
    let r3 = &mut s; // grande problema

    println!("{}, {}, and {}", r1, r2, r3);
```

Mas o código assim funciona:

```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{} and {}", r1, r2);
    // variables r1 and r2 will not be used after this point

    let r3 = &mut s; // no problem
    println!("{}", r3);
```

Porque ```r1``` e ```r2``` já foram usados antes de criar a próxima referência.


### Referências Pendentes

Uma referência pendente é um ponteiro que referencia a um lugar na memória que pode ter sido dado por outra pessoa, em Rust, o compilador garante que aquela referência nunca vai ser pendente. Se você tem uma referência a alguma data, o compilador vai garantir que a data não vai sair do escopo antes da referência aos dados:

```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s
}
```

Vai retornar um erro:

```console
this function's return type contains a borrowed value, but there is no value
for it to be borrowed from
```

Isso se da porque a função cria uma nova string dentro do seu escopo e tenta retornar uma referência àquela string. Porém, como sabemos, assim que a função acabar, as valores do seu escopo vão ser droppados. Logo, aquela referência que foi criada diretamente dentro da função, deixará de existir.

A solução aqui seria retornar uma String diretamente:

```rust
fn no_dangle() -> String {
    let s = String::from("hello");

    s
}
```

