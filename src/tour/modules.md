# Module

Gleam programs are made up of bundles of functions and types called modules.
Each module has its own namespace and can export types and values to be used
by other modules in the program.

```rust,noplaypen
// inside src/nasa/rocket_ship.gleam

fn count_down() {
  "3... 2... 1..."
}

fn blast_off() {
  "BOOM!"
}

pub fn launch() {
  [
    count_down(),
    blast_off(),
  ]
}
```

Here we can see a module named `nasa/rocket_ship`, the name determined by the
filename `src/nasa/rocket_ship.gleam`. Typically all the modules for one
project would live within a directory with the name of the project, such as
`nasa` in this example.

For the functions `count_down` and `blast_off` we have omitted the `pub`
keyword, so these functions are _private_ module functions. They can only be
called by other functions within the same module.


## Import

To use functions or types from another module we need to import them using the
`import` keyword.

```rust,noplaypen
// inside src/nasa/moon_base.gleam

import nasa/rocket_ship

pub fn explore_space() {
  rocket_ship.launch()
}
```

The statement `import nasa/rocket_ship` creates a new variable with the name
`rocket_ship` and the value of the `rocket_ship` module.

In the `explore_space` function we call the imported module's public `launch`
function using the `.` operator.
If we had attempted to call `count_down` it would result in a compile time
error as this function is private in the `rocket_ship` module.


## Named import

It is also possible to give a module a custom name when importing it using the
`as` keyword.

```rust,noplaypen
import unix/cat
import animal/cat as kitty
```

This may be useful to differentiate between multiple modules that would have
the same default name when imported.


## Unqualified import

Values and types can also be imported in an unqualified fashion.

```rust,noplaypen
import animal/cat.{Cat, stroke}

pub fn main() {
  let kitty = Cat(name: "Nubi")
  stroke(kitty)
}
```

This may be useful for values that are used frequently in a module, but
generally qualified imports are preferred as it makes it clearer where the
value is defined.
