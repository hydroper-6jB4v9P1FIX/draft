# Static variables

`static` variables use a DAG (directed acyclic graph) to determine initialization and are lazily-initialized as if by using `Lazy<T>` internally.

`static`s can't be cyclic.