# Métodos

Métodos são parecidos com funções, nós declaramos eles com a palavra ```fn``` e um nome, eles podem ter parâmetros e retornar valores, e eles contém algum código que pode ser rodado quando alguém chamar esse método. Diferente de funções, métodos são definidos no contexto de uma _struct_, ou um Enum ou um Objeto Trait, o primeiro parâmetro deles sempre é ```self```, que representa a instância da estrutura onde o método está sendo chamado.

Para aprender um pouco mais sobre métodos vamos usar esse código simples de exemplo:

```rust
#[derive(Debug)] // a declaração permite imprimir informações de debug
struct Rectangle {
	width: u32,
	height: u32,
}

fn main() {
	let rect1 = Rectangle {
		width: 30,
		height: 50,
	};

	println!("rect1 is {:?}", rect1); 

	println!("The area of the rectangle is {} square pixels",area(&rect1));
}

fn area(rectangle: &Rectangle) -> u32 {
	rectangle.width * rectangle.height
}
```

O código acima é um programa simples, que tem uma estrutura de um retângulo, recebe as informações de altura e largura e depois usa a função ```area``` para calcular o tamanho da área desse retângulo e imprimir na tela.

## Definindo Métodos

Vamos mudar a função ```area``` que tem uma instância de ```Rectangle``` como parâmetro e, em vez disso, criar um método definido na  estrutura:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

Para criar essa função no escopo do ```Rectangle``` começamos um bloco de ```impl``` (implementação) para ```Rectangle```, tudo que está dentro dele será associado com o tipo ```Rectangle```. O primeiro parâmetro é self, ele mesmo, no caso, a instância de ```Rectangle```.

Depois na ```main``` chamamos o método que agora está associado ao ```Rectangle``` como um método de ```rect1```.

### Métodos Com Mais Parâmetros

Vamos implementar mais um método na _struct_ ```Rectangle```, agora um método que pega outra instância de ```Rectangle``` e retorna true se o segundo ```Rectangle``` consegue preencher totalmente o outro, se não, retorna falso.

```rust
fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };
    let rect2 = Rectangle {
        width: 10,
        height: 40,
    };
    let rect3 = Rectangle {
        width: 60,
        height: 45,
    };

    println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2));
    println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3));
}
```

Agora adicionamos esse outro método ```can_hold```, que recebe a referência a própria instância ```&self```, e a referência a outro objeto ```other: &Rectangle```, depois compara a sua própria instância com a outra que ela recebe, e retorna true se a largura e altura dela mesma é maior que a outra.

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```


### Funções Associadas

Todas as funções definidas dentro de um block ```impl``` são chamadas de _funções associadas_ por que elas estão associadas com o tipo nomeado depois de ```impl```. Podemos definir _funções associadas_ que não _self_ como o primeiro parâmetro, porque elas não precisam de uma instância do tipo para trabalhar com ele.

Funções associadas que não são métodos são geralmente usadas para construtores que vão retornar uma nova instância da _struct_. Por exemplo, vamos criar uma função que cria um novo retângulo do tipo quadrado agora, que recebe um mesmo valor e retorna uma instância com o mesmo valor na altura e largura:

Para chamar essas funções associadas usamos a sintaxe ```::``` assim:

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

fn main() {
	let squr1 = Rectangle::square(2);
	println!("{:?}", squr1); // Rectangle { width: 2, height: 2 }
}
```


### Múltiplos Blocos impl

Podemos também criar blocos separados com ```impl```:

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

impl Rectangle {
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```

Ambos estão associados com a _struct_ ```Rectangle```.

