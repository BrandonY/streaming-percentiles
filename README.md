# streaming-percentiles

## Build Status

![ci](https://github.com/sengelha/streaming-percentiles/actions/workflows/ci.yml/badge.svg)
![nightly](https://github.com/sengelha/streaming-percentiles/actions/workflows/nightly.yml/badge.svg)

## About the Library

This is a cross-platform library with implementations of various
percentile algorithms on streams of data.  These algorithms allow
you to calculate approximate percentiles (e.g. 50th percentile,
95th percentile) in a single pass over a data set.
They are particularly useful for calculating percentiles for
immense data sets, for extremely high-throughput systems, or
for near-real time use cases.

The library supports the following languages:

- C++
- JavaScript

The library implements the following streaming percentile algorithms:

- [Greenwald-Khanna](doc/methodology/GK01.pdf)
- [Cormode-Korn-Muthukrishnan-Srivastava](doc/methodology/CKMS05.pdf) for high-biased, low-biased, uniform, and targeted quantiles
- [Q-Digest](doc/methodology/q-digest.pdf)

For more background on streaming
percentiles, see [Calculating Percentiles on Streaming
Data](//www.stevenengelhardt.com/postseries/calculating-percentiles-on-streaming-data/).

## Obtaining the Library

The current version of the library is 3.1.0, and it was released
March 28, 2019.

### C++

You can download the latest release of the library from the
[streaming-percentiles latest release
page](//github.com/sengelha/streaming-percentiles-cpp/releases/latest).

### JavaScript

If you use NPM, `npm install streaming-percentiles`. Otherwise, download
the latest release of the library from the [streaming-percentiles latest
release
page](//github.com/sengelha/streaming-percentiles-cpp/releases/latest)
page.

For convenience, you can also use the latest release JS directly
from `sengelha.github.io`:

- [Unminified version](//sengelha.github.io/streaming-percentiles/streamingPercentiles.v1.js)
- [Minified version](//sengelha.github.io/streaming-percentiles/streamingPercentiles.v1.min.js)

### Source Code

You can download the latest release's source code from the
[streaming-percentiles latest release
page](//github.com/sengelha/streaming-percentiles-cpp/releases/latest).

See [CONTRIBUTING.md](CONTRIBUTING.md) for instructions on how to build the release from
source.

### Historical Releases

Historical releases may be downloaded from the [streaming-percentiles release page](//github.com/sengelha/streaming-percentiles/releases).

## Using the Library

### C++

Here's an example on how to use the Greenwald-Khanna streaming
percentile algorithm from C++:

```cpp
#include <stmpct/gk.hpp>

double epsilon = 0.1;
stmpct::gk<double> g(epsilon);
for (int i = 0; i < 1000; ++i)
    g.insert(rand());
double p50 = g.quantile(0.5); // Approx. median
double p95 = g.quantile(0.95); // Approx. 95th percentile
```

### JavaScript

#### Node.JS

Here's an example of how to use the library from Node.JS:
```javascript
var sp = require('streaming-percentiles');

var epsilon = 0.1;
var g = new sp.GK(epsilon);
for (var i = 0; i < 1000; ++i)
    g.insert(Math.random());
var p50 = g.quantile(0.5); // Approx. median
var p95 = g.quantile(0.95); // Approx. 95th percentile
```

#### Browser

Here's an example of how to use the library from a browser.  Note that the
default module name is `streamingPercentiles`:
```html
<script src="//sengelha.github.io/streaming-percentiles/streamingPercentiles.v1.min.js"></script>
<script>
var epsilon = 0.1;
var g = new streamingPercentiles.GK(epsilon);
for (var i = 0; i < 1000; ++i)
    g.insert(Math.random());
var p50 = g.quantile(0.5);
</script>
```

## Performance

Below is the performance of the C++ implementation of these algorithms
tested on December 22, 2021 on a Intel(R) Core(TM) i7-8809G CPU @ 3.10GHz:

| Algorithm | Epsilon |         N | Input Type | Throughput (thousands of insertions/s) |
| --------- | ------: | --------: | ---------- | -------------------------------------: |
| ckms_hbq  |     0.1 | 1,000,000 | sorted     |                                1,517.7 |
| ckms_hbq  |    0.01 | 1,000,000 | sorted     |                                1,094.1 |
| ckms_hbq  |   0.001 | 1,000,000 | sorted     |                                  289.8 |
| ckms_uq   |     0.1 | 1,000,000 | sorted     |                                1,533.9 |
| ckms_uq   |    0.01 | 1,000,000 | sorted     |                                1,654.2 |
| ckms_uq   |   0.001 | 1,000,000 | sorted     |                                  399.2 |
| gk        |     0.1 | 1,000,000 | sorted     |                                1,392.1 |
| gk        |    0.01 | 1,000,000 | sorted     |                                1,444.9 |
| gk        |   0.001 | 1,000,000 | sorted     |                                1,114.9 |

See `//cpp:benchmark` for the benchmark program.

Benchmarks are auto-generated on every `master` build.  They can be viewed
at [https://sengelha.github.io/streaming-percentiles/dev/bench/](https://sengelha.github.io/streaming-percentiles/dev/bench/).

## API Reference

See [doc/api_reference/](doc/api_reference/index.md) for detailed API reference documentation.

## License

This project is licensed under the MIT License.  See
[LICENSE](LICENSE) for more information.

## Contributing

If you are interested in contributing to the library, please see
[CONTRIBUTING.md](CONTRIBUTING.md).
