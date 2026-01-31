<!--

@license Apache-2.0

Copyright (c) 2025 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# nanmeanpn

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Compute the [arithmetic mean][arithmetic-mean] along one or more [ndarray][@stdlib/ndarray/ctor] dimensions, ignoring `NaN` values and using a two-pass error correction algorithm.

<section class="intro">

The [arithmetic mean][arithmetic-mean] is defined as

<!-- <equation class="equation" label="eq:arithmetic_mean" align="center" raw="\mu = \frac{1}{n} \sum_{i=0}^{n-1} x_i" alt="Equation for the arithmetic mean."> -->

```math
\mu = \frac{1}{n} \sum_{i=0}^{n-1} x_i
```

<!-- <div class="equation" align="center" data-raw-text="\mu = \frac{1}{n} \sum_{i=0}^{n-1} x_i" data-equation="eq:arithmetic_mean">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@42d8f64d805113ab899c79c7c39d6c6bac7fe25c/lib/node_modules/@stdlib/stats/strided/nanmeanpn/docs/img/equation_arithmetic_mean.svg" alt="Equation for the arithmetic mean.">
    <br>
</div> -->

<!-- </equation> -->

</section>

<!-- /.intro -->



<section class="usage">

## Usage

```javascript
import nanmeanpn from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-nanmeanpn@v0.1.0-deno/mod.js';
```

#### nanmeanpn( x\[, options] )

Computes the [arithmetic mean][arithmetic-mean] along one or more [ndarray][@stdlib/ndarray/ctor] dimensions, ignoring `NaN` values and using a two-pass error correction algorithm.

```javascript
import array from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-array@deno/mod.js';

var x = array( [ 1.0, NaN, -2.0, 4.0 ] );

var y = nanmeanpn( x );
// returns <ndarray>[ 1.0 ]
```

The function has the following parameters:

-   **x**: input [ndarray][@stdlib/ndarray/ctor]. Must have a real-valued or "generic" [data type][@stdlib/ndarray/dtypes].
-   **options**: function options (_optional_).

The function accepts the following options:

-   **dims**: list of dimensions over which to perform a reduction. If not provided, the function performs a reduction over all elements in a provided input [ndarray][@stdlib/ndarray/ctor].
-   **dtype**: output ndarray [data type][@stdlib/ndarray/dtypes]. Must be a real-valued floating-point or "generic" [data type][@stdlib/ndarray/dtypes].
-   **keepdims**: boolean indicating whether the reduced dimensions should be included in the returned [ndarray][@stdlib/ndarray/ctor] as singleton dimensions. Default: `false`.

By default, the function performs a reduction over all elements in a provided input [ndarray][@stdlib/ndarray/ctor]. To perform a reduction over specific dimensions, provide a `dims` option.

```javascript
import array from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-array@deno/mod.js';

var x = array( [ 1.0, NaN, -2.0, 4.0 ], {
    'shape': [ 2, 2 ],
    'order': 'row-major'
});
// returns <ndarray>[ [ 1.0, NaN ], [ -2.0, 4.0 ] ]

var y = nanmeanpn( x, {
    'dims': [ 0 ]
});
// returns <ndarray>[ -0.5, 4.0 ]

y = nanmeanpn( x, {
    'dims': [ 1 ]
});
// returns <ndarray>[ 1.0, 1.0 ]

y = nanmeanpn( x, {
    'dims': [ 0, 1 ]
});
// returns <ndarray>[ 1.0 ]
```

By default, the function excludes reduced dimensions from the output [ndarray][@stdlib/ndarray/ctor]. To include the reduced dimensions as singleton dimensions, set the `keepdims` option to `true`.

```javascript
import array from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-array@deno/mod.js';

var x = array( [ 1.0, NaN, -2.0, 4.0 ], {
    'shape': [ 2, 2 ],
    'order': 'row-major'
});
// returns <ndarray>[ [ 1.0, NaN ], [ -2.0, 4.0 ] ]

var y = nanmeanpn( x, {
    'dims': [ 0 ],
    'keepdims': true
});
// returns <ndarray>[ [ -0.5, 4.0 ] ]

y = nanmeanpn( x, {
    'dims': [ 1 ],
    'keepdims': true
});
// returns <ndarray>[ [ 1.0 ], [ 1.0 ] ]

y = nanmeanpn( x, {
    'dims': [ 0, 1 ],
    'keepdims': true
});
// returns <ndarray>[ [ 1.0 ] ]
```

By default, the function returns an [ndarray][@stdlib/ndarray/ctor] having a [data type][@stdlib/ndarray/dtypes] determined by the function's output data type [policy][@stdlib/ndarray/output-dtype-policies]. To override the default behavior, set the `dtype` option.

```javascript
import getDType from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-dtype@deno/mod.js';
import array from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-array@deno/mod.js';

var x = array( [ 1.0, NaN, -2.0, 4.0 ], {
    'dtype': 'generic'
});

var y = nanmeanpn( x, {
    'dtype': 'float64'
});
// returns <ndarray>

var dt = String( getDType( y ) );
// returns 'float64'
```

#### nanmeanpn.assign( x, out\[, options] )

Computes the [arithmetic mean][arithmetic-mean] along one or more [ndarray][@stdlib/ndarray/ctor] dimensions, ignoring `NaN` values and using a two-pass error correction algorithm, and assigns results to a provided output [ndarray][@stdlib/ndarray/ctor].

