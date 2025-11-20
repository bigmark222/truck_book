2.4 Normals

In this section, we’ll learn how to add normals to a mesh in Truck.

Normals are important because they determine:

how light interacts with the surface

whether a face appears smooth or flat

how shading algorithms interpret the geometry

Truck gives you tools to compute normals automatically or supply them manually.

What Are Normals?

A normal is a vector that points “outward” from a surface.

There are two kinds commonly used in graphics:

Face normals

One normal per polygon

Makes the object look faceted (each face is flat)

Useful for mechanical shapes (cubes, pyramids, polyhedra)

Vertex normals

One normal per vertex

Usually computed as the average of adjacent face normals

Makes the object look smooth (smooth shading on curved surfaces)

Truck supports both.

Adding Normals With Truck

Normals are stored inside the mesh’s attribute structure, StandardAttributes.
To compute them, Truck provides functions in:

truck_meshalgo::filters


The most common ones are:

complete_face_normals(&mesh)

complete_vertex_normals(&mesh)

These add missing normals to the mesh and return a new mesh with normals filled in.

Example — Computing Normals for a Cube

Let’s compute both face normals and vertex normals for our cube mesh.

use truck_meshalgo::prelude::*;
use truck_meshalgo::filters::*;

fn main() {
    let cube = hexahedron();

    // Add face normals
    let cube_with_face_normals = complete_face_normals(&cube);

    // Add vertex normals
    let cube_with_vertex_normals = complete_vertex_normals(&cube);

    // Save both versions
    write_obj(&cube_with_face_normals, "cube-face-normals.obj");
    write_obj(&cube_with_vertex_normals, "cube-vertex-normals.obj");
}

fn write_obj(mesh: &PolygonMesh, path: &str) {
    let mut file = std::fs::File::create(path).unwrap();
    obj::write(mesh, &mut file).unwrap();
}


You will see the difference when visualizing them:

The face-normal version will look sharp and faceted.

The vertex-normal version will look smooth—even though the cube geometry hasn’t changed.

How Truck Computes Normals

Under the hood:

Face normals:

computed by taking the cross product of two edges in the polygon

normalized to length 1

attached to each face

Vertex normals:

computed by averaging the normals of all adjacent faces

weighted by face angle or area depending on the implementation

stored per vertex

Truck handles all of this automatically.

Why Normals Matter in CAD + Rendering

Normals impact:

lighting

shading

smoothness perception

how meshes appear in viewers

downstream rendering pipelines (WebGPU, Vulkan, OpenGL)

Even if the mesh geometry is perfect, missing or incorrect normals can make a model look:

inside-out

flat when it should be smooth

smooth when it should be sharp

strangely lit

inconsistent across faces

Computing normals early avoids these artifacts.

When Should You Recalculate Normals?

Recalculate normals when:

you modify vertex positions

you add or remove faces

you merge or simplify meshes

you convert from B-rep or CSG to mesh form

the mesh comes from a source that does not include normal data