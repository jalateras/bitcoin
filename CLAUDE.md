# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build System

Bitcoin Core uses **CMake** as the primary build system. Basic build commands:

```bash
# Configure and build
cmake -B build
cmake --build build -j$(nproc)

# Development build with all features enabled  
cmake -B build --preset=dev-mode
cmake --build build

# Install (optional)
cmake --install build
```

Key build options:
- `-DBUILD_GUI=ON/OFF` - Qt GUI (default: auto-detect)
- `-DBUILD_TESTS=ON/OFF` - Unit tests (default: ON)
- `-DENABLE_WALLET=ON/OFF` - Wallet functionality (default: ON)  
- `-DWITH_ZMQ=ON/OFF` - ZeroMQ support (default: auto-detect)
- `-DBUILD_FUZZ_BINARY=ON/OFF` - Fuzzing binary (default: OFF)

## Testing

### Unit Tests
```bash
# Run all unit tests
ctest --test-dir build

# Run specific test suite  
build/bin/test_bitcoin --run_test=<test_name>

# Run with full logging
build/bin/test_bitcoin --log_level=all --run_test=<test_name>
```

### Functional Tests  
```bash
# Run all functional tests
build/test/functional/test_runner.py

# Run specific test
build/test/functional/test_runner.py wallet_basic.py

# Run with multiple jobs
build/test/functional/test_runner.py --jobs=4
```

### GUI Tests
```bash
# Run GUI unit tests
build/bin/test_bitcoin-qt
```

## Code Quality

### Linting
Bitcoin Core has comprehensive linting via Docker:
```bash
# Build and run all lints
DOCKER_BUILDKIT=1 docker build -t bitcoin-linter --file "./ci/lint_imagefile" ./
docker run --rm -v $(pwd):/bitcoin -it bitcoin-linter
```

Individual lint scripts are in `test/lint/` directory.

### Code Formatting
Uses clang-format with configuration in `src/.clang-format`. Format code with:
```bash
# Format specific file
clang-format -i src/path/to/file.cpp

# Use the provided script for patches
contrib/devtools/clang-format-diff.py
```

## Architecture Overview

### Core Components
- **`src/bitcoind.cpp`** - Main daemon entry point
- **`src/bitcoin-cli.cpp`** - Command-line interface  
- **`src/bitcoin-qt/`** - Qt GUI application
- **`src/validation.cpp`** - Block/transaction validation core
- **`src/net*.cpp`** - P2P networking layer
- **`src/txmempool.cpp`** - Transaction memory pool

### Key Directories
- **`src/consensus/`** - Consensus rules and validation logic
- **`src/wallet/`** - Wallet functionality (optional component)
- **`src/rpc/`** - JSON-RPC interface implementation  
- **`src/script/`** - Script engine and descriptors
- **`src/crypto/`** - Cryptographic primitives
- **`src/kernel/`** - Bitcoin kernel library (consensus engine)
- **`src/node/`** - Node-specific functionality
- **`src/util/`** - Shared utility functions

### Subtrees (External Dependencies)
- **`src/secp256k1/`** - Elliptic curve cryptography
- **`src/leveldb/`** - Database backend  
- **`src/minisketch/`** - Set reconciliation
- **`src/univalue/`** - JSON library

## Development Workflow

### Code Standards
- **Language**: C++20 standard
- **Style**: Enforced by clang-format (see `src/.clang-format`)
- **Naming**: snake_case for variables, PascalCase for classes/functions
- **Testing**: Unit tests required for new functionality

### Repository Structure
- **Main development** happens on the `master` branch
- **GUI-only changes** should target https://github.com/bitcoin-core/gui
- **All other changes** target this repository

### Pull Request Process
1. Fork repository and create topic branch
2. Ensure all tests pass: `ctest --test-dir build && build/test/functional/test_runner.py`
3. Run linting to ensure code quality
4. Submit PR with clear description and test plan

### Communication  
- **IRC**: `#bitcoin-core-dev` on Libera Chat
- **Issues/PRs**: GitHub (this repository)
- **Complex changes**: Bitcoin development mailing list

## Common Development Tasks

### Adding Unit Tests
Add `BOOST_AUTO_TEST_CASE` functions to existing files in `src/test/` or create new test suites.

### Debugging Builds
```bash
# Debug build with symbols
cmake -B build -DCMAKE_BUILD_TYPE=Debug

# Release with debug info (default)
cmake -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo
```

### Memory-Constrained Builds
```bash
# Reduce memory usage during compilation
cmake -B build -DCMAKE_CXX_FLAGS="--param ggc-min-expand=1 --param ggc-min-heapsize=32768"
```

### Working with Subtrees
The codebase includes several subtree dependencies. Avoid modifying subtree code directly - changes should be made upstream and then subtree-merged.

## Critical Notes

- This is a **security-critical project** - any bugs can result in financial loss
- **Extensive testing and review** is required for all changes
- **Consensus code changes** require special care and broad review
- **Performance matters** - Bitcoin Core must handle high transaction volumes
- **Deterministic builds** are essential for security verification

## OP_RETURN Documentation
- OP_RETURN is a Bitcoin script opcode that allows embedding small amounts of data into transactions
- Typically used for metadata, proof of existence, or simple data storage
- Limited to 80 bytes of data to prevent blockchain bloat
- Does not create a spendable output, making it a "provably unspendable" output
- Useful for timestamping, token issuance, and other non-financial use cases
- Transactions with OP_RETURN are valid but do not create dust outputs