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

Both modes deliver honest, full-range sin/cos with zero caveats:

| Mode | How | Accuracy | Relative speed (vs `-O2` libm) |
|------|-----|----------|-------------------------------|
| SSE2 default (`aric file.ari`) | quick-path poly for \|x\|≤π/4, octant-reduced FMA3 Horner polynomial + Cody-Waite for larger inputs | **~1e-15 relative** across whole real line | **aric 1.21× faster than libm** on pure sin/cos loops; ~1.60× slower on mixed workloads |
| x87 (`aric --precision=15 file.ari`) | FSIN/FCOS with hardware FPREM1 | **IEEE-754 80-bit full precision** (~15 digits) | ~9× slower |

Measured examples (SSE2 default, 15-digit print):

| Call | Our result | Reference (mpmath) | Error |
|------|-----------|-------------------|-------|
| `sin(10)` | -0.544021110889369 | -0.544021110889370 | ~1e-15 |
| `sin(1e6)` | -0.349993502171292 | -0.349993502171286 | ~6e-15 |
| `sin(π)` | 0.000000000000000 | 0 | <1e-16 |
| `sin²(12.345) + cos²(12.345)` | 0.999999999999999 | 1 | ~1e-15 |

Requires CPU with SSE4.1 + FMA3 (every mainstream x86 since ~2013; Zen 3
is the reference target).

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
