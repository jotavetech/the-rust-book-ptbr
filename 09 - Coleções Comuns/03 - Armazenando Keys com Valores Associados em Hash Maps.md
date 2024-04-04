# Armazenando Keys com Valores Associados em Hash Maps

A última das nossas coleções comuns é o _hash map_. O tipo ```HashMap<K, V>``` armazena um mapeando das keys do tipo ```K``` para os valores do tipo ```V``` usando funções hash, que determina como isso coloca os valores na memória. 

Eles são úteis quando você quer procurar dados sem usar indices. Por exemplo, em um jogo, você pode rastrear cada pontuação de cada time em um hash map, onde cada key é o nome do tipo e os valores dentro da key são as pontuações. 

### Criando um Novo Hash Map

Uma maneira de criar um hash map vazio é usando ```new``` e depois adicionar os seus elementos com ```insert```. Aqui criamos dois times e suas pontuações, o primeiro parâmetro é a chave e o segundo é o valor associado a chave.

```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);
```

Assim como vetores, hash maps armazenam seus dados na heap, e são homogêneos, todas as suas chaves e valores devem possuir o mesmo tipo.

### Acessando Valores em Hash Maps

Para pegarmos um valor de um hash map usando uma key, usamos o método ```get```:

```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    let team_name = String::from("Blue");
    let score = scores.get(&team_name).copied().unwrap_or(0);
```

O método ```get``` retorna um tipo ```Option<&V>```, se a key não tiver valor, será retornado none, então usamos ```copied``` para criar uma cópia daquele valor e não mais uma referência, depois ```unwrap_or``` para setar a pontuação para zero caso não exista nada associado a aquela chave. 

Também podemos iterar sobre cada chave e valor com um ```for``` loop:

```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    for (key, value) in &scores {
        println!("{key}: {value}");
    }
```

### Atualizando um Hash Map

As chaves do hash map não podem se repetir, não podem ter o mesmo nome.

Quando você quer mudar um dado em um hash map você tem que decidir o que fazer em caso que a chave já tenha um valor associado, se você quer ignorar, sobrescrever ou combinar..

### Sobrescrevendo um Valor

Se inserirmos um novo valor com a mesma chave, o valor associado a chave será sobrescrito, assim: 

```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Blue"), 25);

    println!("{:?}", scores); // 25
```

Agora, a chave ```Blue``` tem valor ```25```.

### Adicionar Apenas Se Não Estiver Presente

Para checar se uma chave já existe e apenas inserir se ela não existir podemos usar o método```entry``` e ```or_insert```:

```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();
    scores.insert(String::from("Blue"), 10);

    scores.entry(String::from("Yellow")).or_insert(50);
    scores.entry(String::from("Blue")).or_insert(50);

    println!("{:?}", scores);
```

### Atualizando Chaves Baseado no Valor Antigo

Outro caso comum para hash maps é atualizar o valor baseado no valor antigo.

Esse código conta quantas vezes a palavra se repete, se for a primeira vez que vemos uma palavra, o primeiro valor inserido será 0.

```rust
    use std::collections::HashMap;

    let text = "hello world wonderful world";

    let mut map = HashMap::new();

    for word in text.split_whitespace() {
        let count = map.entry(word).or_insert(0);
        *count += 1;
    }

    println!("{:?}", map);
```







