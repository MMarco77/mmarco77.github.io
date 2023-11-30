# Exemple of error management

Good reference: [Designing error types in Rust](https://mmapped.blog/posts/12-rust-error-handling.html)

## TCP Echo project

```
Project
    ├── Cargo.toml
    ├── index.html
    └── src
        ├── error.rs
        ├── lib.rs
        └── main.rs
```
- Cargo.toml

```toml
[package]
name = "tcp_echo"
version = "0.1.0"
edition = "2021"

[dependencies]
```

- index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Rust</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

- error.rs

```rs
#[derive(Debug)]
pub enum AppError {
    Io(std::io::Error),
    Utf8(std::string::FromUtf8Error),
    General(String),
}

impl std::fmt::Display for AppError {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        match self {
            AppError::Io(e) => write!(f, "IO error: {}", e),
            AppError::Utf8(e) => write!(f, "UTF-8 error: {}", e),
            AppError::General(s) => write!(f, "General error: {}", s),
        }
    }
}

impl From<std::io::Error> for AppError {
    fn from(e: std::io::Error) -> Self {
        Self::Io(e)
    }
}
impl From<std::string::FromUtf8Error> for AppError {
    fn from(e: std::string::FromUtf8Error) -> Self {
        Self::Utf8(e)
    }
}

pub type AppResult<T> = Result<T, AppError>;

```

- lib.rs

```rs
pub mod error;
```

- main.rs

```rs
use std::io::BufReader;
use std::io::prelude::*;
use std::net::TcpListener;
use std::net::TcpStream;

use tcp_echo::error::AppResult;

fn main() {
    match start_server("127.0.0.1:8080") {
        Ok(_) => println!("Server ending"),
        Err(_) => println!("Server failed"),
    }
}

fn start_server(url: &str) -> AppResult<()> {
    let listener = TcpListener::bind(url)?;
    println!("Listening on port 8080");

    for stream in listener.incoming() {
        let stream = stream?;
        println!("Connection established!");
        handle_connection(stream);
    }
    Ok(())
}

fn handle_connection(mut stream: TcpStream) {
    let buf_reader = BufReader::new(&mut stream);
    let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();
    println!("Request: {:#?}", http_request);
}
```
