Phantom-algebra is a library implementing strongly-typed small tensors
with dimensions 0 ≤ 4, rank ≤ 2, and limited to square matrices.
It is inspired by GLSL conventions:

  * addition is the usual vector addition, with scalar broadcasted
    to tensors of any dimension and rank

  * `x * y` is interpreted as:
    * the external product if either `x` or `y` is a scalar
    * the matrix product if either `x` or `y` is a matrix
    * the component-wise (Hadamard) product otherwise
      (if both `x` and `y` are a vector)

  * the cross-product of two 2d vectors yields a scalar whereas
    the cross-product of two 3d vectors yields a 3d pseudo-vectors.
    (other cross-product are type errors).


  * index are also-strongly typed, trying to access a index beyond the
    tensor dimension yields a compiler-error.
    ```OCaml
    let v = vec2 2. 3.
    let fine = v.%(x)
    let wrong = v.%(z)
    let m = mat2 v v
    let fine = m.%(xy)
    let also_wrong = m.%(zx)
    let wrong_rank_this_time = m.%(x)
    ```

  * Similarly, slicing a rank `k` tensor with a rank `n` index
    yields a rank `k-n` tensor of the same dimension, e.g
    ```OCaml
    let e1 = (vec2 1. 0.)
    let id = mat2  e1 (vec2 0. 1.)
    let e1' = id.%[x] + e1 (* this is the first row of the id matrix *)
    let zero = id.%[xy]
    ```