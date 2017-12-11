theme: Poster, 1

# [fit] _**Here we Rust**_

![](red.jpg)

---

## Structures, simple structures

```rust
struct Rocket {
    position: [f32; 3],
    velocity: [f32; 3],
    color: [u8; 3],
    owner: String,
}
```

^ no class, only struct

---

## No ugly overloaded constructor

```rust
impl Rocket {
    fn with_owner(owner: String) -> Self {
        Self {
            position: [0.0; 3],
            velocity: [0.0; 3],
            color: [0, 0, 0],
            owner: owner,
        }
    }
}
```

^ constructor doesn't exists, static methods are used for this

---

## Match, powerfull switch-case

```rust
let elon = String::from("Elon Musk");

let falcon = Rocket::with_owner(elon);

match falcon.owner.as_ref() {
    "Elon Musk" => println!("Hello Elon !"),
    _ => (),
}
```

---

## But... Elon do Cars too

```rust
struct Car {
    position: [f32; 3],
    velocity: [f32; 3],
    color: [u8; 3],
    owner: String,
}

impl Car {
    fn with_owner(owner: String) -> Self {
        // same as before...
    }
}
```

_**Whoops !** Duplication of function declaration_

---

## Why not declaring a trait ?

```rust
trait Vehicle {
    fn with_owner(owner: String) -> Self;
}
```

_a trait is a contract you sign with an object._

---

## Easy to implement

```rust
impl Vehicle for Rocket {
    fn with_owner(owner: String) -> Self {
        // ...
    }
}

impl Vehicle for Car {
    fn with_owner(owner: String) -> Self {
        // ...
    }
}
```

---

## [fit] _**linear types**_

![autoplay loop](water.mov)

---

## Moving, cloning, copying

```rust
let elon = String::from("Elon Musk");

let falcon = Rocket::with_owner(elon.clone());

let model_s = Car::with_owner(elon);

println!("Can I speak to you {} ?", elon);
// can't use value after move       ^^^^
```

^ try using `elon` again, it doesn't work

---

![](http://cl.ly/image/2v3K3f3d0J1X/britney.gif)

---

## a person wrapper seems more correct

```rust
struct Person {
    name: String,
    age: u8,
}

impl Person {
    fn new(name: String, age: u8) -> Self {
        Self { name, age }
    }
}
```

---

## let's update the trait a little

```rust
trait Vehicle {
    fn with_owner(person: Person) -> Self;
}

impl Vehicle for Rocket {
    fn with_owner(person: Person) -> Self {
        Self {
            owner: person.name,
            // ...
        }
    }
}
```

---

> Once you can walk barefoot (c), it's easy to walk with shoes (go) but it will take time to learn to ride a bike (rust)
-- /u/freakhill on Reddit

---

