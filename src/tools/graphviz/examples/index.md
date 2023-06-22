# GraphViz

## Simple command

-  With `graphiviz` python module

```python
import graphviz

dot_data = """
// The Round Table
digraph {
    A [label="King Arthur"]
    B [label="Sir Bedevere the Wise"]
    L [label="Sir Lancelot the Brave"]
    A -> B
    A -> L
    B -> L [constraint=false]
}
"""
graph = graphviz.Source(dot_data)
graph
```
  
![svg](./assets/output_1_0.svg)

- With command line


```python
!dot -Tpng ./assets/source.dot -o ./assets/source.png
```

![image](./assets/output_1_0.svg)

## Some examples

## Basic

- graph

No direction

```text
graph {
  hello -- world;
}
```

![image](./assets/basic.png)

- digraph

oriented graph

```text
graph {
  hello -> world;
}
```

![image](./assets/digraph_basic.png)

- digraph oriented AND edge in color

```text
digraph {
  rankdir=LR;
  
  hello [ label = "Hello" ];
  hello -> world [ color="orange", penwidth=3.0 ];
}
```

![image](./assets/digraph_color_basic.png)

- digraph with font

```text
digraph {
  Tinos [ fontname="Tinos" ];
  Handlee [ fontname="Handlee" ];
  "Sedgwick Ave" [ fontname="Sedgwick Ave" ];
  "*also Sedgwick*";
}
```

![image](./assets/digraph_font_basic.png)

- digraph custom edge

```text
digraph {
  node [ shape=square ];
  edge [ style=dashed ];
  
  see -> think -> do;
}
```

![image](./assets/digraph_custom_edge.png)


### Simple Graph

```graphviz
graph {
    a -- b;
    b -- c;
    c -- d;
    d -- e;
    e -- f;
    a -- f;
    a -- c;
    a -- d;
    a -- e;
    b -- d;
    b -- e;
    b -- f;
    c -- e;
    c -- f;
    d -- f;
}
```
    
![svg](./assets/output_7_0.png)
    
### Full digraph

```python
import graphviz

dot_data = """
digraph {
    a -> b[label="0.2",weight="0.2"];
    a -> c[label="0.4",weight="0.4"];
    c -> b[label="0.6",weight="0.6"];
    c -> e[label="0.6",weight="0.6"];
    e -> e[label="0.1",weight="0.1"];
    e -> b[label="0.7",weight="0.7"];
}
"""
graphviz.Source(dot_data)
```
    
![svg](./assets/output_9_0.svg)
    
### Showing A path

```python
import graphviz

dot_data = """
graph {
    a -- b[color=red,penwidth=3.0];
    b -- c;
    c -- d[color=red,penwidth=3.0];
    d -- e;
    e -- f;
    a -- d;
    b -- d[color=red,penwidth=3.0];
    c -- f[color=red,penwidth=3.0];
}
"""
graphviz.Source(dot_data)
```
    
![svg](./assets/output_11_0.svg)

### Subgraph

```python
import graphviz

dot_data = """
digraph {
    subgraph cluster_0 {
        label="Subgraph A";
        a -> b;
        b -> c;
        c -> d;
    }

    subgraph cluster_1 {
        label="Subgraph B";
        a -> f;
        f -> c;
    }
}
"""
graphviz.Source(dot_data)
```

### Complex digraph

```python
import graphviz

dot_data = """
digraph G {
    size ="4,4";
    main [shape=box]; /* this is a comment */
    main -> parse [weight=8];
    parse -> execute;
    main -> init [style=dotted];
    main -> cleanup;
    execute -> { make_string; printf}
    init -> make_string;
    edge [color=red]; // so is this
    main -> printf [style=bold,label="100 times"];
    make_string [label="make a\nstring"];
    node [shape=box,style=filled,color=".7 .3 1.0"];
    execute -> compare;
}
"""
graphviz.Source(dot_data)
```
   
![image](./assets/output_15_0.png)
    
### Complex label

```text
digraph G {
    a -> b -> c;
    b -> d;
    a [shape=polygon,sides=5,peripheries=3,color=lightblue,style=filled];
    c [shape=polygon,sides=4,skew=.4,label="hello world"]
    d [shape=invtriangle];
    e [shape=polygon,sides=4,distortion=.7];
}
```

![image](./assets/label.png)

### Advanced Graphs

#### Edge connection

Use `<angle-brackets>` to connect edge

```text
digraph {
  rankdir=LR;
  node [ shape=record ];

  struct1 [
      label = "a|b|<port1>c";
  ];
  
  struct2 [
      label = "a|{<port2>b1|b2}|c";
  ];
  
  struct1:port1 -> struct2:port2 [ label="xyz" ];
}
```

![image](./assets/digraph_edge_connection.png)

#### `Clusters` (or `subgraph`)

You can group related nodes by putting them in a subgraph whose name begins with `cluster_`.

```text
digraph {
  node [ fontname="Handlee" ];
  subgraph cluster_frontend {
    label="*Frontend*";
    React;
    Bootstrap;
  }
      
  subgraph cluster_backend {
    label="*Backend*";
    expressjs;
    "aws-sdk";
  }

  React -> expressjs;
  expressjs -> "aws-sdk";
}
```

![image](./assets/digraph_subgraph.png)

