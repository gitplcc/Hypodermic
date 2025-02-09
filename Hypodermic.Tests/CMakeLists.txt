find_package(Boost REQUIRED COMPONENTS system unit_test_framework)

set(HypodermicTests_sources
  main.cpp
  CircularDependencyTests.cpp
  ContainerBuilderTests.cpp
  ContainerTests.cpp
  DefaultConstructibleTests.cpp
  FactoryTests.cpp
  IsCompleteTests.cpp
  MemoryTests.cpp
  NamedTests.cpp
  NestedContainerTests.cpp
  PerformanceTests.cpp
  PersistentInstanceRegistrationTests.cpp
  ProvidedDependenciesTests.cpp
  ProvidedInstanceFactoryRegistrationTests.cpp
  ProvidedInstanceRegistrationTests.cpp
  RegistrationTests.cpp
  RuntimeRegistrationTests.cpp
  UseIfNoneTests.cpp
)

add_executable(Hypodermic.Tests)
target_sources(Hypodermic.Tests PRIVATE ${HypodermicTests_sources})
target_compile_features(Hypodermic.Tests INTERFACE cxx_std_11)
set_target_properties(Hypodermic.Tests PROPERTIES CXX_EXTENSIONS OFF)
target_link_libraries(Hypodermic.Tests
  PRIVATE
    Hypodermic::Hypodermic
    Boost::unit_test_framework
    Boost::dynamic_linking
)

add_test(
  NAME cmake_Hypodermic.Tests
  COMMAND Hypodermic.Tests --result_code=no --report_level=no
)
