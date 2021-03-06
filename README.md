# nix-parsec

Parser combinators in Nix. Nix isn't meant to be a general purpose programming
language, so using parser combinators like this should really be avoided,
unless:

- Nix's built-in regex tools won't work for your use case
  - You _usually_ don't need to parse things in Nix, and when you do, you
    usually don't need anything more powerful than regex
- Parsing performance will not be a bottleneck for the build
  - If it is, consider implementing your parser in your language of choice and
    using nix to invoke it
- It's difficult to pass results of parsing in another language back to Nix

Don't ask what I actually needed this for.

### Usage

Include by fetching via usual means (`fetchTarball`, `fetchFromGitHub`, etc.):

```nix
let
  version = "v0.1.0";
  sha256 = "...";

  nix-parsec = import (builtins.fetchTarball {
    url = "https://github.com/nprindle/nix-parsec/archive/${version}.tar.gz";
    inherit sha256;
  });

  inherit (nix-parsec) parsec lexer;
in ...
```

At the top level, two attribute sets are exported:

- `parsec`: Parser combinators and functions to run parsers
- `lexer`: Combinators for parsing token-related things

The parsing/lexing APIs roughly corresponds to those of Haskell's `megaparsec`
library. See `examples/` for some example parsers.

