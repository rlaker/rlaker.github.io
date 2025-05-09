---
title: "Attempting to write formatter for GAMS"
layout: single
excerpt: "and having more respect for syntax highlighting"
tags: [til]
---

My current focus at work is developing the in house linear programming solver, mostly written in [GAMS](https://www.gams.com/products/gams/gams-language/). This "Generic Algebraic Modelling System" is a high level language for describing linear optimisation problems, that it can then compile into any low level solver, such as [CPLEX](https://www.ibm.com/products/ilog-cplex-optimization-studio/cplex-optimizer).

As an example, say we have 5 items, $i$, and a rucksack with capacity `100`, how do we decide which items to put into the bag? Each one has a different weight, $w_{i}$ and value, $v_{i}$ and we can set up the problem by introducing a new binary variable $x_i$ to represent if we pick that item or not.

We can then write the objective function to maximise as: 
$$Z = x_1*v_{1} + x_2*v_{2} + x_3*v_{3} + x_4*v_{4} + x_5*v_{5}$$

subject to the constraint: 
$$x_1*w_{1} + x_2*w_{2} + x_3*w_{3} + x_4*w_{4} + x_5*w_{5} \le 100$$

This representation obviously does not scale well with 1000s of items, so we can refactor it in GAMS like so:

```
Z =e= sum(i, x(i) * v(i) )
```

and 

```
sum(i, x(i) * w(i) ) =l= 100
```

That all seems fairly innocuous, but when you write an application to handle complex business logic it can turn into a sea of parentheses...

This issue gets worse when you discover that there are no formatting rules in GAMS! Imagine my horror when trying to understand what a thousand lines like this could be doing

```
equation_1(a)$(condition_1(i,j,t) and value_1(x) < value_2(x)).. sum((ab(a,b),bc(b,c),cd(c,d)), value(a,b,c,d)$(condition_2(a,b,c,d))) =l= 100
```

# Topiary

I couldn't find a formatter, like [ruff](https://docs.astral.sh/ruff/formatter/), that could understand this GAMS code, so I made the terrible decision of trying to write my own...

Things started off well after finding this very recent (Jan 2025) blog [post](https://www.tweag.io/blog/2025-01-30-topiary-tutorial-part-1) about a new "universal formatting engine" called [Topiary](https://topiary.tweag.io/). Apparently, all I needed to do was use the tree-sitter package to generate a grammar.js file and I would be off and away.

This great [intro](https://derek.stride.host/posts/comprehensive-introduction-to-tree-sitter) to syntax trees shows how we can define rules to turn expressions like $x*y+z$ into the syntax tree below

<img src="https://derek.stride.host/assets/images/graphs/tree-sitter-parsing-part-6.svg">

Because we could parse this tree as `(x + y) + z` or `x + ( y + z)` we need to explicitly tell the parser that we prefer the left option (in the `grammar.js` this is done with `prec.left()` wrapped around the rule)

In GAMS you can assign a set like `x = 1`, a subset of the set `x(i) = 1` or an attribute `x.lo(i) = 1`. We can set up these rules like so:

```javascript
parameter_reference: $ => seq(
	$.identifer,
	optional(seq(token("."), $.attribute)),
	optional($.indexing)
)

$.identifer: $ => /[a-zA-Z_][a-zA-Z0-9_]*/, //matches letters

$.attribute: $ => choice(
	"l",
	"lo",
	"up",
	"scale",
	"fx"
),

$.indexing: $ => seq(
	"(",
	$.identifier,
	optional(seq(",", $.identifier)),
	")"
)
```

which should be able to handle:
- `x` as `identifer`
- `x(i)` as `identifier(index)`
- `x.lo(i)`  as `identifier.attribute(index)`

However, I couldn't vibe code my way past the issue that tree-sitter reads from left to right, meaning it reads the `x` in `x.lo(i)` and instantly assigns it as its own parameter_reference before waiting to read the whole `x.lo(i)`!!!

I tried changing the precedence of rules (as explained above) but nothing worked. Weirdly if I have something like `x.lo(i) = y.lo(i)` it parses properly on the RHS but not the LHS!!

At this point I gave up, because GAMS has a lot of syntax rules and I couldn't even get past `x.lo(i)`...

...and then we got access to GitHub copilot at work. I asked copilot to format my files for me and hey presto, I got something like this

```
equation_1(a)$(
	condition_1(i,j,t) 
	and value_1(x) < value_2(x)
	)
	.. 
	sum(
		(
			ab(a,b),
			bc(b,c),
			cd(c,d)
		),
		value(a,b,c,d)$(condition_2(a,b,c,d))
	) =l= 100
```

So, who needs to meticulously build a parser these days when all the languages on the internet have been modelled in an LLM?

At least I now have more respect whenever I see syntax highlighting. 

> As an aside, these types of syntax trees are useful for understanding grammar in natural languages (as I read in [The Sense of Style](https://en.wikipedia.org/wiki/The_Sense_of_Style))
> 
> <img src="https://external-content.duckduckgo.com/iu/?u=http%3A%2F%2Fellinfobcps.weebly.com%2Fuploads%2F4%2F8%2F6%2F7%2F48674241%2F650692067.png&f=1&nofb=1&ipt=8bcc605927ef165c999ea0fd776183bde15d174b32214f0a5340ababa29c3fb5">
> 
