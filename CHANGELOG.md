# Change Log

v1.0.5

- Addressed some clang-tidy suggestions

v1.0.4

- Moved inclusion of `GNUInstallDirs`

v1.0.3

- Changed the memory comparison to use `std::memcmp` rather than a slow loop
- Changed string constants to `const char * const` for simplicity and eye
  toward C++11 compatibility, also removed `constexpr` and nested namespaces
  since those broke C++11 compatibility
- Minor CMakeLists.txt change

v1.0.2

- Revised build options for Windows to enable additional warnings
- Enable warnings to be treated as errors during builds
- Employ `if constexpr` for if statements that were constant

v1.0.1

- Updated link dependencies to link with `Threads::Threads`

v1.0.0

- Initial Release
