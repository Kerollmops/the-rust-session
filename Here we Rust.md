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

# [fit] _**Whoops !**_

![](crime.jpg)

^ duplication of code

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
    ("Elon", "Musk", _)  => println!("Hello Elon !"),
    (name, lastname, birth) => {
        println!("Hello {} {}", name, lastname)
    },
}
```

---

## syntactic sugar for simple cases

```rust
let person = ("Xavier", "Niel", 1962);

if let ("Xavier", "Niel", _) = person
{
    println!("Hi Xavier !")
}
else
{
    println!("Whooops ! who are you ?")
}
```

---

## [fit] _**Linear Types**_

![autoplay loop](water.mov)

---

## Types without Copy

```rust
struct Person {
    name: String,
}

impl Person {
    fn with_name(name: String) -> Self {
        Self { name }
    }
}
```

---

## Moving, cloning, copying

```rust
fn enter_in_the_house(person: Person) {
    // ...
}

let name = String::from("Elon Musk");
let elon = Person::with_name(elon);

enter_in_the_house(elon);

let use_elon = elon;
//             ^^^^ can't use value after move
```

^ try using `elon` again, it doesn't work

---

![](http://cl.ly/image/2v3K3f3d0J1X/britney.gif)

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

> Once you can walk barefoot (c), it's easy to walk with shoes (go) but it will take time to learn to ride a bike (rust)
-- /u/freakhill on Reddit
