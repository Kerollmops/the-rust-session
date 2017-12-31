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

let tuples = (0b0011, "cookies", 'b');

let utf8_char = '😀';
let utf8_str = "I like 🐢!";
```

---

## different kind of loop

```rust
loop {
    // ...
}

for x in 0..25 {
    // ...
}

while x > 32 {
    // ...
}
```

---

## custom structs

```rust
struct Unit; // ...like an empty tuple `()`

struct Tuple(i32, String);

struct Classic {
    foo: String,
    bar: i32,
}
```

---

## custom enums

```rust
enum Never{}; // ...the old never type `!`

enum Unit {
    Variant1,
    Variant2,
}

enum Mixed {
    Foo { foo: i32, bar: f32 },
    Bar(f64, u8)
}
```

^ unit enum is not a C enum

---

## the unit type

```rust
let unit = ();

fn foo() {
    // ...
}

fn bar() -> () {
    // ...
}
```

---

## the never type

```rust
let never = !;

fn will_never_return() -> ! {
    loop { /* ... */ }
}

fn never_error(text: String) -> Result<String, !> {
    Ok(text)
}
```

^ `let Ok(string) = String::try_from(another_string);`

---

## useful enums

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

# [fit] _**Match**_

![](magdalena.jpg)

---

## Pattern match enum

```rust


fn maybe_return_thing() -> Option<u32> { /* ... */ }

match maybe_return_thing() {
    Some(thing) => println!("{}", thing),
    None => (),
}
```

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

## syntactic sugar for simple cases

```rust


fn maybe_return_thing() -> Option<u32> { /* ... */ }

if let Some(13) = maybe_return_thing() {
    println!("{}", 13)
}
```

---

# [fit] _**Standard**_

![](water-micro.jpg)

---

## useful structs

```rust
let vector = Vec::new();

let hashmap = HashMap::new();

let tcpstream = TcpStream::connect("127.0.0.1:34254");

let path = Path::new("/tmp/foo/bar.txt");
```

---

## default, small aside

```rust
let mut hashmap = HashMap::new();

let value = hashmap.entry("foo").or_default();

assert_eq!(*value, 0);

let value = hashmap["bar"];
//                 ^^^^^^^ Index accessor not implemented
```

---

## The String type

```rust
let string = String::from("world");

let formatted = format!("Here is a {:?} string !", "cool");

let another = format!("hello {kiki} !", kiki="Rock");

let concat = String::from("The cat is") + " alive !";
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

## constancy by default

```rust


let thing = 42;

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
    pub struct Delorean;
}

struct Plane;

let my_delorean = car::Delorean;

let my_tesla = car::Tesla;
//                  ^^^^^ private struct
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

## Automatic deriving

```rust
#[derive(Debug, Copy, Clone)]
enum Color {
    White,
    Silver,
    Pink,
    Red,
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

    println!("the vehicle position is {:?}", position);
    println!("the vehicle velocity is {:?}", velocity);
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

^ show `std::mem::drop` signature

---

# [fit] _**Iterators**_

![](costa-rica.jpg)

---

## some details

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;

    fn count(self) -> usize { ... }

    fn last(self) -> Option<Self::Item> { ... }

    // ...
}
```

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

## powerful libraries

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
let thing = 42;

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

## lightweight structures

```rust
struct OnlyDigits<'a> {
    text: &'a str
}

impl<'a> OnlyDigits<'a> {
    pub fn new(text: &'a str) -> Option<Self> {
        // construct only if text is only digits
    }
}
```

---

## lightweight memory

```rust


// a move or a clone of the String is needed
fn moving_function(text: String);


// just a borrow of the internal str
fn borrowing_function(text: &str);

```

---

Copy On Write is powerful sometimes let's see _[the Cow enum](https://doc.rust-lang.org/std/borrow/enum.Cow.html)_ declaration

While you only read the inner data no clone is done

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

## Useful links

read news of the week on _[this-week-in-rust.org](https://this-week-in-rust.org/)_

learn by example on _[rustbyexample.com](http://rustbyexample.com)_

the _[memory mecanics](https://rufflewind.com/img/rust-move-copy-borrow.png)_  and _[containers](https://goo.gl/67RKj9)_ cheat sheets

official _[collections guide](https://doc.rust-lang.org/std/collections/)_

---

## Other links

The _[Rust Cookbook](https://rust-lang-nursery.github.io/rust-cookbook/intro.html)_ is a collection of simple examples

dive into modules with this _[dev.to blogpost](https://dev.to/hertz4/rust-module-essentials-12oi)_

draw funny things with _[turtle.rs](http://turtle.rs/)_
