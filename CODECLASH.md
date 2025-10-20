# [CodeClash] Halite
This is the starter codebase for the Halite arena featured in CodeClash.

The code represented in this codebase is a mirror of the Halite repository found at https://github.com/HaliteChallenge/Halite.

We make several modifications to the original Halite codebase to better suit the CodeClash environment:
- **Reduced number of supported languages**: The original Halite codebase supports 15 languages - we limit the code options to 4 (C, C++, OCaml, Rust) for the following reasons:
    - We omit Python and JavaScript, despite how easy it is to support them, because they are represented in other languages
    - We omit the other 15 - 4 - 2 = 9 languages because they are either niche or hard to support in an automated setting
    - We keep the above languages because they are popular and not represented in other CodeClash arenas
- **Add a `submission/` folder**: To facilitate submission handling for potentially multiple languages, we ask the player to put any relevant bot code in the `submission/` folder. The folder can contain multiple files and subfolders as needed, but needs to satisfy the following criteria:
    - There must be a *single* entry point file named `main.<ext>`, where `<ext>` is one of the supported language extensions (`c`, `cpp`, `ml`, `rs`)
    - The entry point file must be located directly in the `submission/` folder (i.e., not in a subfolder)
    - The entry point file must implement the Halite bot.

We automate the compilation and execution of the bot with these mappings:
```python
# Command to be run in each agent's `submission/` folder to compile agent
MAP_FILE_TYPE_TO_COMPILE = {
    ".cpp": "g++ -std=c++11 {path}.cpp -o {name}.o",
    ".c": "gcc {path}.c -o {name}.o",
    ".ml": "ocamlbuild -lib unix {name}.native",
    ".rs": "cargo build",
}

# Command to be run from `environment/` folder to run competition
MAP_FILE_TYPE_TO_RUN = {
    ".c": "./{path}/{name}.o",
    ".cpp": "./{path}/{name}.o",
    ".ml": "./{path}/{name}.native",
    ".rs": "./target/debug/{path}",
}
```