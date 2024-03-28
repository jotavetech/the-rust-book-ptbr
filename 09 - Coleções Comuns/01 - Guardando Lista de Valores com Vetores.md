# Guardando Lista de Valores com Vetores

O primeiro tipo de coleção que vamos ver é ```Vec<T>```, também conhecido como um vetor, ele permite que você armazene mais de um valor em uma estrutura de dados única, colocando os valores pŕoximos um do outro na memória. Eles só armazenam valores do mesmo tipo, e são muito úteis quando você tem uma lista de itens, como as linhas de texto em um arquivo ou preços dos itens em um carrinho de mercado. 

## Criando um Novo Vetor

```rust
    let v: Vec<i32> = Vec::new();
```

Declaramos um vetor passando o tipo dele, pois o compilador precisa saber qual o tipo de elementos que ele vai guardar. 

Rust também tem o macro ```vec!``` que cria um novo vetor que segura os valores que você passar. 

```rust
    let v = vec![1, 2, 3];
```

### Atualizando um Vetor

Usamos um método push para adicionar elementos a um vetor

```rust
    let mut v = Vec::new();

    v.push(5);
    v.push(6);
    v.push(7);
    v.push(8);
```

### Lendo Elementos de Vetores

Tem duas maneiras de referenciar a um valor armazenado em um vetor, pelo index ou usando o método ```get```. 

```rust
    let v = vec![1, 2, 3, 4, 5];

    let third: &i32 = &v[2];
    println!("O terceiro elemento é {third}");

    let third: Option<&i32> = v.get(2);
    match third {
        Some(third) => println!("O terceiro elemento é {third}"),
        None => println!("Não tem terceiro elemento."),
    }
```

Com o método ```get``` recebemos um ```Option<&T>``` que pode ser usado com ```match```. 

Quando queremos um index que passou no tamanho do vetor, usar ```[]``` vai fazer um pânico e quebrar o seu programa, já o método ```get``` vai retornar ```None```, permitindo que você lide com o programa. 

### Iterando sobre Valores em um Vetor

Para acessar cada elemento de um vetor podemos usar o ```for``` loop.

```rust 
    let v = vec![100, 32, 57];
    for i in &v {
        println!("{i}");
    }
```

Também podemos iterar sobre referências mutáveis para cada elemento no vetor. 

```rust
    let mut v = vec![100, 32, 57];
    for i in &mut v {
        *i += 50;
    }
```


### Soltar um Vetor Também Solta Os Seus Elementos

Como qualquer outra estrutura, um vetor deixa de existir quando sai do escopo, o mesmo acontece com os elementos dentro dele.

```rust
    {
        let v = vec![1, 2, 3, 4];

        // do stuff with v
    } // <- v goes out of scope and is freed here
```


