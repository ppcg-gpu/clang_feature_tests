# Clang Feature Tests

This module provides CMake utility functions to detect and handle various Clang/LLVM API differences across versions.

## Usage

Include the submodule and link the special `clang_feature_tests` target:

```cmake
# Include the clang_feature_tests submodule
add_subdirectory(ThirdParty/clang_feature_tests)

# Create your target
add_executable(my_target ...)

# Add include path for clang_features.h into your target
target_link_libraries(my_target clang_feature_tests)
```

After that, you can include the generated header in your source files:

```cpp
#include "clang_features.h"

// Now you can use the feature macros, for example:
#ifdef HAVE_DECAYEDTYPE
// Code that uses DecayedType
#else
// Code that doesn't use DecayedType
#endif
```

## Features Detected

The module detects numerous Clang/LLVM API differences, including but not limited to:

- Method name differences across versions
- Class name changes (e.g., TypedefNameDecl vs TypedefDecl)
- API signature changes
- Header location changes
- Constructor parameter differences
- Return type changes

