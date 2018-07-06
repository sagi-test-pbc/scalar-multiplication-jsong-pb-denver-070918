
# Scalar multiplication for Elliptic Curves

Because we can add a point to itself, we can introduce some new notation:

`(170,142) + (170,142) = 2⋅(170,142)`

Similarly, because we have associativity, we can actually add the point again:

`2⋅(170,142) + (170,142) = 3⋅(170, 142)`

We can actually do this as many times as we want. This is what we call Scalar Multiplication. That is, we have a _scalar_ number in front of the point. We can do this because we have defined point addition.

What's interesting about scalar multiplication is that it's really hard to predict without actually calculating.

This is because point addition is non-linear. That is, not easy to calculate. In fact, doing the scalar multiplication is more or less straightforward, but doing the opposite, scalar division, is not.

This is called the Discrete Log problem and is the basis of Elliptic Curve Cryptography.

The interesting thing about Scalar Multiplication is that at a certain number, we get to the point at infinity (remember, point at infinity is the additive identity or 0). If we imagine a point G and scalar multiply until we get the point at infinity, we end up with a set like this:

{ G, 2G, 3G, 4G, ... nG }

It turns out that this set is called a Group and because n is finite, we have a Finite Group. Groups are interesting mathematically because they behave a lot like addition:

G+4G=5G or aG+bG=(a+b)G

When we combine the fact that scalar multiplication is easy to go in one direction but hard in the other and the mathematical properties of a Group, we have exactly what we need for Elliptic Curve Cryptography.

#### Why is this called the Discrete Log Problem?

You may be wondering why the problem of scalar *multiplication* is referred to as the discrete *log* problem.

We called the operation between the points "addition", but we could easily have called it a point "operation". Typically, a new operation that you define in math utilizes the dot operator (⋅). The dot operator is also used for multiplication, and it sometimes helps to think that way:

$P~1~⋅P~2~=P~3~$

When you do lots of multiplying, that's the same as exponentiation. Scalar multiplication when we called it "point addition" becomes scalar exponentiation:

`P^7 = Q`

The discrete log problem is really the ability to reverse this:

$log~P~Q=7$

The log equation on the left is not analytically calculatable. That is, there is no known formula that you can plug in to get the answer generally. This is all a bit confusing, but it's fair to say that we could call the problem the "Discrete Point Division" problem instead of Discrete Log.

### Try it

#### Find the following scalar multiplications on the curve  \\( y^2 = x^3 + 7: F_{223} \\)
```
2*(192,105), 2*(143,98), 2*(47,71), 4*(47,71), 8*(47,71), 21*(47,71)
```

#### Hint: add the point to itself n times


```python
from ecc import FieldElement, Point

prime = 223
a = FieldElement(0, prime)
b = FieldElement(7, prime)

multiplications = ((2, 192, 105), (2, 143, 98), (2, 47, 71), (4, 47, 71), (8, 47, 71), (21, 47, 71))

# iterate over the multiplications
for n, x_raw, y_raw in multiplications:
    # Initialize points this way:
    # x = FieldElement(x_raw, prime)
    # y = FieldElement(y_raw, prime)
    # p = Point(x, y, a, b)
    # start product at 0 (point at infinity)
    # loop over n times (n is 2, 4, 8 or 21 in the above examples)
        # add the point to the product
    # print product
```

### Scalar Multiplication Redux

The key insight to set up Public Key Cryptography is the fact that scalar multiplication on Elliptic Curves is very hard to reverse. 

If we look closely at the numbers, there's no real discernible pattern to the scalar multiplication. The x-coordinates don't always increase or decrease and neither do the y-coordinates. About the only pattern in this set is that between 10 and 11, the x coordinates seem to be aligned (10 and 11 have the same x, as do 9 and 12, 8 and 13 and so on).

Scalar Multiplication looks really random and that's what we're going to use for what we call an *assymetric* problem. An *assymetric* problem is one that's easy to calculate in one direction, but hard to reverse. For example, it's easy enough to calculate 12⋅(47,71). But if we were presented this:

`s⋅(47,71)=(194,172)`

Would you be able to solve for `s`?
