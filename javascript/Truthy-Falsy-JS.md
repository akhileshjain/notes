## Truthhy and Falsy Values in JavaScript

In JavaScript there are **six** falsy values:
- false
- 0 (zero)
- `''` or `""` (empty string)
- `null`
- `undefined`
- `NaN` (Not a Number)

*Everything else in JavaScript is Truthy. But, just because a value is truthy, doesn’t mean that it will coerce to true when testing equality.*

**The reason a truthy value can return true in a boolean context, but not coerce to equal true has to do with the implementation of JavaScript. JavaScript simply uses different operations in a boolean context versus in a coercion context.**

For more reference, read this [Link.](https://codeburst.io/javascript-truthy-values-dont-always-equal-true-8afaf071a4a6)