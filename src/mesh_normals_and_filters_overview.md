# Mesh Normals and Filters

This part of the book shows how to clean up meshes after tessellation:

- **Normals**: use `NormalFilters` (`add_naive_normals`, `add_smooth_normals`, `normalize_normals`) or helper functions like `complete_face_normals` / `complete_vertex_normals` to fix missing or noisy lighting data.
- **Optimization**: run mesh filters (`OptimizingFilter`, `StructuringFilter`) to merge duplicated vertices, drop degenerate faces, and remove unused attributes so files stay small and render quickly.
- **Subdivision / refinement**: apply `Subdivision` filters to increase smoothness when a coarse mesh needs more detail.

Read `mesh_normals.md` for normal strategies and `mesh_filters.md` for cleanup/subdivision pipelines you can apply before exporting or rendering.
