---
layout: post
title:  Graphs and Neural Networks
date:   2021-06-07 14:09:47 -0700
---

A very natural way to represent data is in the form of graphs. The graph data structure allows for a natural representation of objects and their relations with other objects. How graphs are constructed depend heavily on the application. 

A simple yet ubiquitous example of a graph is an image. Images are often stored in a rectangular grid. Due to the spatial nature of the grid, images can be represented as a graph where the pixels are the vertices and edges are drawn between neighboring pixels.

## Insert image grid / graph visual

In this post, I will go over the basics of graphs and the application of neural networks on them.

## table of contents

## Basic Graph Theory
For this application, a special type of graph is covered - the undirected simple graph.
Mathematically speaking an undirected simple graph is an ordered pair $$G = (V,E)$$ where:

- $$V$$ is a set of vertices (nodes)
- $$E \subseteq \{\{x,y\}\ where\ x,y \in V and\ x \ne y\} $$ is the set of edges between nodes

A common way to represent this graph is with adjacency and degree matrices. An adjecency matrix A, for an undirected simple graph is as follows:

- $$A_{i,j} = 1$$ if there is an edge between $$v_i$$ and $$v_j $$ 
- $$A_{i,j} = 0$$ otherwise

A degree matrix is as follows:

- $$D\ =\ diag(degree(v_1),degree(v_2),...,degree(v_n)) $$.  
- $$degree(v_i)\ =\ number\ of\ edges\ containing\ vertex\ v_i $$.

Together with the degree matrix and the adjacency matrix we can define the graph laplacian L which is a linear map from $$\mathbb{R}^n$$ to $$\mathbb{R}^n$$.

Informally, the Laplacian is defined as follows:

- $$L = D - A$$ where $$D$$ is the degree matrix and $$A$$ is the adjacency matrix

## Insert laplacian graphic

The Laplacian matrix is effectively a difference operator
 
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
/end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
