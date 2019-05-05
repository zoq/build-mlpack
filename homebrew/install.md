# mlpack Homebrew

## Quicklinks
  * [Ready to Install package](#ready-to-install-package)
    * [stable build](#stable-build)
    * [nightly build](#nightly-build)
  * [Compiling manually](#compiling-manually)
    * [stable build](#stable-build)
    * [nightly build](#nightly-build)
  * [Example application](#example)
    * [build the application](#build)
    * [run the application](#run)

### Ready to Install package

We provide both stable and nightly packages.

#### stable build
```shell
brew tap zoq/mlpack
brew install mlpack
```

#### nightly build
```shell
brew tap zoq/mlpack
brew install mlpack-nightly
```

### Compiling manually

You can either build the latest stable version or the git master branch.

#### stable build
```shell
brew install armadillo boost cmake

curl https://www.mlpack.org/files/mlpack-3.1.0.tar.gz | tar xvz
cd mlpack-3.1.0
mkdir build && cd build
cmake ..
make install
```

#### nightly build
```shell
brew install armadillo boost cmake
git clone https://github.com/mlpack/mlpack.git
cd mlpack
mkdir build && cd build
cmake ..
make install
```

### Example

This simple program uses the mlpack::neighbor::NeighborSearch object to find the
nearest neighbor of each point in a dataset using the L1 metric, and then print
the index of the neighbor and the distance of it to stdout.

```cpp
#include <mlpack/core.hpp>
#include <mlpack/methods/neighbor_search/neighbor_search.hpp>
using namespace mlpack;
using namespace mlpack::neighbor; // NeighborSearch and NearestNeighborSort
using namespace mlpack::metric; // ManhattanDistance
int main()
{
  // Load the data from data.csv (hard-coded).  Use CLI for simple command-line
  // parameter handling.
  arma::mat data = arma::randn(10, 100);
  // Use templates to specify that we want a NeighborSearch object which uses
  // the Manhattan distance.
  NeighborSearch<NearestNeighborSort, ManhattanDistance> nn(data);
  // Create the object we will store the nearest neighbors in.
  arma::Mat<size_t> neighbors;
  arma::mat distances; // We need to store the distance too.
  // Compute the neighbors.
  nn.Search(1, neighbors, distances);
  // Write each neighbor and distance using Log.
  for (size_t i = 0; i < neighbors.n_elem; ++i)
  {
    std::cout << "Nearest neighbor of point " << i << " is point "
        << neighbors[i] << " and the distance is " << distances[i] << ".\n";
  }
}
```

#### build
Save the code above as ``test.cpp`` and build the code either with:

```
clang++ test.cpp -o test -std=c++11 `pkg-config --cflags --libs mlpack`
```

or with:

```
clang++ test.cpp -o test -std=c++11 -lmlpack -larmadillo -lboost_serialization -lboost_program_options -lboost_system
```

#### run

Once build you can run the code with:

```shell
./test
```
