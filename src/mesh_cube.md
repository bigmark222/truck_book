# Cube

Now let's build a cube (a [hexahedron](https://en.wikipedia.org/wiki/Hexahedron)) from vertex lists and quad faces.

Reference: [github.com/ricosjp/truck-tutorial-code/blob/v0.6/chapter2/src/section2_2.rs](https://github.com/ricosjp/truck-tutorial-code/blob/v0.6/chapter2/src/section2_2.rs)

## Cube mesh

```rust
/// Create a cube
fn cube() -> PolygonMesh {
    let positions = vec![
        Point3::new(0.0, 0.0, 0.0),
        Point3::new(1.0, 0.0, 0.0),
        Point3::new(1.0, 1.0, 0.0),
        Point3::new(0.0, 1.0, 0.0),
        Point3::new(0.0, 0.0, 1.0),
        Point3::new(1.0, 0.0, 1.0),
        Point3::new(1.0, 1.0, 1.0),
        Point3::new(0.0, 1.0, 1.0),
    ];

    let attrs = StandardAttributes {
        positions,
        ..Default::default()
    };

    let faces = Faces::from_iter([
        [3, 2, 1, 0], // bottom
        [0, 1, 5, 4], // front
        [1, 2, 6, 5], // right
        [2, 3, 7, 6], // back
        [3, 0, 4, 7], // left
        [4, 5, 6, 7], // top
    ]);

    PolygonMesh::new(attrs, faces)
}
```

## Save to OBJ

```rust
/// Output the contents of `polygon` to the file specified by `path`.
fn write_polygon(polygon: &PolygonMesh, path: &str) {
    let mut obj = std::fs::File::create(path).unwrap();
    obj::write(polygon, &mut obj).unwrap();
}

fn main() {
    write_polygon(&cube(), "cube.obj");
}
```

Run:

```bash
cargo run
```
