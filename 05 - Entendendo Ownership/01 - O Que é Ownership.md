# O Que é Ownership

É uma das mais únicas ferramentas do Rust que permite deixar a memória do seu programa mais segura sem precisar de um garbage collector, como em algumas outras linguagens, é muito importante entender como isso funciona.

Ownership é um set de regras que comandam em como um programa Rust controla a memória, todos os programas tem seus métodos para controlar o modo de que usam a memória enquanto estão rodando, algumas linguagens tem o garbage collector, que procura por memória que não está mais sendo usada e libera, em outras linguagens o programador tem que alocar e liberar a memória manualmente. 

Em Rust a memória é gerenciada por um sistema de propriedade junto com um conjunto de regras do compilador, se alguma das regras forem violadas o programa não vai compilar. 

## Regras do Ownership 

- Cada valor no Rust tem um dono.
- Eles só podem ter um dono por vez.
- Quando o dono sair do escopo, o valor será droppado.


## Escopo de Variável

Vamos ver o escopo de algumas variáveis, o escopo é o lugar que a variável é válida

```rust 
    {                      // s não é válida aqui, ainda não foi declarada
        let s = "hello";   // s é válida daqui em diante

        // faça algo com s
    }                      // o escopo acabou, a vriável não é mais válida
```

Associamos a variável ```s``` a uma string _literal_ e podemos ver dois importantes pontos:
- Quando ```s``` está no escopo, é valido.
- Ela continua válida até sair do escopo. 


## O Tipo String

O tipo de string usado anteriormente é guardado na memória stack pois o compilador sabe o seu tamanho, o que precisamos agora é de um tipo mais complexo e dinâmico, que pode mudar e que seja armazenado na _heap_. 

O tipo ```String``` nos permite criar strings que não sabemos o valor no compile time.

```rust
let s = String::from("hello");
```

Usamos o ```::``` para usar a função particular de ```String``` e criar uma string.

```rust
    let mut s = String::from("hello");

    s.push_str(", world!"); // push_str() coloca uma string literal

    println!("{}", s); //  hello, world!
```

Por que o tipo ```String``` pode ser mutável e o tipo _literal_ não? A diferença é como esses dois tipos lidam com a memória.

## Memória e Alocação

No caso de uma string _literal_ nós sabemos o conteúdo na hora de compilar, então o texto vai direto para o executável final, mas só funciona para strings que sabemos o quanto vamos usar, e não para strings dinâmicas, pois não sabemos quando espaço precisamos armazenar na hora de compilar, e o tamanho também pode mudar enquanto o programa está rodando.

Com o tipo ```String``` criamos uma variável mutável, que pode crescer e diminuir, e para isso alocamos um monte de memória heap:

- A memória precisa ser requisitada no alocador de memória durante o tempo de execução.
- Precisamos de um modo de retornar essa memória para o alocador quando terminamos de usar a ```String``` 

Quando usamos ```String::from``` pedimos a memória que precisamos.

No rust a memória é automaticamente retornada, uma vez que a memória que a possui sai do escopo. 

```rust
    {
        let s = String::from("hello"); // s é válido daqui em diante

        // faça algo com s
    }                                  // o escopo acabou e s não é mais válido
```

Quando uma variável sai do seu escopo o Rust chama uma especial função para nós, chamada ```drop```, ele chama essa função automaticamente no final das chaves {}.

### Variáveis e Interação de Data com Move.

Múltiplas variáveis podem interagir com o mesmo dado de formar diferentes: 

```rust 
    let x = 5;
    let y = x;
```

Nós podemos chutar que provavelmente estamos associando o valor 5 para a variável ```x``` e depois fazendo uma cópia do valor de x e associando em y, agora temos duas variáveis iguais a 5. E é isso que está acontecendo, pois os números inteiros são valores fixos e conhecidos, esse dois valores 5 são colocados na _stack_.

Vamos ver agora a versão em ```String```:

```rust
    let s1 = String::from("hello");
    let s2 = s1;
```

Parece muito similar a outra associação, com números, podemos pensar que está fazendo a mesma coisa agora, criando uma cópia de s1 e associando ela a s2, mas não é exatamente assim que funciona.

