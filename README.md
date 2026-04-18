# aricode-math

Math stdlib for the [aricode](https://github.com/Lynx-Boss/aricode)
compiler: trigonometry extras, core utilities, activation functions
for ML, and high-precision float printing.

## Layout

```
aricode-math/
├── trig.ari         — tan, pow, pi, e, deg/rad conversions
├── activation.ari   — sigmoid, relu, leaky_relu, tanh, + derivatives
├── core.ari         — min/max/clamp/sign/floor/ceil/fmod (f64 and i32)
├── print.ari        — high-precision f64 printing via print_char
├── examples/
│   └── demo.ari
└── tests/
    └── test_math.ari
```

## Relation to aricode builtins

The aricode compiler already ships as **builtins**: `math_sin`, `math_cos`,
`math_exp`, `math_log`, `math_sqrt`, `math_abs`. Call them directly — no
import needed. This module adds what the builtin set is missing.

### Precision modes

- **SSE2 default** (`aric file.ari`) — 5-6 digit accuracy, full range
  via mod-2π pre-reduction. Fast path, typical for ML / graphics.
- **x87 full precision** (`aric --precision=15 file.ari`) — 15+ digits
  via FSIN/FCOS with hardware FPREM1 reduction. Use for finance,
  scientific computation, or anywhere ULPs matter.

## Public API

### trig.ari

| Function | Description |
|----------|-------------|
| `math_tan(x)` | Tangent (sin/cos) — shares the sin/cos range limitation |
| `math_pow(base, n)` | Integer exponentiation by squaring, `O(log n)` |
| `math_pi()` / `math_e()` | Constants |
| `deg_to_rad(d)` / `rad_to_deg(r)` | Degree ↔ radian conversion |

### activation.ari

| Function | Description |
|----------|-------------|
| `sigmoid(x)` / `sigmoid_deriv(x)` | Logistic σ and σ' |
| `relu(x)` / `relu_deriv(x)` | Rectified Linear Unit and derivative |
| `leaky_relu(x)` | α = 0.01 |
| `tanh_f(x)` | Hyperbolic tangent |

### core.ari

| Function | Description |
|----------|-------------|
| `math_min/max/clamp(a, b[, c])` | f64 |
| `math_sign(x)` | Returns -1.0, 0.0, or 1.0 |
| `math_floor_i(x)` / `math_ceil_i(x)` | f64 → i32 |
| `math_fmod(x, y)` | Floating-point modulo |
| `min_i/max_i/clamp_i(...)` | i32 versions |
| `abs_i(x)` | i32 absolute value |

### print.ari

| Function | Description |
|----------|-------------|
| `print_f64(x, digits)` | Print f64 with N decimal digits, no trailing newline |
| `print_int_raw(n)` | Print i32, no trailing newline |

## Usage

```
import "aricode-math/trig.ari" as trig;
import "aricode-math/activation.ari" as act;

fn main() -> i32 {
    print_float(math_sqrt(2.0));          // builtin, no import
    print_float(trig.math_pow(2.0, 10));  // 1024
    print_float(act.sigmoid(0.0));        // 0.5
    return 0;
}
```

See [`examples/demo.ari`](examples/demo.ari).

## Running the tests

```
aric tests/test_math.ari -o /tmp/test_math
/tmp/test_math
```

Expected output ends with `ALL TESTS PASSED / Failures: 0`.

## License

Copyright (c) 2026 Edwin F. Veliz Jaramillo. All rights reserved.
