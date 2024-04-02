
# Armazenando Texto UTF-8 com Strings

 Discutimos Strings no contexto de coleções pois são implementadas como conjuntos de bytes, e mais alguns métodos que dão a elas funcionalidades úteis quando esses bytes são interpretados como textos. 

## O Que é Uma String?

Strings são conjuntos de bits, que são mapeados para caracteres. Atualmente, para codificar bits para caracteres usamos o esquema UTF-8 (8-bit Unicode Transformation Format), onde cada caractere por ser representado por 1 até 4 bytes, em cada byte cabem 8 bits, com essa codificação, podemos ter mais de 1 milhão de caracteres representados. 

Em Rust temos apenas um tipo de string no core da linguagem, o ```str``` que é geralmente visto da sua forma emprestada ```&str```. Por ter tamanho dinâmico, seus bytes ficam armazenados na memória heap, e na memória stack temos uma referência onde aponta para o primeiro caractere da string na memória heap e o seu tamanho. 

Quando criamos uma string, passando diretamente o conteúdo dela, da forma abaixo, criamos uma string que será armazenada diretamente nos binários da aplicação, pois ela não muda e o compilador sabe qual o seu conteúdo e tamanho durante o tempo de compilação:

```rust
let binaries_string = "armazenada nos binarios";
```

Já o tipo ```String```, provido pela biblioteca padrão do Rust, é um tipo de string controlável, escalável, e mutável. Sempre vai ser armazenada na memória heap, e diferente de uma string slice ```(&str)```, agora referenciamos a 3 valores: O primeiro byte da string, o tamanho da string e a sua capacidade.

## Criando uma Nova String

```rust
let mut s = String::new();
```

Nessa linha criamos uma string vazia chamada ```s```, na qual podemos colocar dados dentro. Também podemos criar uma nova string com conteúdo vindo de uma string literal, que era armazenada nos binários, de dois modos:

```rust
let data = "conteudo inicial";

let s = data.to_string();
let s = "conteudo inicial".to_string();

let s = String::from("conteudo inicial");
```

## Atualizando uma String

Uma ```String``` pode crescer e o seu conteúdo pode mudar, como um ```Vec<T>```:

```rust
    let mut s = String::from("foo");
    s.push_str("bar");
	// s = foobar
```

Também podemos colocar um único caractere com o ```push```:

```rust
    let mut s = String::from("lo");
    s.push('l');
    // s = lol
```

### Concatenação com Operador +

Passando o valor de ```s1``` sem passar a referência, como usamos no ```&s2``` torna o ```s1``` inacessível novamente, pois teve seu dono mudado e não existe mais, ao concatenar strings em Rust usando o operador "+", a propriedade da primeira string é transferida para a operação de concatenação.  

```rust
    let s1 = String::from("Hello, ");
    let s2 = String::from("world!");
    let s3 = s1 + &s2; // s1 foi movido e não existe mais
```

## Index em Strings

Em outras linguagens, acessar caracteres individuais em uma string usando seu index é uma operação comum, mas em Rust, você vai receber um erro.

```rust
    let s1 = String::from("hello");
    let h = s1[0];
```

Mas por quê? Vamos entender como o Rust armazena as Strings na memória.

### Representação Interna

```rust
let hello = String::from("Hola");
```

Quando olhamos ao tamanho da String, sabemos que ela será armazenadas com 4 bytes, pois cada letra usa 1 byte quando codificada com UTF-8.

Porém, agora:

```rust
let hello = String::from("Здравствуйте");
```

Você pode pensar que temos 12 bytes nessa string, mas não, agora ela possuí 24 bytes. Porque agora, com o codificador do UTF-8, cada caractere da string possuí 2 bytes. 

Considere esse código inválido em Rust:

```rust
let hello = "Здравствуйте"; 
let answer = &hello[0];
```

Sabemos agora que a resposta não vai ser 3, a primeira letra. Quando codificamos com UTF-8 o primeiro byte, que representa o 3 é 208 e o segundo é 151, (208 151). A resposta poderia ser 208, mas apenas 208 não é um caractere válido, e provavelmente não é o que um usuário iria querer se ele estivesse procurando pela primeira letra de uma string. 

### Fatiando Strings

Se você realmente precisar criar uma fatia de uma string, o Rust vai pedir para ser mais específico. Ao invés de usar ```[]``` para pegar um index da string, você pode usar ```[]``` com uma distancia para pegar bytes específicos: 

```rust
let hello = "Здравствуйте";

let s = &hello[0..4];
``` 

Assim, temos uma ```&str``` que terá os primeiros 4 bytes da string: Зд.

Se tentarmos pegar apenas uma parte do caractere ```&hello[0..1]```, o Rust vai dar panico, pois não é um caractere válido.

### Métodos para Iterar sobre Strings

A melhor forma de operar sobre partes de uma strings é especificar que você quer bytes ou caracteres, para retornar os caracteres com ```char```:

```rust
for c in "Зд".chars() {
    println!("{c}");
}
```

Esse código vai imprimir o seguinte: 

```
З
д
```

Se você quiser retornar os bytes dos caracteres, pode usar o método ```bytes```:

```rust
for b in "Зд".bytes() {
    println!("{b}");
}
```

Que vai imprimir:

```208
151
208
180
```












