# aricode-stdlib-math

Math functions for Aricode -- trigonometry, exponentiation, and activation functions for ML.

## Functions

### trig.ari

| Function | Signature | Description |
|----------|-----------|-------------|
| `math_sin` | `fn math_sin(x: f64) -> f64` | Sine (Taylor series, radians) |
| `math_cos` | `fn math_cos(x: f64) -> f64` | Cosine (Taylor series, radians) |
| `math_tan` | `fn math_tan(x: f64) -> f64` | Tangent (sin/cos) |
| `math_pow` | `fn math_pow(base: f64, n: i32) -> f64` | Integer exponentiation by squaring |

### activation.ari

| Function | Signature | Description |
|----------|-----------|-------------|
| `sigmoid` | `fn sigmoid(x: f64) -> f64` | Logistic sigmoid |
| `sigmoid_deriv` | `fn sigmoid_deriv(x: f64) -> f64` | Sigmoid derivative |
| `relu` | `fn relu(x: f64) -> f64` | Rectified Linear Unit |
| `relu_deriv` | `fn relu_deriv(x: f64) -> f64` | ReLU derivative |
| `leaky_relu` | `fn leaky_relu(x: f64) -> f64` | Leaky ReLU (alpha = 0.01) |
| `tanh_approx` | `fn tanh_approx(x: f64) -> f64` | Hyperbolic tangent approximation |

## Usage

```aricode
import "math/trig.ari";
import "math/activation.ari";

fn main() -> i32 {
    let angle: f64 = 1.5708;
    let s: f64 = math_sin(angle);
    let a: f64 = relu(s);
    print_f64(a);
    return 0;
}
```

## License

Copyright (c) 2026 Edwin F. Veliz Jaramillo. All rights reserved.
