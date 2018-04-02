unity-path-finding
=====================

![Demo](https://raw.githubusercontent.com/mattatz/unity-path-finding/master/Captures/Demo.gif)

Shortest path finding with Dijkstra's algorithm for Unity.

## Usage (2D grid nodes example)

```cs
var nodes = new List<Node>();
var edges = new List<Edge>();

// grid size
int width = 10, height = 10;

// create nodes on grid

for(int y = 0; y < height; y++)
{
    for(int x = 0; x < width; x++)
    {
        var node = new Node3D(new Vector3(x, 0, y));
        nodes.Add(node);
    }
}

// connect edges between neighbor nodes on grid
for(int y = 0; y < height; y++)
{
    var yoff = y * width;
    for(int x = 0; x < width; x++)
    {
        var idx = yoff + x;
        var node = nodes[idx];

        // connect horizontally
        if(x < width - 1)
        {
            var to = nodes[idx + 1];
            var e = node.Connect(to, Vector3.Distance((node as Node3D).Position, (to as Node3D).Position));
            edges.Add(e);
        }

        // connect vertically
        if(y < height - 1)
        {
            var to = nodes[idx + width];
            var e = node.Connect(to, Vector3.Distance((node as Node3D).Position, (to as Node3D).Position));
            edges.Add(e);
        }
    }
}

// create graph
var graph = new Graph(nodes, edges);

// build a Route with shortest path finding from source
int source = 0; // source node index
Route route = graph.Find(source);

// traverse a route from source to destination
int destination = 84; // destination node index

// traverse nodes from destination to source
List<Node> nodes = route.Traverse(graph, destination);
for(int i = 0, n = nodes.Count - 1; i < n; i++)
{
    var from = nodes[i];
    var to = nodes[i + 1];

    // draw line between from and to nodes
    Gizmos.DrawLine((from as Node3D).Position, (to as Node3D).Position);
}

```

## Sources

- Dijkstra's algorithm - Wikipedia - https://en.wikipedia.org/wiki/Dijkstra's_algorithm
- Priority Queue implementation from Microsoft/MixedRealityToolkit-Unity - https://github.com/Microsoft/MixedRealityToolkit-Unity
