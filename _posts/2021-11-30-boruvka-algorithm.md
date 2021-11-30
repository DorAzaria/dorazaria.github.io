---
title:  "Borůvka's Algorithm"
layout: single
date:   2021-11-30 10:45:54
author_profile: true
read_time: true
show_date: true
related: true
categories:
  - Algorithms
tags:
  - Algorithms
  - ComputerScience
  - MST
comments: true
share: true
related: true
toc: true
toc_sticky: true
usemathjax: true
---

Borůvka's Algorithm is a greedy algorithm published by Otakar Borůvka, a Czech mathematician best known for his work in graph theory. 
Its most famous application helps us find the minimum spanning tree in a graph.

<iframe width="560" height="315" src="https://www.youtube.com/embed/t92xyTDvl_c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Problem Description
* **Input**: An undirected, weighted and connected graph $G=(V,E)$ .
* **Output**: A minimal spanning tree (MST).

## The Method
* We begin with individual components (each one represents a node).
* We initialize the minimum spanning tree $T$ as empty.
* While there is more than one component -  we'll find the minimum-weight edge that connects the component to any other component and does not making a circle in the graph. 
	- If the edge isn't in $T$, we add it.
* Return $T$ which contains only one component (MST).

![](https://www.google.com/url?sa=i&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FBor%25C5%25AFvka%2527s_algorithm&psig=AOvVaw1PP_SUs_wMksQ4GAxGcFA_&ust=1638349927244000&source=images&cd=vfe&ved=0CAsQjRxqFwoTCLD53Z7fv_QCFQAAAAAdAAAAABAI)

### Time Complexity
One advantage that Borůvka's algorithm has compared to the alternatives (e.g. Kruskal and Prim) is that it doesn't need to presort the edges or maintain a priority queue in order to find the minimum spanning tree. Even though that doesn't help its complexity, since it still passes the edges $log|E|$ times, it is a bit more simple to code.
Therefore, the time complexity of this algorithm is $O(|E| \log |V|)$ .

### Algorithm Pseudocode

```java
Boruvka(G)

	for each v∈V(G) do: // O(|V|)
		MakeSet(v)
	end-for

	create Tree T ⇐ ∅
	treeSize ⇐ 0

	while treeSize < |V(G)|-1 do:
		create Edge[|V(G)|] cheapest // init NULL

		for each edge∈E(G) do:
			root1 ⇐ FindSet(edge.v1)
			root2 ⇐ FindSet(edge.v2)

			if root1 ≠ root2 then:

				if cheapest[root1] = NULL OR 
				OR edge.weight < cheapest[root1].weight then:

						cheapest[root1] ⇐ edge
				end-if

				if cheapest[root2] = NULL OR 
				OR edge.weight < cheapest[root2].weight then:

					cheapest[root2] ⇐ edge
				end-if
			end-if
		end-for

		for each v∈V(G) do:
			if cheapest[v] ≠ NULL then:
				v1 ⇐ cheapest[v].v1()
				v2 ⇐ cheapest[v].v2()

				if FindSet(v1) ≠ FindSet(v2) then:
					treeSize ⇐ treeSize + 1
					edge ⇐ new Edge(v1, v2, cheapest[v].weight)
					T.add(edge)
					Union(v1,v2)
				end-if
			end-if
		end-for
	end-while

	return T
end-Boruvka

MakeSet(v): // O(1)
	v.parent ⇐ v
end-MakeSet

FindSet(v): // O(log|V|)
	if v = v.parent then:
		return v.parent
	else:
		return FindSet(v.parent)
	end-if
end-FindSet

Union(u,v): // O(log|V|)
	uRoot ⇐ FindSet(u)
	vRoot ⇐ FindSet(v)

	uRoot.parent ⇐ vRoot
end-Union

```

