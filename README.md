Flamenco
=========

[Main Image]

### Description

A GPU position-based dynamics (PBD) cloth simulation sufficiently fast and robust for use in games. Our take on this well-studied problem is an amalgam of some of the industry's best PBD cloth methods, some dating back as far as 2003, some recently communicated in GDC 2018, all mixed and ported to the GPU. By parallelizing these methods on the GPU, we achieve frame rates far higher than their CPU-based counterparts, and easily satisfy the game industry's 60 fps standard.

### Links

This repository is a placeholder for the Project. You can track this progress by going to (any of the following links):

- [GPU Cloth Sim Issue Tracker: Issue #47](https://github.com/vasumahesh1/azura/issues/47)
- [CPU Cloth Sim Issue Tracker: Issue #46](https://github.com/vasumahesh1/azura/issues/46)
- [Main Docs (Will be updated when complete)](https://vasumahesh1.github.io/azura_docs/)

The issue tracker has screenshots reporting progress. All commits related to them are tagged as "Issue #XYZ" format.

This project builds online as we commit to it. The latest build can be run and is available at [Appveyor](https://ci.appveyor.com/project/vasumahesh1/azura/history). Check for the latest master build with `WIN64_RELEASE` tag and download the `3_ClothSim` zipped executable.

[![Build Status: Windows](https://ci.appveyor.com/api/projects/status/github/vasumahesh1/azura)](https://ci.appveyor.com/project/vasumahesh1/azura)

This will download all the necessary config / logging / shaders / textures etc. to run the application, all as one zip.

### Methodology

#### PBD Algorithm

We write the core PBD algorithm here for convenience:

```
ALGORITHM Position Based Dyanmics:
  for all vertices i do
    initialize x_i, vi, wi = 1/mi 
  end for
  loop
    for all vertices i do vi = vi + wi * dt * f_ext
    for all vertices i do pi = xi + dt * vi
    for all vertices i do genCollConstraints(xi, pi)
    loop solverIterations times
      projectConstraints()
    end loop
    for all vertices i do
      vi = (pi - xi) / dt
      xi = pi
    end for
  end loop
END.
```

#### GPU-Based PBD Solver

The PBD algorithm is typically evaluated on the CPU using a Gauss-Seidel type solver, which works exclusively in a serial fashion. Porting the PBD algorithm therefore requires a different approach. Researchers typically choose one of two methods - A Jacobi iterative solver or a graph coloring algorithm. The graph coloring method identifies independent sets of vertices (those not linked by constraint functions) then solves the constraints associated with each of these sets in a serial fashion using the Gauss-Seidel method. While this method guarantees convergence, it limits vertex/constraint throughput on the GPU and is still inherently serial. The Jacobi method, on the other hand, maximizes parallelism but does not guarantee convergence without the following adjustment to the PBD algorithm: the change in position is computed for each vertex i for all constraints that apply to i in parallel. The final correction applied to i, however, is the average of these adjustments.

#### Geometric Constraints

Our cloth model includes four distinct geometric constraints intended to approximate real cloth behavior: distance, isometric bending, long-range attachments, and anchor constraints. We briefly describe each of these here.

##### Distance
##### Isometric Bending
##### Long-Range Attachments
##### Anchors

#### Environmental Collisions

#### Mesh Definition

#### Spatial Hashing with Predictive Constraints for Self-Collisions

### Implementation

This project was written for an engine being developed by one of the authors (see links above). This engine builds to D3D12 and Vulkan for rendering, but for this particular project we restrict ourselves to the D3D12 build.

### References

1. Jan Bender, Matthias Müller, and Miles Macklin, [Position-Based Simulation Methods in Computer Graphics](http://mmacklin.com/EG2015PBD.pdf)
2. Chris Lewin, [Cloth Self-Collision with Predictive Contacts](https://media.contentapi.ea.com/content/dam/eacom/frostbite/files/gdc2018-chrislewin-clothselfcollisionwithpredictivecontacts.pdf)
3. Marco Fratarcangeli and Fabio Pellacini, [A GPU-Based Implementation of Position Based Dynamics for Interactive Deformable Bodies](http://publications.lib.chalmers.se/records/fulltext/219708/local_219708.pdf)
4. Matthias Müller, Bruno Heidelberger, Marcus Hennix, and John Ratcliff, [Position Based Dynamics](http://matthias-mueller-fischer.ch/publications/posBasedDyn.pdf)
5. Matthias Teschner, Bruno Heidelberger, Matthias Müller, Danat Pomeranets, and Markus Gross, [Optimized Spatial Hashing for Collision Detection of Deformable Objects](http://matthias-mueller-fischer.ch/publications/tetraederCollision.pdf)


### Made by:

* Vasu Mahesh
  * [LinkedIn](http://linkedin.com/in/vasumahesh)
  * [Code Blog](http://www.codeplaysleep.com)

* Zach Corse
  * [LinkedIn](https://www.linkedin.com/in/wzcorse/)
  * [Personal Website](https://wzcorse.com)