Quando fazemos isso o que acontece agora é que os dados da ```String``` são copiados, o que significa que copiamos o ponteiro, o tamanho e capacidade que está na stack.

Como dissemos, quando uma variável sai do seu escopo o Rust automaticamente chama a função ```drop``` e libera a memória _heap_ para aquela variável, e como vimos ambas as variáveis tem um ponteiro apontando para o mesmo lugar. Esse é um problema, quando s1 ou s2 saem do escopo as duas vão tentar liberar a mesma memória podendo corromper a memória.

Para manter a segurança da sua memória, depois que você associa a variável ```let s2 = s1``` a variável ```s1``` não é mais válida, então o Rust não tem que liberar nada quando ```s1``` sair do escopo:

```rust
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{}, world!", s1);
```

O código acima retornará um erro, porque ```s1``` foi movido e agora ```s2``` é dono daqueles dados!

Em escopos:

```rust
let s1 = String::from("hello");

{
	let s2 = s1; // s2 agora é dono de s1 e s1 é droppado
}

println("{s1}");  // erro, porque s1 não existe mais
```

Se quisermos copiar a data _heap_ da ```String``` e não apenas a _stack_, podemos usar um método comum chamado ```clone```:

```rust
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
```

Isso funciona perfeitamente, pois a data _heap_ é copiada. Não apontam pra o mesmo local e não ocupam o mesmo espaço em memória, diferente de quando associamos a ```String s1``` a ```s2```  fazendo s2 ser proprietária dos dados. 


### Stack-Only Data: Cópia

Isso funciona e é válido:

```rust    
let x = 5;
let y = x;

println!("x = {}, y = {}", x, y);
```

O motivo de ```x``` continuar sendo válido mesmo sem chamar ```clone``` é que ```x``` é um inteiro no qual vamos ter o tamanho dele na hora de compilar, então ele será armazenado na _stack_, é bem rápido fazer essa cópia, isso significa que não há motivo de prevenir ```x``` de continuar sendo válido mesmo depois de associar a outra variável.

Rust tem uma notação especial chamada ```Copy```, que podemos colocar em tipos que são armazenados na memória _stack_, as variáveis que usam isso não são movidas, são trivialmente copiadas, deixando elas continuarem válidas depois de ser associadas a outra variável.

Alguns tipos que implementam ```Copy```:
- Todos os inteiros, como u32.
- Booleanos.
- Floating, como f64.
- Char.
- Tuplas. Mas só quando tem o mesmo tipo em todos os elementos como (i32, i32, i32).

## Ownership e Funções

Passar valores para uma função é similar a associar um valor para uma variável . Passar uma variável para uma função vai mover ou copiar.

```rust
fn main() {
    let s = String::from("hello");  // s vem no escopo

    takes_ownership(s);             // o valor de s é passado para a função
                                    // ... s deixa de ser válido aqui

    let x = 5;                      // x vem no escopo

    makes_copy(x);                  // x seria movido para a função,
                                    // mas i32 é um tipo que pode ser copiado
                                    // então continua válido
}

fn takes_ownership(some_string: String) { // some_string vem no escopo
    println!("{}", some_string);
} // aqui o escopo acabou é o drop é chamado para limpar o some_string

fn makes_copy(some_integer: i32) { // some_integer vem no escopo
    println!("{}", some_integer);
} // aqui some_integer sai do escopo mas nada especial acontece
```


### Retornar Valores e Escopo

Retornar valores também transfere o proprietário, o dono de uma variável sempre segue o mesmo padrão, associar um valor para outra variável, move ele. Quando uma variável que possui seus dados na _heap_ sai do escopo, o valor vai ser limpo pela função ```drop``` a não ser que o proprietário dos dados seja movido para outra variável.

Pegar um dono e retornar ele para cada função é tedioso, e se quisermos que uma função use um valor mas não seja o proprietário dele? É chato que tudo que passamos também precisa ser passado de volta se quisermos usar novamente,  além de qualquer dados resultantes da função que também possamos querer retornar.

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() retorna o tamanho da string.

    (s, length)
}
```

Mas isso é muito trabalho para um conceito que deveria ser comum, mas felizmente, o Rust possui um recurso para usar um valor sem transferir propriedade, é chamado de referências (_references_).