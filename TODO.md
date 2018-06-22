- great ecosystem
    - Cargo: the compiler wrapper and package manager
        - http://crates.io
        - alternative register
    - online documentation http://docs.rs
    - local documentation using `cargo doc` and `rustup doc`

- show generics and traits advantages over interfaces
    - more explicitly less errors
    - explicit monomorphization
    - explicit dynamic dispatch (or Trait object)
    - powerful reused types
        - `String` is just a `Vec<u8>`
        - `HashSet` is just an `HashMap<_, ()>`

- useful libraries
    - `std` and `core`
        - `Result`, `Option`, std::collections
        - `&[]`, `Vec` and `&str` `String`
        - `mpsc`, std::arch
        - `Iterator` and std::iter
        - `checked_add/mul/div`
    - `rand`, `regex`, `byteorder` and `num`
    - `rayon`, `serde`, `diesel` and `gfx`

- important projects
    - `TiKV`, `ripgrep`, `redox-os`
    - `rlsl` and `accel`
    - `servo` http://youtu.be/u0hYIRQRiws
    - http://hellorust.com
    - http://demo.nphysics.org

- show system programming: why is it possible to do that ?
    - everything is safe
        - one `&mut` at the same time
        - no race conditions possible
    - Linear typing
    - performances
        - no garbage collector
        - minimum overhead
    - explicit memory representation
    - easily testable
    - compilation mode can be specified

- big societies using this language
    - Mozilla for Firefox and other projects
    - Dropbox for there compression system (22% compression gain)
    - NPM replace C with Rust
    - Atlassian to analyze petabytes of source code
    - OVH developed an high performance log management system
    - Baidu to execute WASM in a secure way

