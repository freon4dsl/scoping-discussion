# Terminology

Let's start with some commonly used terms in the field of scoping.

Name Binding
: "The association between a name and a AST node, such as a variable or function."

Scope

: "The region in the AST where a certain name binding is valid." 

This definition is how the term scope is being used in the parsing community.
For instance, the
name 'A' may be linked to the node with id '102' in the subtree of the AST formed
by model unit 'U1'. However, in the subtree of model unit 'U2' this name binding is
not valid.

There is also a more loose use of the term 'scope', for which actually 'in scope' is a better
phrase, which means more or less the set of nodes that are visible from a certain point in the AST.

Note that both notions of scope start 'their thinking' from the point of the reference.

# The Relationship between Scope and Namespace

Starting with the first definition of scope, we can derive a notion of **Namespace** as follows.

Scope is the visibility of a name (= validity of a name binding)
within a certain part (region) of the model. Any part of the model is always a set of AST nodes, therefore we
can defined scope as a function:

>scope(name): set of ASTnodes

Let's say that the scope of 'A' is 'setA'. Combining the scopes of 'A' and 'B' would yield four sets:
1. setAB, the set of nodes where both A and B are visible,
2. setA-notB, the set of nodes where A is visible but B is not, 
3. setB-notA, the set of nodes where B is visible but A is not,
4. set-notA-notB, the set of nodes where neither A nor B is visible.

When we now combine all the scopes of all possible names in the model, we derive a division of the complete 
AST into sets of nodes. In each part one or more name bindings are valid, i.e. one or more named nodes are 
visible. 

Let's call such a part: Namespace. By definition, it is associated with a set of visible 
nodes. And again, by definition, it is associated with a set of 'contained' nodes.

So the definition of **Namespace** is: 

Namespace
: "A container that holds a set of AST nodes, which is capable of determining whether a certain name-binding is valid."

(Still open: should all names be unique within the container, or should all names of nodes
of the same type be unique, or is it enough that all nodes are unique?)

Furthermore, we can define the term **inScope**.

inScope
: "A name is in scope in a certain AST node 'X', when this name is within the set of visible nodes of 
 the namespace that contains 'X'."

Some other terms can be defined as well.

Declared (or Contained) Nodes of a Namespace
: All the nodes that the namespace holds.

Visible Nodes of a Namespace
: All the nodes that visible in the namespace.

Name Resolution
: In the scope graph framework (Visser e.a), name resolution involves finding a path
from a name (a reference) to an AST node (a declaration).

# What is Scoping?

The whole topic of scoping can be brought down to the following question: Given a reference/name,
to which node can it be coupled?

Answer: it can be coupled to any of the visible nodes of the namespace in which the reference resides. 
In other words, the binding of name 'XYZ' to a node with id '123' is valid in namespace 'K', iff this binding is 
in the set of valid bindings. Or again, but different, iff the node '123' is in the set of visible nodes 'K'.

This brings us to the following requirements for a language workbench.
For any language:
1. it must be possible to divide the AST into namespaces, and
2. it must be possible to indicate how to determine the set of visible nodes in a namespace.

# How to Divide the AST into Namespaces?

The restriction that Freon puts on namespaces is that the set of AST nodes that it holds cannot be merely a set,
it needs to be a subtree of the AST. Which subtrees are namespaces is determine by the (meta)type
of the root of the subtree. 

Following the structure of the AST, the namespaces from a tree as well.

# How to Determine the Set of Visible Nodes?

(Anneke: this is the part that is most interesting to me!)

Starting point is the rule that every node contained in a namespace is also in the set of visible nodes.

Most common types of scoping all come down to some form of hierarchical block scoping. The block can be a function, 
a class, a module, a file, etc. In general, the block is a (sub)tree of AST nodes. (Only dynamic scoping 
cannot be brought into this context.) Therefore a second rule is that when namespaces form a hierarchy, the visible 
names of the parent namespace are include in the set of visible names of the child namespace.

## Imports
When namespace 'A' imports namespace 'B', the contained names from 'B' are included in the set of visible names of 'A'.
This could be extended to include all visible names of 'B'.

## Exports
Another common form of adapting scoping is 'exports'. Namespace 'X' can export name (node) 'Y', which means that 
node 'Y' is made visible in another namespace. The question is in which namespaces?

## Public/private Distinction
A namespace can prevent a name (node) being visible in another namespace by defining it to be 'private'. This is 
often extended with the notion 'protected', where the name is visible in certain namespaces but not in others.

## Using (the Type of) the Parent of the Reference
When using qualified names, the parent of the reference is used to determine the visible names. For instance,
`K.L.M` means that `L` is visible in `K`, and `M` is visible in `L`. 

In programming languages this is often extended in such a manner that not the parent node, but the type of the 
parent node is used to determine the visible names. For instance, `errors.length` means that the name `length` is
visible in the type of the object `errors`.

## More ways to determine the set of visible names

To be extended ...