```javascript
import array from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-array@deno/mod.js';
import zeros from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-zeros@deno/mod.js';

var x = array( [ 1.0, NaN, -2.0, 4.0 ] );
var y = zeros( [] );

var out = nanmeanpn.assign( x, y );
// returns <ndarray>

var v = out.get();
// returns 1.0

var bool = ( out === y );
// returns true
```

The method has the following parameters:

-   **x**: input [ndarray][@stdlib/ndarray/ctor]. Must have a real-valued or "generic" [data type][@stdlib/ndarray/dtypes].
-   **out**: output [ndarray][@stdlib/ndarray/ctor].
-   **options**: function options (_optional_).

The method accepts the following options:

-   **dims**: list of dimensions over which to perform a reduction. If not provided, the function performs a reduction over all elements in a provided input [ndarray][@stdlib/ndarray/ctor].

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   Setting the `keepdims` option to `true` can be useful when wanting to ensure that the output [ndarray][@stdlib/ndarray/ctor] is [broadcast-compatible][@stdlib/ndarray/base/broadcast-shapes] with ndarrays having the same shape as the input [ndarray][@stdlib/ndarray/ctor].
-   The output data type [policy][@stdlib/ndarray/output-dtype-policies] only applies to the main function and specifies that, by default, the function must return an [ndarray][@stdlib/ndarray/ctor] having a real-valued floating-point or "generic" [data type][@stdlib/ndarray/dtypes]. For the `assign` method, the output [ndarray][@stdlib/ndarray/ctor] is allowed to have any supported output [data type][@stdlib/ndarray/dtypes].

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
import uniform from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-uniform@deno/mod.js';
import filledarrayBy from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-filled-by@deno/mod.js';
import bernoulli from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-bernoulli@deno/mod.js';
import getDType from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-dtype@deno/mod.js';
import ndarray2array from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-to-array@deno/mod.js';
import ndarray from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-ctor@deno/mod.js';
import nanmeanpn from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-nanmeanpn@v0.1.0-deno/mod.js';

function rand() {
    if ( bernoulli( 0.8 ) < 1 ) {
        return NaN;
    }
    return uniform( 0.0, 20.0 );
}

// Generate an array of random numbers:
var xbuf = filledarrayBy( 25, 'generic', rand );

// Wrap in an ndarray:
var x = new ndarray( 'generic', xbuf, [ 5, 5 ], [ 5, 1 ], 0, 'row-major' );
console.log( ndarray2array( x ) );

// Perform a reduction:
var y = nanmeanpn( x, {
    'dims': [ 0 ]
});

// Resolve the output array data type:
var dt = getDType( y );
console.log( dt );

// Print the results:
console.log( ndarray2array( y ) );
```

</section>

<!-- /.examples -->

* * *

<section class="references">

## References

-   Neely, Peter M. 1966. "Comparison of Several Algorithms for Computation of Means, Standard Deviations and Correlation Coefficients." _Communications of the ACM_ 9 (7). Association for Computing Machinery: 496–99. doi:[10.1145/365719.365958][@neely:1966a].
-   Schubert, Erich, and Michael Gertz. 2018. "Numerically Stable Parallel Computation of (Co-)Variance." In _Proceedings of the 30th International Conference on Scientific and Statistical Database Management_. New York, NY, USA: Association for Computing Machinery. doi:[10.1145/3221269.3223036][@schubert:2018a].

</section>

<!-- /.references -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2026. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/stats-nanmeanpn.svg
[npm-url]: https://npmjs.org/package/@stdlib/stats-nanmeanpn

[test-image]: https://github.com/stdlib-js/stats-nanmeanpn/actions/workflows/test.yml/badge.svg?branch=v0.1.0
[test-url]: https://github.com/stdlib-js/stats-nanmeanpn/actions/workflows/test.yml?query=branch:v0.1.0

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/stats-nanmeanpn/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/stats-nanmeanpn?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/stats-nanmeanpn.svg
[dependencies-url]: https://david-dm.org/stdlib-js/stats-nanmeanpn/main

-->

[chat-image]: https://img.shields.io/badge/zulip-join_chat-brightgreen.svg
[chat-url]: https://stdlib.zulipchat.com

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/stats-nanmeanpn/tree/deno
[deno-readme]: https://github.com/stdlib-js/stats-nanmeanpn/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/stats-nanmeanpn/tree/umd
[umd-readme]: https://github.com/stdlib-js/stats-nanmeanpn/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/stats-nanmeanpn/tree/esm
[esm-readme]: https://github.com/stdlib-js/stats-nanmeanpn/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/stats-nanmeanpn/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/stats-nanmeanpn/main/LICENSE

[@stdlib/ndarray/ctor]: https://github.com/stdlib-js/ndarray-ctor/tree/deno

[@stdlib/ndarray/dtypes]: https://github.com/stdlib-js/ndarray-dtypes/tree/deno

[@stdlib/ndarray/output-dtype-policies]: https://github.com/stdlib-js/ndarray-output-dtype-policies/tree/deno

[@stdlib/ndarray/base/broadcast-shapes]: https://github.com/stdlib-js/ndarray-base-broadcast-shapes/tree/deno

[arithmetic-mean]: https://en.wikipedia.org/wiki/Arithmetic_mean

[@neely:1966a]: https://doi.org/10.1145/365719.365958

[@schubert:2018a]: https://doi.org/10.1145/3221269.3223036

</section>

<!-- /.links -->
