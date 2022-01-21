----

----

 # This repository has moved
 
 Please update your links to the [wapc/wapc-rs](https://github.com/wapc/wapc-rs) repository.
 
----

----


# Wasmtime Engine Provider

This is a pluggable engine provider for the [waPC](https://github.com/wapc) RPC exchange protocol. This engine encapsulates 
the Bytecode Alliance [wasmtime](https://github.com/bytecodealliance/wasmtime) WebAssembly runtime.

To run the demo:
```
cargo run --example demo -- ./.assets/hello.wasm test
```

An example of using this engine provider:
```rust
pub fn main() -> Result<(), Box<dyn std::error::Error + Send + Sync>> {
    env_logger::init();
    
    let module_bytes = load_file(&std::env::args().skip(1).next().unwrap());
    let engine = WasmtimeEngineProvider::new(&module_bytes, None);
    let host = WapcHost::new(Box::new(engine), host_callback)?;
    let func = std::env::args().skip(2).next().unwrap();

    let _res = host.call(&func, b"this is a test")?;
    Ok(())
}

fn host_callback(
    id: u64,
    bd: &str,
    ns: &str,
    op: &str,
    payload: &[u8],
) -> Result<Vec<u8>, Box<dyn std::error::Error + Send + Sync>> {
    println!(
        "Guest {} invoked '{}->{}:{}' with payload of {}",
        id,
        bd,
        ns,
        op,
        ::std::str::from_utf8(payload).unwrap()
    );
    Ok(vec![])
}
```
