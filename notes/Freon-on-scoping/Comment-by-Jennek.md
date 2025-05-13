#### scope graph
what do the edges in the scope graph mean? I assume they mean "lexical scoping".
I would prefer is lexical scoping is not built-in, but rather an explicit parameter of the way the resolver uses these edges: do they signify visibility or not?
Surely lexical scoping makes a good default, many languages have it but it is conceivable to have a language that does not (for example a functional language that wants to prevent any side effect)
In other words: each scope is a black box (you cannot reference any declared name from outside) unless explicitly stated otherwise.
This suggests that we need only one type of visibility edge in the scope graph: A can see inside B (and may refer to named nodes in B as if they were in A). This one may be used upwards, mirroring the lexical containment, thus implementing lexical scoping. This one may also be used sideways, cutting through lexical containment, thus implementing inheritance. This one might even be used downward, but I do not know of an example from practice.
However it is my hunch that we need a second visibility edge: A may see inside B but has to prefix the name in the reference with the name of B. when used sideways, this would amount to importing a namespace as in say Java. when used upwards, this is a way to circumvent shadowing.
#### on visible nodes and declared nodes
declared nodes and visible nodes are very different phenomena.
- declared nodes is part of the notion of scope: what nodes are member of a scope. on other words, when overlaying the scope graph over the AST, what nodes does a particular scope node attract?
- visible nodes are part of the notion of the resolution calculus: what scopes are reachable from a given reference node (making the declared nodes of the reachable scopes visible). in other words, what do the edges of the scope graph mean?

#### on qualified names
the suggested way of constructing the qualified name is: going up and prefixing the name of any ancestor namespace. you may consider the FQN as a property of the declared node. Now how about using FQN in a reference node? How would a reference with this FQN be able to see the declared node? this is visibility from the top going down, and there is no such thing unless explicitly allowed. Apparently the resolution calculus must cater for this.

#### on the expression for a namespace addition or replacement
I find it hard to judge the value and completeness of the proposal. I suppose I first have to try to reconstruct various scoping patterns from real life languages.

#### other questions

do you actually construct the scope graph at runtime? (just curious)

in the example AST there is one non-tree part `A1-C3-D1` what is the meaning of it?



