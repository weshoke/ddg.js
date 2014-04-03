ddg.js
=========

#### Discrete Differential Geometry in Javascript ####

Differential geometry studies how differential elements like tangents and normals transform when mapped between spaces.  Surfaces, for example, can be mapped to a plane as long as you're allowed to stretch them out.  Differential geometry allows us to quantify how this stretching out affects the surface and any calculations we might want to perform on the surface.

Discrete Differential Geometry (DDG) is simple differential geometry applied to the discrete setting of polygonal meshes.  DDG is built on top of Discrete Exterior Calculus (DEC), which is a unified framework for handling calculus over manifolds of varying dimensionality.  For a thorough and approachable explanation of DDG and DEC see the [Digital Geometry Processing with Discrete Exterior Calculus](http://www.cs.columbia.edu/~keenan/Projects/DGPDEC/) Siggraph 2013 course notes.


### Mesh Data Structure ###
Computing quanitites over a mesh requires being able to navigate a mesh.  This requires having adjacency information linking vertices and faces together through edges.  There are many mesh data structures with various tradeoffs in terms of computational and memory efficiency.  The halfedge data structure is a good compromise that is also commonly used, so will be familiar to those who have done mesh processing programming before.

The halfedge data structure takes its name from how it treats edges.  Edges are the boundary between two faces.  In the halfedge data structure, each edge is split in half with each half attached to a face on the boundary.  These are called __halfedges__.  Halfedges are the glue binding together the mesh.  They enable us to navigate not just between halfedges, but also around vertices and faces.

Every halfedge has an __opposite__.  The opposite halfedge is its partner on the boundary between two faces.  On the boundary, every halfedge is aligned in the counter-clockwise direction around its face.  Every halfedge also has an associated vertex.  Since every halfedge has a direction, it can be thought of as a vector originating at one vertex and terminating at another.  These are called the source and sink vertices respectively.  The vertex assocaited with a particular halfedge is the sink vertex.

In addition to an opposite, every halfedge has an associated __previous__ and __next__ halfedges that are used to circulate around a face.  Halfedges also have links to their __face__ and __vertex__.  Faces and vertices only have links to their associated halfedge.


### DEC + DDG ###
Discrete exterior calculus operations are generally carried out over a set of mesh elements if not the entire mesh at once.  Consequently, they're easiest to represent and compose as matrices.  Since a lot of the entries in the matrices are zero, DEC operations tend to use sparse matrices to save memory and computation.  ddg.js uses numeric.js for matrix and sparse matrix calculuations.

The currently supported DEC and DDG operations are:
* Hodge star 0-form and its inverse
* Hodge star 1-form and its inverse
* 0-form exterior derivative
* Laplacian
* Mean curvature normals