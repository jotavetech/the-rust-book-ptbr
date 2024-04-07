
# Traits, Definindo Comportamento Compartilhado

Uma trait nos permite a definir um set de métodos que são compartilhados por diferentes tipos.
## Definindo uma Trait

Vamos supor que temos duas estruturas ```NewsArticle``` e ```Tweet``` e queremos que essas duas tenham um método que nos possibilite imprimir um resumo na tela.

```rust
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {}", self.headline, self.author)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

pub trait Summary {
    fn summarize(&self) -> String;
}

fn main() {
    let tweet = Tweet {
        username: String::from("joao"),
        content: String::from("poca pica"),
        reply: true,
        retweet: true,
    };

    let article = NewsArticle {
        author: String::from("jao"),
        content: String::from("muita bala"),
        headline: String::from("blablabla"),
        location: String::from("socorro"),
    };

    println!("Tweet summary: {}", tweet.summarize());
    println!("Article summary: {}", article.summarize());
}

```

Criamos uma trait Summary com a palavra-chave ```trait```, note que apenas definimos a assinatura da função, não é a função em si. Isso diz que pra cada tipo que implementa essa trait, ele deve ter o método summarize().

Depois implementamos esse método definido na trait nos nossos dois tipos criados, cada um com a mesma assinatura porém com sua forma de lidar com o tipo.

Também podemos colocar uma implementação padrão na declaração da trait, que pode ser sobrescrita:

```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("Read more..")
    }
}
```

Traits permitem adicionar mais que um método para os tipos:

```rust
impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }

    fn summarize_author(&self) -> String {
        self.username.clone()
    }
}

pub trait Summary {
    fn summarize(&self) -> String {
        String::from("Read more..")
    }

    fn summarize_author(&self) -> String;
}
```

### Trait Bounds

Agora vamos falar sobre traits como parâmetros: 

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

Criamos uma função que recebe como parâmetro uma referência para qualquer tipo que implemente a trait Summary. Assim podemos passar a nossa estrutura ```article``` ou ```tweet``` que implementam a trait Summary:

```rust
notify(&article);
```

Podemos usar genéricos também, para dizer que um ou mais parâmetros são do mesmo tipo:

```rust
pub fn notify2<T: Summary>(item1: &T, item2: &T) {
	//....
}
```

Também da de definir múltiplas traits com a sintaxe ```impl```:

```rust
pub fn notify3(item1: &(impl Summary + Display), item2: &impl Summary) {
    //....
}
```

### Retornando Tipos que Implementam Traits

Podemos usar a sintaxe ```impl Trait``` no retorno de uma função para indicar que vamos retornar o valor de algum tipo que implementa essa trait:

```rust
fn returns_summarizable() -> impl Summary {
    Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    }
}
```

