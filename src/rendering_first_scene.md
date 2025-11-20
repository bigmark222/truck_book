# First Scene

Load a teapot OBJ, build a camera/light/model scene, and render it with truck-platform + truck-rendimpl.

## Imports and app skeleton

Reuse `app.rs` from the previous section.

```rust
mod app;
use app::*;

use std::sync::Arc;
use truck_platform::*;     // GPU abstraction + scene graph
use truck_rendimpl::*;     // renderer implementation
use winit::window::Window;
```

Application with a `WindowScene`:

```rust
struct MyApp {
    scene: WindowScene,
}

#[async_trait(?Send)]
impl App for MyApp {
    async fn init(window: Arc<Window>) -> Self {
        // build scene
        MyApp { scene }
    }

    fn render(&mut self, frame: &SwapChainFrame) {
        self.scene.render_scene(&frame.output.view);
    }
}

fn main() {
    MyApp::run()
}
```

## Build the scene (inside `init`)

### Camera

```rust
let mut camera = Camera::default();
camera.matrix = Matrix4::look_at(
    Point3::new(5.0, 6.0, 5.0),  // eye
    Point3::new(0.0, 1.5, 0.0),  // target
    Vector3::unit_y(),           // up
)
.invert()
.unwrap(); // cgmath -> world-space transform
```

### Light

```rust
let mut light = Light::default();
light.position = camera.position(); // simple headlight
```

### Scene descriptor

```rust
let scene_desc = WindowSceneDescriptor {
    studio: StudioConfig {
        camera,
        lights: vec![light],
        ..Default::default()
    },
    ..Default::default()
};

let mut scene = WindowScene::from_window(window, &scene_desc).await;
```

## Load and add the teapot

Place `teapot.obj` in `src/`, then:

```rust
let polygon: PolygonMesh =
    polymesh::obj::read(include_bytes!("teapot.obj").as_ref()).unwrap();

let instance = scene.create_instance(&polygon, &Default::default());
scene.add_object(&instance);
```

Instancing lets multiple objects share GPU mesh data.

## Return the app

```rust
MyApp { scene }
```

## Run

```bash
cargo run
```

You should see a lit, shaded 3D teapot. Key pipeline steps: configure camera/light, build `WindowScene`, load OBJ → GPU instance, render each frame with `scene.render_scene(...)`.
