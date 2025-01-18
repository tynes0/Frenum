# Frenum: An Advanced Enum Management Library

`frenum` is a modern C++ library that provides advanced features for working with enums. It allows you to map enum values to strings, retrieve enum indices, iterate over enums, and use enums across different namespaces seamlessly. The library leverages modern C++ features such as `concepts`, `constexpr`, and `std::optional` to deliver both performance and safety.

---

## Features

- **String Conversion:** Convert enum values to their string representations and vice versa.
- **Namespace Support:** Fully supports enums defined inside namespaces.
- **Type Safety:** Guarantees type safety using `concepts` and compile-time validation.
- **Enum Metadata:** Retrieve metadata such as size, name, and indices of enum values.
- **Iteration Support:** Iterate over all enum values effortlessly.
- **Custom Namespace Registration:** Seamlessly handle enums from any namespace.
- **Modern C++ Compatibility:** Built with C++20, utilizing features like `concepts` and `std::optional`.

---

## Installation

Simply include the `frenum.h` file in your project.

```cpp
#include "frenum.h"
```

Ensure your project is compiled with C++20 or later:

```bash
g++ -std=c++20 -o my_project main.cpp
```

---

## Usage

### Basic Enum Declaration

Using `Frenum` for defining enums:

```cpp
#include "frenum.h"

Frenum(MyEnum, int, Value1 = 1, Value2, Value3);

int main() {
    // Convert enum to string
    std::cout << frenum::to_string(MyEnum::Value1) << std::endl; // Output: "Value1"

    // Get enum index
    constexpr auto index = frenum::index(MyEnum::Value2);
    if (index.has_value()) {
        std::cout << "Index: " << *index << std::endl;
    }
}
```

### Enum in a Namespace

Using `FrenumInNamespace` for namespace-specific enums:

```cpp
namespace MyNamespace {
    FrenumInNamespace(MyNamespace, MyEnum, int, Alpha = 0, Beta, Gamma);
}

int main() {
    // Convert enum to string
    std::cout << frenum::to_string(MyNamespace::MyEnum::Beta) << std::endl; // Output: "Beta"
}
```

### Class-Style Enums

Using `FrenumClass` for `enum class` types:

```cpp
FrenumClass(StrongEnum, int, A = 10, B, C);

int main() {
    std::cout << frenum::to_string(StrongEnum::B) << std::endl; // Output: "B"
}
```

### Extending Existing Enums

You can use `MakeFrenum` to register an already defined enum:

```cpp
enum LegacyEnum { OldValue1 = 1, OldValue2, OldValue3 };

MakeFrenum(LegacyEnum, OldValue1, OldValue2, OldValue3);

int main() {
    std::cout << frenum::to_string(LegacyEnum::OldValue2) << std::endl; // Output: "OldValue2"
}
```

### Using Enums with Custom Namespaces

Registering enums defined in namespaces using `MakeFrenumWithNamespace`:

```cpp
namespace CustomNamespace {
  namespace AnotherNamespace {
    enum ExampleEnum { First, Second, Third };
  }
}

MakeFrenumWithNamespace(CustomNamespace::AnotherNamespace, ExampleEnum, First, Second, Third);

int main() {
    std::cout << frenum::to_string(CustomNamespace::ExampleEnum::Second) << std::endl; // Output: "Second"
}
```

### Using Enums Directly Within Namespaces

For enums defined inside namespaces, use `MakeFrenumInNamespace`:

```cpp
namespace AnotherNamespace {
    enum NestedEnum { One, Two, Three };
    MakeFrenumInNamespace(AnotherNamespace, NestedEnum, One, Two, Three);
}

int main() {
    using namespace AnotherNamespace;
    std::cout << frenum::to_string(NestedEnum::Three) << std::endl; // Output: "Three"
}
```

---

## Accessing Metadata and Conversion Functions

### Retrieve Underlying Value

The `underlying` function retrieves the raw value of an enum:

```cpp
Frenum(MyEnum, int, A = 10, B, C);

int main() {
    std::cout << "Underlying value: " << frenum::underlying(MyEnum::B) << std::endl; // Output: 11 // Same with frenum::value
}
```

### Cast from String to Enum

The `cast` function converts a string to an enum value:

```cpp
Frenum(MyEnum, int, X = 0, Y, Z);

int main() {
    constexpr auto value = frenum::cast<MyEnum>("Y");
    if (value.has_value()) {
        std::cout << "Casted value: " << frenum::underlying(*value) << std::endl; // Output: 1
    } else {
        std::cout << "Invalid string!" << std::endl;
    }
}
```

### Check if an Enum is Frenum-Managed

The `is_frenum_v` variable tells whether a given enum is managed by `frenum`:

```cpp
Frenum(MyEnum, int, A, B, C);
enum NotManagedEnum1 { X, Y, Z };
enum NotManagedEnum2 { X2, Y2, Z2 };

MakeFrenum(NotManagedEnum2, X2, Y2, Z2);

int main() {
    std::cout << std::boolalpha;
    std::cout << "Is MyEnum a frenum? " << frenum::is_frenum_v<MyEnum> << std::endl; // Output: true

    std::cout << "Is NotManagedEnum1 a frenum? " << frenum::is_frenum_v<NotManagedEnum1> << std::endl; // Output: false
    std::cout << "Is NotManagedEnum2 a frenum? " << frenum::is_frenum_v<NotManagedEnum2> << std::endl; // Output: true
}
```

---

## Requirements

- C++20 or later
- A modern C++ compiler (e.g., GCC 10+, Clang 10+, MSVC 2019+)

---

## Contributing

Feel free to contribute by submitting pull requests or reporting issues. Before contributing, please ensure that your changes are covered with tests.

---

## License

This project is licensed under the MIT License. See the LICENSE file for details.

---
