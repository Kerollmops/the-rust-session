theme: Poster, 1

# [fit] _**Here we Rust !**_

![](red.jpg)

---

## some primitive types

```rust
let integer: i32 = 42;
let unsigned: u64 = 42_000;
let float: f64 = 32.542;

let boolean: bool = true;

let array = [0.0; 3];
let slice = &[3, 4, 5];

let utf8_char = '😀';
let utf8_str = "I like 🐢!";

let tuples = (3, "cookies", 'b');
```

---

## usefull enums

```rust
enum Option<T> {
    Some(T),
    None,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

---

# [fit] _**Mecanics**_

![](stairs.jpg)

---

## Decomposition

```rust
let (a, b, c) = (1, 2, 3);

if let Ok(file) = File::create("foo.txt") {
    // ...
}

if let Some(bin_name) = env::args().next() {
    // ...
}
```

---

## mutability, constancy by default

```rust
// all bindings are immutable by default
let thing = 42;

// but you can make them mutable if necessary
let mut another_thing = 32;
another_thing += 10;

assert_eq!(another_thing, 42);
```

---

## visibility, privacy is the key

```rust
struct Safe {
    pub lock: SafeLock,
    content: Content,
}

// assume we declare `Safe::new`
let safe = Safe::new();

println!("The lock shows {}", safe.lock);

println!("The content is {}", safe.content);
// the `content` field is private  ^^^^^^^
```

---

## same for modules

```rust
mod car {
    struct Tesla;
    struct Delorean;
}

struct Plane;

let my_tesla = car::Tesla;
//                  ^^^^^ private struct
```

```bash
car/
    mod.rs
    tesla.rs
    delorean.rs

plane.rs
```

---

## enums

```rust
enum Color {
    White,
    Silver,
    Pink,
    Red,
}

enum Animal {
    Sheep { color: Color, shaved: bool },
    PolarBear,
    Salmon(Color),
    Wolf(Color),
}
```

---

## constructors for enums

```rust
impl Animal {
    fn sheep(color: Color) -> Self {
        Animal::Sheep { color: color, shaved: false }
    }

    fn salmon(color: Color) -> Self {
        Animal::Salmon(color)
    }

    // ...
}
```

---

## structures

```rust
struct Rocket {
    position: [f32; 3],
    velocity: [f32; 3],
}

struct Car {
    position: [f32; 3],
    velocity: [f32; 3],
}
```

---

## methods for structures

```rust
impl Rocket {
    fn position(&self) -> [f32; 3] {
        self.position
    }
}

impl Car {
    fn position(&self) -> [f32; 3] {
        self.position
    }
}
```

---

# [fit] _**Traits**_

![](forest.png)

---

## Why not declaring a trait ?

```rust
trait Vehicle {
    fn new() -> Self;

    fn position(&self) -> [f32; 3];
    fn velocity(&self) -> [f32; 3];
}
```

_a Trait is a contract you sign with an object_

---

## Easy to implement

```rust
impl Vehicle for Rocket {
    fn new() -> Self { /* ... */ }

    fn position(&self) -> [f32; 3] { self.position }
    fn velocity(&self) -> [f32; 3] { self.velocity }
}

impl Vehicle for Car {
    fn new() -> Self { /* ... */ }

    fn position(&self) -> [f32; 3] { self.position }
    fn velocity(&self) -> [f32; 3] { self.velocity }
}
```

---

# [fit] _**Generics**_

![](trou-de-ver.jpg)

---

## for functions

```rust
fn display_a_vehicle<V: Vehicle>(vehicle: V)
{
    let position = vehicle.position();
    let velocity = vehicle.velocity();

    println!("the vehicle position is {}", position);
    println!("the vehicle velocity is {}", velocity);
}
```

---

## for custom types

```rust
struct Garage<V: Vehicle> {
    vehicle: Option<V>,
}

impl<V: Vehicle> Garage<V> {
    fn empty() -> Self {
        Self { vehicle: None }
    }

    fn with_vehicle(vehicle: V) -> Self {
        Self { vehicle: Some(vehicle) }
    }
}
```

---

# [fit] _**Match**_

![](magdalena.jpg)

---

## a switch case but better

```rust
let age = 42_u32;

let message = match age {
    0...8  => "you are too young, sorry !",
    9...18 => "ok body, you come in !",
    _      => "you are too old, man !",
};

println!("{}", message);
```

---

## with complex partterns

```rust
let person = ("Elon", "Musk", 1971);

match person {
    ("Elon", "Musk", _) => {
        println!("Hello Elon !")
    },
    (name, lastname, birth) => {
        println!("Hello {} {}", name, lastname)
    },
}
```

---

## syntactic sugar for simple cases

```rust
let person = ("Xavier", "Niel", 1962);

