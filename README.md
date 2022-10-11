# VectorsCheatSheet
Vectors Cheatsheet with [Babylon.js](https://www.babylonjs.com/).

**Language: Javascript**

**Start: 2022**

## Why
I wanted to collect useful formulas related to vectors. For a few formulas, I added examples with [Babylon.js](https://www.babylonjs.com/) 3D library.

## Vectors

X, Y, Z axes are represented with R, G, B colors:

![XYZ-RGB](/images/xyzrgb.jpg)

### Vector notations

A vector _v_ going from point _A_ to point _B_:

$$ \overrightarrow{AB} = (x_B - x_A, y_B - y_A, z_B - z_A) $$

where the parentheses are the [ordered set notation](https://en.wikipedia.org/wiki/Vector_notation#Ordered_set_notation) to represent the _x_, _y_, _z_ components of the vector.

```javascript
const pO = new BABYLON.Vector3(0, 0, 0);
const pA = new BABYLON.Vector3(3, 5, 0);
const pB = new BABYLON.Vector3(7, 8, 9);
const v = pB.subtract(pA);
drawArrow(scene, pA, pB, new BABYLON.Color3(1, 1, 1), false);
drawArrow(scene, pO, v, new BABYLON.Color3(0, 1, 1), false);
```

![two vector notations](/images/vector_notation.jpg)

### Vector length (aka magnitude)

$$ ||v|| = \sqrt{x^2 + y^2 + z^2} $$ 

```javascript
const length = BABYLON.Vector3.Distance(pA, pB);
```

### Unit vector (normalizing the vector)

$$ \frac{v}{||v||} $$ 

```javascript
const vUnit = v.normalize();
```
### Scalar multiplication

$$ av = (ax, ay, az) $$ 

```javascript
const vScale = v.scale(3);
drawArrow(scene, pO, vScale, new BABYLON.Color3(0.86, 0.3, 0.91), false); // purple
drawArrow(scene, pO, v, new BABYLON.Color3(0, 1, 1), false); // light blue
```

![scalar multiplication](/images/scalar_mult.jpg)

With a scale factor of -1:

![scalar multiplication](/images/scalar_mult_min1.jpg)

### Dot product (aka scalar product, inner product, projection product)

$$ v \cdot w = ||v|| ||w|| \cos{\theta} = \sum_1^n{v_i w_i}  $$

where &theta; is the angle between the two vectors _v_ and _w_ and _n_ is the number of components of the vectors (3 in the case of a 3D vector: x, y, z). Therefore, the result is 0 when the vectors are normal to each other (_cos(90°) = 0_).

```javascript
const scalar = BABYLON.Vector3.Dot(v, w);
```

### Angle between two vectors

$$ \theta = \arccos{\frac{v \cdot w}{||v|| ||w||}} $$

which can be simply derived from the dot product definition.

### Cross product (aka vector product)

$$ a \times b = ||a|| ||b|| \sin(\theta) n $$

where \theta is the angle between _a_ and _b_ in the plane containing them (0°..180°) and _n_ is the unit vector perpendicular to such plane (right-hand rule):

![right-hand rule](/images/right_hand.jpg)

Therefore, the cross product is used to find the vector orthogonal to two given vectors.

For 3D vectors, it can also be calculated as:

$$ a \times b = (a_y b_z - a_z b_y, a_z b_x - a_x b_z, a_x b_y - a_y b_x) $$

```javascript
const pA = new BABYLON.Vector3(5, 5, 0);
const pB = new BABYLON.Vector3(10, 5, 0);
const pC = new BABYLON.Vector3(5, 10, 0);
scene.useRightHandedSystem = true; // activate the right-hand system on the 3D scene
drawArrow(scene, pA, pB, new BABYLON.Color3(1, 1, 0), false);
drawArrow(scene, pA, pC, new BABYLON.Color3(1, 1, 1), false);

const v = pB.subtract(pA);
const w = pC.subtract(pA);
var ortho = BABYLON.Vector3.Cross(v, w);
ortho = ortho.normalize();

const pD = pA.add(ortho.scale(10)); // add the orthogal vector (with length 10) to pA to obtain the second point
drawArrow(scene, pA, pD, new BABYLON.Color3(1, 0, 1), false);
```

![cross product](/images/cross_product.jpg)

The cross product can be used to identify which side the vector _b_ is in relation to the vector _a_ (Wikipedia image):  

![cross product wikipedia](/images/cross_product_wiki.gif)

### Addition

$$ v + w = (x_v + x_w, y_v + y_w, z_v + z_w) $$

```javascript
const pA = new BABYLON.Vector3(2, 2, 5);
const pB = new BABYLON.Vector3(7, 8, 13);
const pC = new BABYLON.Vector3(5, 10, 0);
drawArrow(scene, pA, pB, new BABYLON.Color3(1, 1, 0), false); // yellow
drawArrow(scene, pA, pC, new BABYLON.Color3(1, 0, 1), false); // purple

const v = pB.subtract(pA);
const w = pC.subtract(pA);
const t = v.add(w);
drawArrow(scene, pA, t, new BABYLON.Color3(0, 1, 1), false); // light blue
drawArrow(scene, pB, t, new BABYLON.Color3(1, 1, 1), false); // white
drawArrow(scene, pC, t, new BABYLON.Color3(1, 1, 1), false); // white
```

![addition](/images/addition.jpg)