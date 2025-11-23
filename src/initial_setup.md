# Initial Project Setup

Use one library crate (`truck_meshes`) where each shape lives in its own file and is re-exported from `src/lib.rs`.

## 1) Create the workspace

```bash
cargo new --lib truck_meshes
cd truck_meshes
mkdir -p examples
```

## 2) Add dependencies

`Cargo.toml`:

```toml
[dependencies]
truck-meshalgo = "0.6.0"
```

## 3) Seed `src/lib.rs`

Include the helper used by all examples and re-export shape modules as you add them:

```rust
use std::iter::FromIterator;
use truck_meshalgo::prelude::*;

/// Write any mesh to an OBJ file.
pub fn write_polygon(mesh: &PolygonMesh, path: &str) {
    let mut obj = std::fs::File::create(path).unwrap();
    obj::write(mesh, &mut obj).unwrap();
}

// Add shapes here as modules (one file per shape)
// pub mod triangle;
// pub use triangle::triangle;
```

## 4) Add shapes in their own files

Example: `src/triangle.rs` with `pub fn triangle() -> PolygonMesh { ... }`, then update `src/lib.rs`:

```rust
pub mod triangle;
pub use triangle::triangle;
```

Repeat for `square`, `tetrahedron`, `hexahedron`, `octahedron`, `dodecahedron`, `icosahedron`.

## 5) Export from examples

Place small runners in `examples/` to write OBJs, e.g. `examples/triangle.rs`:

```rust
fn main() {
    let mesh = truck_meshes::triangle();
    truck_meshes::write_polygon(&mesh, "triangle.obj");
}
```

Run with `cargo run --example triangle`.

<details>
<summary>File tree after adding the triangle</summary>

```
truck_meshes/
├─ Cargo.toml
├─ src/
│  ├─ lib.rs
│  └─ triangle.rs
└─ examples/
   └─ triangle.rs
```

</details>
