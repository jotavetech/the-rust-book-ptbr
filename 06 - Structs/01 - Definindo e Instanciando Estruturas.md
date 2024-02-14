# O Que É

_Para fins de aprendizado vamos chamar estruturas pelo seu nome técnico structs_

Uma _struct_ é um tipo de dado customizado, que permite você empacotar vários valores relacionados de um grupo. É parecido com os atributos de um objeto de uma linguagem POO.

# Definindo e Instanciando Estruturas

Structs são familiares com _tuples_, esses dois armazenam valores múltiplos, e como uma _tuple_, as partes de uma estrutura podem ter tipos diferentes, e diferente de uma _tupla_, em uma _struct_ você sempre vai nomear cada parte dos seus dados, então é claro o que aquele valor significa. 

Definimos uma _struct_ com a palavra ```struct``` e colocamos seus dados dentro de chaves ```{}```:

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

Definimos a _struct_, agora para usar temos que criar uma _instancia_ dela, especificando cada valor em cada campo dos dados. As chaves precisam ter os mesmos nomes que definimos na criação da _struct_. 

```rust
fn main() {
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };
}
```

Para acessar um valor de uma instancia da estrutura usamos ```.```, por exemplo, para acessar a chave username: ```user1.username```. Se a instancia for mutável podemos acessar a chave e modificar assim também: 

```rust
fn main() {
    let mut user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    user1.email = String::from("anotheremail@example.com");
}
```

Para mudar algo na instancia, toda ela precisa ser mutável, o Rust não nos permite deixar apenas alguns campos mutáveis e outros não.

Podemos também criar uma função para criar novas instancias, aqui fazemos uma função que recebe um email, um nome e retorna uma instancia de ```User```: 

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username: username,
        email: email,
        sign_in_count: 1,
    }
}
```

_Quando os parâmetros da função e os campos da struct tiverem o mesmo nome, você pode simplesmente passar eles, sem ter que fazer ```username: username```_.

### Struct Update Syntax

Geralmente é bem útil criar novas instancias de uma struct que contem a maioria dos valores de outra instância, mas muda alguns. Podemos fazer isso usando _struct update syntax_.

```rust
fn main() {
    // --snip--

    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };
}
```

Ou também podemos passar desestruturando, com ```..user1```, na função abaixo vamos usar todos os campos de ```user1```, menos o email, o email vamos criar o nosso próprio:

```rust
fn main() {
    // --snip--

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

Lembre-se, os tipos dinâmicos, como String, ao serem associados tem seus donos movidos, logo, quando criamos o ```user2``` e desestruturamos ele passando o valor do campo username, username deixa de existir no ```user1```, mas os campos ```active``` e ```sign_in_count``` continuam existindo em ```user1``` pois são tipos literais têm a trait ```Copy```.

### Usando Tuple Structs Sem Campos Com Nome Para Criar Tipos Diferentes

Rust também suporta structs parecidas com tuples, chamadas _tuple structs_. Tuple structs apenas tem seus tipos nos campos, não tem nomes para eles. São úteis quando você quer dar um nome a tupla toda e fazer ela ser um tipo diferente das outras tuplas. 

Para definirmos uma _tuple struct_ usamos a palavra ```struct``` seguido do seu nome e entre ```()``` os tipos que para ela:

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

Cada _struct_ que você definir vai ter seu próprio tipo. Uma função que recebe um parâmetro do tipo ```Color``` não pode receber ```Point``` como argumento, mesmo que elas tenham os mesmos campos com as mesmos tipos, e você também pode acessar um valor específico em uma _tuple struct_ com ```black.0``` por exemplo. 

### Unit-Like Structs Sem Campos

Também podemos definir _structs_ sem campos, são chamadas de unit-like structs, porque elas são similar ao ```()```, elas são úteis quando você precisa implementar alguma característica em algum tipo mas não tem nenhum dado que você quer guardar no próprio tipo, vamos discutir mais à frente. 

```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```





