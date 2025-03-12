cmake_minimum_required(VERSION 3.16...3.28)

project(Hypodermic
  VERSION 
    2.5.0
  DESCRIPTION 
    "Hypodermic is a non-intrusive header library only IoC container for C++."
  HOMEPAGE_URL
    "https://github.com/ybainier/Hypodermic"
  LANGUAGES
    CXX
)

include(FeatureSummary)
include(GNUInstallDirs)

include(CheckPIESupported)
check_pie_supported(LANGUAGES CXX)
add_feature_info(PIE CMAKE_CXX_LINK_PIE_SUPPORTED
                 "position independent code generation")

if (NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build." FORCE)
  set_property(CACHE CMAKE_BUILD_TYPE
    PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo"
  )
endif()

if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
  include(CTest)
endif()

add_subdirectory(Hypodermic)
if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME AND BUILD_TESTING)
  add_subdirectory(Hypodermic.Tests)

  include(CMakePackageConfigHelpers)
  write_basic_package_version_file(
    HypodermicConfigVersion.cmake
    VERSION ${PACKAGE_VERSION}
    COMPATIBILITY AnyNewerVersion
  )

  install(
    EXPORT HypodermicTargets
    FILE HypodermicConfig.cmake
    NAMESPACE Hypodermic::
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/Hypodermic"
  )
  install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/HypodermicConfigVersion.cmake"
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/Hypodermic"
  )

  # uninstall target
  ##################
  #

  configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY
  )

  add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake
  )

  # Packaging stuff
  #################
  #

  set(CPACK_PACKAGE_DESCRIPTION
    "Hypodermic is a non-intrusive header only IoC container for C++. "
    "It provides dependency injection to your existing design by managing the "
    "creation of your components and their dependencies in the right order, "
    "sparing you the trouble of writing and maintaining boiler plate code."
  )
  set(CPACK_SOURCE_GENERATOR "TBZ2")
  set(CPACK_SOURCE_IGNORE_FILES
    "/.git"
    "/.hg/"
    "/.hgignore$"
    "build/"
    "ipch/"
    "/resources/"
    ".sdf$;.suo$;.tss$"
  )
  include(CPack)

  #~ add_custom_target(dist COMMAND ${CMAKE_MAKE_PROGRAM} package_source)
endif()

feature_summary(
  QUIET_ON_EMPTY
  WHAT
    DISABLED_FEATURES
  DESCRIPTION "Disabled features:"
)
 
