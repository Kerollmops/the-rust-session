theme: Poster, 1

# [fit] _**Programming is Hard**_

![](cables.jpg)

---

### _**because we are all humans and humans depends on software and its quality**_

![](eye.png)

---

# The Therac-25 horror story

A radiaton therapy machine produced in 1982. Involved in at least _**6 accidents**_ between 1985 and 1987 in which patients were given _**massive overdose of radiation**_.

Because of concurrent programming errors, it sometimes gave its patients radiation doses that were hundreds of times greater than normal, resulting in _**death**_ or _**serious injury**_.

![](eye.png)

---

There is a huge difference between

"_program correctness was **checked** by tests_"

and

"_program correctness was logically **proven**_"

![](stairs.jpg)

---

![](linux-cve-2018-jan-apr.png)

---

# _**buffer overflow bugs are one of the most dangerous**_

![](oil.jpg)

---

## they often cause secrets to be compromised, denial of service and remote code execution vulnerabilities

![](oil.jpg)

<!--

heartbleed.com

Linux kernel is written by best of the best, top 5% of the industry

-->

---

![](buffer-overflow-linux.png)

---

# [fit] _**At what cost ?**_

![](pluto.jpg)

---

No garbage collector. You pay for what you need.

When you need to share readable and writable data, Rust will _force_ you to use thread synchronized primitives (e.g. Mutex, Atomic types).

You can express everything that you could have expressed in _C_ or _C++_. Even _[SIMD]_ instruction are available in the standard library. You may unleash the full potential of your hardware by using [convenient wrapper libraries].

[convenient wrapper libraries]: https://github.com/jackmott/simdeez
[SIMD]: https://fr.wikipedia.org/wiki/Single_instruction_multiple_data

![](pluto.jpg)

---

![](fast-programs.png)

---

# [fit] _**Tests**_

![](trou-de-ver.jpg)

---

Note that for performance reasons, the standard library contains a lot of potentially unsafe code and raw pointer arithmetic. This is the main reason why tests are important.

As of the 10 of july 2017 there are a little over _6k_ tests in the Rust test suite. All features and bug fixes are accompanied by test cases.

![](trou-de-ver.jpg)

---

Wrappers are everywhere in Rust, concepts that needs performances are often implemented with unsafe code that permit raw communication.

Multi-threading libraries use this concept to abstract complexes algorithms and let the user focus on its own work.

```rust
let sum = many_numbers.iter().sum();

let sum = many_numbers.par_iter().sum();
```

![](trou-de-ver.jpg)

---

# [fit] _**Libraries**_

![](water-micro.jpg)

---

<!-- TODO -->

![](water-micro.jpg)

---

# [fit] _**Mecanics**_

![](stairs.jpg)

---

# [fit] _**Go Duck Typing**_

![](rubber-duck.jpg)

---

# [fit] _**Rust Traits**_

![](forest.png)

---

# [fit] _**Generics**_

![](trou-de-ver.jpg)

---

## [fit] _**Linear Types**_

![](jupiter.jpg)

---

# [fit] _**Iterators**_

![](costa-rica.jpg)

---

# [fit] _**Lifetimes**_

![](pic.jpg)

---

# [fit] _**Documentation**_

![](pluto.jpg)

---

# [fit] _**Practice**_

![](colors.jpg)
