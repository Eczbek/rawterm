# rawterm
A c++ header-only library for working with raw mode in a terminal. This has
been tested solely in c++ - I don't know if this works in C.

See `main.cpp` for an example on how this works

Note that `escape.cpp` will print out the raw code that you press, and exists
for testing purposes only.

### How to use
Your startup and cleanup are handled with a few function calls:

```cpp
#define RAWTERM_IMPLEMENTATION
#include "rawterm.h"

int main() {
    enable_raw_mode();
    enter_alt_screen();
    // Do things here
    exit_alt_screen();
    return 0;
}
```

We also provide the `Key` event to track and handle keypresses, returned from
the `process_keypress()` function, which takes no parameters. A `Key` looks
like this:

```cpp
struct Key {
    char code;
    std::vector<Mod> mod;
    std::string raw;
};
```

* `code` - is the value you pressed (ie `a`)
* `mod` - is a vector of modifier keys
    * Values include: `Alt_L`, `Arrow`, `Control`, `Delete`, `Escape`, `Function`, `Shift`, `Space`
* `raw` - is the raw ascii string that the terminal takes in.
    * This mostly exists for debugging

The `get_term_size()` function provides the size of the terminal window as
an `std::pair<int, int>` as well

### Be Aware Of:
* `^m` and `enter` are the same key - they both pass `\x0d` code to the terminal
* `^i` and `tab` are the same key - they both pass `\x09` code to the terminal
* left alt and right alt are different keys - only left-alt has been handled
    * I think this is also the same for alt-gr on windows laptops
* `CMD` on macos doesn't return a unique ascii code, and is treated like the
base character being pressed