if let ("Xavier", "Niel", _) = person {
    println!("Hi Xavier !")
}
else {
    println!("Whooops ! who are you ?")
}
```

---

## [fit] _**Linear Types**_

![](jupiter.jpg)

---

## Types without Copy

```rust
struct Person {
    name: String,
}

impl Person {
    fn with_name(name: &str) -> Self {
        Self {
            name: String::from(name)
        }
    }
}
```

---

## Moving, cloning, copying

```rust
fn enter_in_the_house(person: Person) {
    // ...
}

let elon = Person::with_name("Elon Musk");

enter_in_the_house(elon);

let use_elon = elon;
//             ^^^^ can't use value after move
```

^ try using `elon` again, it doesn't work

---

# [fit] _**Iterators**_

![](costa-rica.jpg)

---

## enumerate your values

```rust
let numbers = vec![1, 3, 12, 21, 42].into_iter();

for (i, n) in numbers.enumerate() {
    println!("{}. {}", i, n);
}

// should output:
// 0. 1
// 1. 3
// 2. 12
// 3. 21
// 4. 42
```

---

## basic combinations

```rust
let infinity = 0..;
let threes = repeat(3);

let iterator = infinity.zip(threes)
                       .map(|(a, b)| a + b)
                       .take(4);

for (i, n) in iterator.enumerate() {
    println!("{}. {}", i, n);
}

// should output:
// 0. 3
// 1. 4
// 2. 5
// 3. 6
```

---

## lazy evaluations

```rust
let infinity = 0..;

for n in infinity.filter(|&n| n % 2 == 0).take(3) {
    println!("{}", n);
}

// should output:
// 0
// 2
// 4
```

---

## map reduce

```rust
let infinity = 0..;

let sum: usize = infinity.filter(|&n| n % 2 == 0)
                         .map(|n| n * 3)
                         .take(20)
                         .sum();

assert_eq!(sum, 1140);
```

---

## powerfull libraries

```rust
let infinity = (0..).into_par_iter();

let sum: usize = infinity.filter(|&n| n % 2 == 0)
                         .map(|n| n * 3)
                         .take(20)
                         .sum();

assert_eq!(sum, 1140);
```

---

# [fit] _**Lifetimes**_

![](pic.jpg)

---

## simple rules

```rust
let mut thing = 42;

let ref_to_thing = &thing;


let another_ref_to_thing = &thing;
```

---

## simple rules

```rust
let mut thing = 42;

let ref_to_thing = &thing;
//                  ----- immutable borrow here

let another_ref_to_thing = &thing;

let mut_ref_to_thing = &mut thing;
// mutable borrow here      ^^^^^
```

---

# [fit] _**Documentation**_

![](pluto.png)

---

Standard library on _[doc.rust-lang.org/std](https://doc.rust-lang.org/std/)_

All published crates on _[crates.io](https://crates.io/)_

And their documentations on _[docs.rs](https://docs.rs/)_

---

## but also locally

```rust
/// This text preceded by 3 slashes will be available
/// as documentation. But will be visible
/// only if it is a public one.
///
/// This is markdown, yes, and code blocks are tested.
pub fn basic_function(value: i32) -> String {
    // ...
}
```

```bash
$ cargo doc --open
```

---

> Once you can walk barefoot (c), it's easy to walk with shoes (go) but it will take time to learn to ride a bike (rust)
-- /u/freakhill on Reddit

---

# [fit] _**Practice**_

![](colors.jpg)

---

Install **Rust** via _[rustup.rs](https://rustup.rs/)_

or just play with it on _[play.rust-lang.org](https://play.rust-lang.org/)_

---

create a _Person_ struct with a _name_, an _age_ and a constructor

---

create a _Kitchen_ struct that can accept only one _Person_ at a time

---

create the _Ingredient_ and _CookedIngredient_ enums with enough variants  to make tastefull diches

---

create a _Casserole_ struct that can accept multiple _Ingredient_ and returns _CookedIngredient_

---

create a _Dish_ enum that can be constructed with multiple _Ingredient_

---

add a _cook_ method to the _Kitchen_ struct that takes a _Casserole_ and multiple _Ingredient_ and returns a _Dish_

---

let's make a _Table_ type that can accept any number of _People_ with some useful methods

---

now we can serve the _Dish_ on the _Table_, but what if we want to serve other than just _Dishes_ ?

---

what about an _Edible_ Trait ?

---

## Usefull links

read news of the week on _[this-week-in-rust.org](https://this-week-in-rust.org/)_

learn by example on _[rustbyexample.com](http://rustbyexample.com)_

draw funny things with _[turtle.rs](http://turtle.rs/)_
