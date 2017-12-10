theme: Poster, 1

# [fit] _**Here we Rust**_

![](red.jpg)

<!-- ![autoplay loop](water.mov) -->

---

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

```rust
let elon = String::from("Elon Musk");

let falcon = Rocket::with_owner(elon);

match falcon.owner.as_ref() {
    "Elon Musk" => println!("Hello Elon !"),
    _ => (),
}
```

<!-- // same as

if let "Elon Musk" = falcon.owner.as_ref() {
    println!("Hello Elon !")
} -->

^ try using `elon` again, it doesn't work

---

```rust
let elon = String::from("Elon Musk");

// cloning `elon` explicitly
let falcon = Rocket::with_owner(elon.clone());

// moving `elon`
let model_s = Car::with_owner(elon);

// can't use `elon` after moving it...
```

---

![](http://cl.ly/image/2v3K3f3d0J1X/britney.gif)

---

```rust
enum Terrain {
    Land,
    Water,
    SubWater,
}
```

---

> Once you can walk barefoot (c), it's easy to walk with shoes (go) but it will take time to learn to ride a bike (rust)
-- /u/freakhill on Reddit
