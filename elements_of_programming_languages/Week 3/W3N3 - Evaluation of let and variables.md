One approach: whenever we see $let x=e_1 in e_2$,
1. evaluate $e_1$ to $v_1$
2. replace $x$ with $v_1$ in $e_2$ and evaluate that

**Definition:** $e\Downarrow v$ for $L_{Let}$
$$
\frac{e_1\Downarrow v_1 \;\;\;e_2[v_1/x]\Downarrow v_2}{\text{let }x=e_1\text{ in }e_2\Downarrow v_2}
$$
With this definition we always substitute a value for a variable, so we don't need a rule for "evaluating" a variable. This approach is called *eager* or *strict* evaluation.

# Interpreter changes
```scala
type Variable = String
...
case class Var(x: Variable) extends Expr
case class Let(x: Variable, e1: Expr, e2: Expr) extends Expr
...
def eval(e: Expr): Value = e match {
	...
	case Let(x, e1, e2) => {
		val v = eval(e1);
		val e2vx = subst(e2, v, x);
		eval(e2vx)
	}
}
```
We don't need a case for `Var(x)` as all variables must be bound at some point.
# Types and variables
TODO