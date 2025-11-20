# Mesh Filters

Truck includes several helpful utilities—called mesh filters—that let you clean, modify, and analyze meshes.
In this section, we’ll look at:

Topological conditions (regular, oriented, closed, etc.)

Merging duplicate vertices

Adding or overriding normals

Smoothing normals

These operations are important when preparing meshes for rendering, simulation, or export.

The completed example code is available at:

https://github.com/ricosjp/truck-tutorial-code/blob/v0.6/chapter2/src/section2_5.rs

Mesh Topology Conditions

Truck can classify a mesh based on its topology. The most common classifications are:

Irregular

An edge has three or more faces touching it.

Regular

Each edge has at most two faces attached.

Oriented

No two edges appear more than twice with the same direction.

Closed

Every edge appears exactly twice, once in each direction
→ This is what you want for watertight, manifold solids.

Truck can check these properties using:

mesh.shell_condition()

Example: Checking the Topology of a Sphere Mesh

If you created a sphere using the earlier code, it might look smooth externally but internally be stored in multiple patches with repeated coordinates.

To inspect the mesh:

use truck_meshalgo::prelude::*;

// Read the sphere OBJ file created earlier
let mut mirror_ball = obj::read(include_bytes!("sphere.obj").as_slice()).unwrap();

println!(
    "default mirror ball shell condition: {:?}",
    mirror_ball.shell_condition()
);


You will likely see:

Oriented but not Closed

Why?
Because the sphere is made of several patches where the same coordinates are duplicated at different indices.

Even if two vertices share the same XYZ position, they are treated as separate vertices unless merged manually.

Merging Duplicate Vertices

To merge vertices that have identical (or nearly identical) attribute values, use:

mirror_ball.put_together_same_attrs(1.0e-3);


The argument (1.0e-3) is the tolerance.
Vertices within this distance are treated as identical.

After merging:

println!(
    "after apply filter `put_together_same_attrs`: {:?}",
    mirror_ball.shell_condition()
);


Now the mesh becomes Closed.

This is extremely useful when:

importing models from other software

exporting triangulated surfaces

converting from procedural geometry

removing seams created by subdivision or parameterization

Adding Normals

Next, let’s override or add normals to the mesh.

If you want a “mirror ball” effect (sharp reflections), you can compute simple face-based normals:

mirror_ball.add_naive_normals(true);


The true argument means:

overwrite existing normals

After adding normals, save the mesh:

write_polygon(&mirror_ball, "mirror-ball.obj");


(Use the same write_polygon helper from previous sections.)

Adding Smooth Normals

Truck also includes a function for smooth shading, similar to how macOS Preview or Blender smooths meshes.

mirror_ball.add_smooth_normals(1.0, true);


Arguments:

Angle threshold (radians) — controls edge preservation

If two faces meet at an angle greater than this value, their normals are not smoothed together.

Setting 1.0 rad (~57°) means “smooth almost everything.”

Overwrite (true/false) — replace existing normals

Finally, save the smoothed mesh:

write_polygon(&mirror_ball, "mirror-ball-with-smooth-normal.obj");

When Should You Use These Filters?
Use put_together_same_attrs when:

importing OBJ files

converting from patches (NURBS → mesh)

fixing seams

merging duplicated geometry

Use add_naive_normals when:

rendering sharp mechanical or faceted surfaces

you want crisp reflections

debugging face orientations

Use add_smooth_normals when:

rendering curved shapes (spheres, organic models)

you want soft lighting

preparing assets for real-time graphics