# Walker Layout

This package provides a way to generate level-based tree layouts (i.e. (x, y) coordinates for each node in a tree graph, such that nodes with the same depth from the root node are placed on the same y-height) using Walker's algorithm (https://onlinelibrary.wiley.com/doi/abs/10.1002/spe.4380200705) including improvements by Buchheim et al. (2002) to make it linear in time (https://link.springer.com/content/pdf/10.1007/3-540-36151-0_32.pdf). One can then use these layouts to draw the graph in the xy-plane.

You can either directly supply a networkx graph:

```python
from walkerlayout import WalkerLayouting
import networkx as nx

G = nx.Graph()
G.add_nodes_from(range(0,8))
G.add_edges_from([(0,1), (0,2), (0,3), (1,4), (3,5), (3,6), (5,7)])

layout = WalkerLayouting.layout_networkx(graph=G, root_node=0)

nx.draw(G, pos=layout, with_labels=True)
```

Or define the tree purely in `WalkerLayouting` using unique keys:

```python
from walkerlayout import WalkerLayouting
#   0
#  /|\
# 1 2 3
# |   /\
# 4  5  6
#    |
#    7
tree = WalkerLayouting(root_key=0)
tree.add(parent_key=0, child_key=1)
tree.add(parent_key=0, child_key=3)
tree.add(parent_key=0, child_key=2, index=1)
tree.add(parent_key=3, child_key=6)
tree.add_from([(1,4), (3,5,0), (5, 7), (5, 8), (8, 9)]) # (parent, child [,index])
tree.remove(8)

layout = tree.get_layout()
# {
#  0: (0.0, 0.0), 
#  1: (-1.0, 1.0), 
#  2: (0.0, 1.0), 
#  3: (1.0, 1.0), 
#  4: (-1.0, 2.0), 
#  5: (0.5, 2.0), 
#  6: (1.5, 2.0), 
#  7: (0.5, 3.0)
# }
```

Keys can be of any type. However, you can type-hint them to a fixed type:

```python
tree: WalkerLayouting[str] = WalkerLayouting(root_key='root')
```

# LICENSE (MIT)

Copyright 2023 Elias Foramitti

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.