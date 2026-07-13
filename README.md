# ⚠️ IMPORTANT — this is a patched fork, not upstream

**What this is:** a fork of [`Kixiron/size-of`](https://github.com/Kixiron/size-of) `0.1.5`, used
only as a Cargo `[patch]` by the Midnight support in
[`equilibriumco/hyperlane-monorepo`](https://github.com/equilibriumco/hyperlane-monorepo). We
never call `size-of` ourselves — it arrives transitively through the Starknet chain tooling
(`hyperlane-starknet -> cainome -> starknet-types-core -> size-of`).

**The change (one macro, in `src/core_impls.rs`):** the `impl_function_ptrs!` list drops the 6
target-specific calling-convention ABIs (`aapcs`, `cdecl`, `win64`, `sysv64`, `stdcall`,
`fastcall`), keeping only the universal `"C"`, `"Rust"`, `"system"`.

**Why:** upstream declares those ABIs for *every* target. Old rustc ignored an unsupported ABI;
**rustc >= 1.95 turns it into a hard error (E0570)** (e.g. `aapcs` on aarch64-apple-darwin). Our
Midnight build is pinned to rustc >= 1.95 (the ledger-9.1 crates pull `sysinfo 0.39`, which
requires it), and `0.1.5` is the last release under the `size-of` name (development continued as
the renamed `feldera-size-of`), so it can't simply be bumped. The dropped impls are never
instantiated by our only consumer, so removing them is functionally a no-op.

**Delete this fork** once upstream gates its ABI list by target, or once the dependency stops
reaching us.

---

![Crates.io](https://img.shields.io/crates/v/size-of)
![docs.rs](https://img.shields.io/docsrs/size-of)

# Size Of

A crate for measuring the total memory usage of an object at runtime

## Features

`size-of` has built-in support for many 3rd party crates that can be enabled with feature flags

- `std`: Enables support for the rust standard library (enabled by default, when disabled `size-of` is `#![no_std]` compatible)
- `derive`: Enables support for `#[derive(SizeOf)]` (enabled by default)
- `time`: Enables support for the [`time`](https://docs.rs/time) crate
  - `time-std`: Enables support for `time`'s `std` feature
- `chrono`: Enables support for the [`chrono`](https://docs.rs/chrono) crate
- `hashbrown`: Enables support for the [`hashbrown`](https://docs.rs/hashbrown) crate
- `fxhash`: Enables support for the [`fxhash`](https://docs.rs/fxhash/latest/fxhash) crate
- `rust_decimal`: Enables support for the [`rust_decimal`](https://docs.rs/rust_decimal) crate
- `ordered-float`: Enables support for the [`ordered-float`](https://docs.rs/ordered-float) crate
- `ahash`: Enables support for the [`ahash`](https://docs.rs/ahash) crate
  - `ahash-std`: Enables support for `ahash`'s `std` feature
- `xxhash-rust`: Enables support for the [`xxhash-rust`](https://docs.rs/xxhash-rust) crate
  - `xxhash-xxh32`: Enables support for `xxhhash-rust`'s `xxh32` feature
  - `xxhash-xxh64`: Enables support for `xxhhash-rust`'s `xxh64` feature
  - `xxhash-xxh3`: Enables support for `xxhhash-rust`'s `xxh3` feature
- `bigdecimal`: Enables support for the [`bigdecimal`](https://docs.rs/bigdecimal) crate
- `num-bigint`: Enables support for the [`num-bigint`](https://docs.rs/num-bigint) crate
