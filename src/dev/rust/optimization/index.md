```
                                                                      
   ▄▄▄▄                           ██                  ██              
  ██▀▀██               ██         ▀▀                  ▀▀              
 ██    ██  ██▄███▄   ███████    ████     ████▄██▄   ████     ████████ 
 ██    ██  ██▀  ▀██    ██         ██     ██ ██ ██     ██         ▄█▀  
 ██    ██  ██    ██    ██         ██     ██ ██ ██     ██       ▄█▀    
  ██▄▄██   ███▄▄██▀    ██▄▄▄   ▄▄▄██▄▄▄  ██ ██ ██  ▄▄▄██▄▄▄  ▄██▄▄▄▄▄ 
   ▀▀▀▀    ██ ▀▀▀       ▀▀▀▀   ▀▀▀▀▀▀▀▀  ▀▀ ▀▀ ▀▀  ▀▀▀▀▀▀▀▀  ▀▀▀▀▀▀▀▀ 
           ██                                                         
                                                                      
                                                  
                        ██                        
             ██         ▀▀                        
  ▄█████▄  ███████    ████      ▄████▄   ██▄████▄ 
  ▀ ▄▄▄██    ██         ██     ██▀  ▀██  ██▀   ██ 
 ▄██▀▀▀██    ██         ██     ██    ██  ██    ██ 
 ██▄▄▄███    ██▄▄▄   ▄▄▄██▄▄▄  ▀██▄▄██▀  ██    ██ 
  ▀▀▀▀ ▀▀     ▀▀▀▀   ▀▀▀▀▀▀▀▀    ▀▀▀▀    ▀▀    ▀▀ 
```

## TOML file

```toml
[package]
name = "ApplicationNAme"
version = "0.0.1"
edition = "2021"
publish = false

[lib]
crate-type = ["cdylib", "lib"]

[[bin]]
name = "app_bin"
path="./path/to/bin_main.rs"

[profile.release]
# Disable debug
debug=false
# Optimization Level
opt-level = 's'
# Link Time Optimization (LTO)
lto = true
# Parallel Code Generation Units
codegen-units = 1
# Strip source code
strip = true
# Compilation behavior.
panic = "abort"

[profile.dev]
# Incremental compilation
incremental = true
# Disable debug
debug=false
# Optimization Level
opt-level = 0
# Link Time Optimization (LTO)
lto = true
# Parallel Code Generation Units
codegen-units = 1
# Strip source code
strip = true
# Compilation behavior.
panic = "unwind"

# Production App. dependencies
[dependencies]

# Dev App. dependencies
[dev-dependencies]
```

## Crates optimization

[Okno's Blog (Ref)](https://oknozor.github.io/blog/optimize-rust-binary-size/)

### Crates size

- Get `crate` size

```shell
# After comment 'strip option'
$ cargo bloat --crates --release
```

- Duplicate crates

```shell
$ cargo tree --duplicates -e=no-dev
```
- Graph dependencies

```shell
cargo depgraph | dot -Tpng > graph.png
```