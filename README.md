# stl-manifold-test-fixtures

Minimal ASCII STL test meshes (open boundary hole, inverted normal, watertight cube) and glTF-to-STL unit checklist for 3D printing slicers.

## 3-Facet Open Boundary vs. Watertight Tetrahedron

A closed, watertight tetrahedron requires 4 triangular facets where every edge is shared by exactly 2 faces (`V - E + F = 2`). Omitting the base facet produces a **3-facet open boundary loop** with three naked edges that slicers flag as non-manifold:

```stl
solid open_tetra_3_facets
  facet normal 0 -0.8944 0.4472
    outer loop
      vertex 0 0 0
      vertex 10 0 0
      vertex 5 5 10
    endloop
  endfacet
  facet normal 0.8944 0.4472 0.4472
    outer loop
      vertex 10 0 0
      vertex 5 8.66 0
      vertex 5 5 10
    endloop
  endfacet
  facet normal -0.8944 0.4472 0.4472
    outer loop
      vertex 5 8.66 0
      vertex 0 0 0
      vertex 5 5 10
    endloop
  endfacet
endsolid open_tetra_3_facets
```

Capping the missing fourth base loop `(0,0,0) -> (5,8.66,0) -> (10,0,0)` with outward counter-clockwise winding restores a watertight 4-facet manifold volume.

## glTF/GLB (Meters, Y-up) to Slicer STL (Millimeters, Z-up)

AI 3D generators typically export `.glb` / `.gltf` assets in **meters with Y-up** coordinates, whereas **OrcaSlicer** and **PrusaSlicer** expect unitless `.stl` coordinates in **millimeters with Z-up**. Always apply a `1000×` uniform scale and a `+90°` X-axis rotation (`(x, y, z) -> (1000x, -1000z, 1000y)`) before slicing, otherwise a 15 cm model imports as a microscopic `0.15 mm` speck lying on its back.

## Browser Mesh Diagnostic Tools

- [MeshFixer — Online STL Fixer](https://meshfixer.com/) — Inspect naked boundary edges, inverted normals, and auto-cap open holes in the browser.
- [GLB to STL Converter](https://meshfixer.com/glb-to-stl) — Convert meter-scale Y-up `.glb` exports into millimeter-scale Z-up `.stl` meshes for OrcaSlicer and PrusaSlicer.
- [STL Volume & Weight Calculator](https://meshfixer.com/volume-calculator) — Verify watertight signed volume (`cm³`) and filament mass estimates.

## License

MIT
