# First Triangle


## Add the triangle module to lib.rs

`src/lib.rs` (root exports + helper):

```rust
use std::iter::FromIterator;
use truck_meshalgo::prelude::*;

/// Write any mesh to an OBJ file.
pub fn write_polygon(mesh: &PolygonMesh, path: &str) {
    let mut obj = std::fs::File::create(path).unwrap();
    obj::write(mesh, &mut obj).unwrap();
}

pub mod triangle; //add this
pub use triangle::triangle; //add this
```

`src/triangle.rs`:

```rust
use std::iter::FromIterator;
use truck_meshalgo::prelude::*;

/// A single equilateral triangle in the XY plane.
pub fn triangle() -> PolygonMesh {
    let positions = vec![
        Point3::new(0.0, 0.0, 0.0),
        Point3::new(1.0, 0.0, 0.0),
        Point3::new(0.5, f64::sqrt(3.0) / 2.0, 0.0),
    ];

    let attrs = StandardAttributes {
        positions,
        ..Default::default()
    };

    let faces = Faces::from_iter([[0, 1, 2]]);

    PolygonMesh::new(attrs, faces)
}
```

## Export the triangle

From the crate root:

```bash
cargo test -- --nocapture
```

Or add a tiny example at `examples/triangle.rs`:

```rust
fn main() {
    let mesh = truck_meshes::triangle();
    truck_meshes::write_polygon(&mesh, "triangle.obj");
}
```

Run it:

```bash
cargo run --example triangle
```

## View it

Open `triangle.obj` in Preview/3D Viewer/ParaView/Blender. You should see a single triangle.

<details>
<summary>File tree after this step</summary>

```
truck_meshes/
├─ Cargo.toml
├─ src/
│  ├─ lib.rs
│  └─ triangle.rs
└─ examples/
   └─ triangle.rs   (optional helper to export)
```

</details>

<details>
<summary>Full code:</summary>

`src/lib.rs`:

```rust
use std::iter::FromIterator;
use truck_meshalgo::prelude::*;

pub fn write_polygon(mesh: &PolygonMesh, path: &str) {
    let mut obj = std::fs::File::create(path).unwrap();
    obj::write(mesh, &mut obj).unwrap();
}

pub mod triangle;
pub use triangle::triangle;
```

`src/triangle.rs`:

```rust
use std::iter::FromIterator;
use truck_meshalgo::prelude::*;

pub fn triangle() -> PolygonMesh {
    let positions = vec![
        Point3::new(0.0, 0.0, 0.0),
        Point3::new(1.0, 0.0, 0.0),
        Point3::new(0.5, f64::sqrt(3.0) / 2.0, 0.0),
    ];

    let attrs = StandardAttributes {
        positions,
        ..Default::default()
    };

    let faces = Faces::from_iter([[0, 1, 2]]);

    PolygonMesh::new(attrs, faces)
}
```
`examples/triangle.rs`:

```rust
fn main() {
    let mesh = truck_meshes::triangle();
    truck_meshes::write_polygon(&mesh, "triangle.obj");
}
```

</details>
