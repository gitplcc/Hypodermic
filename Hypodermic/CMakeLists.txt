add_library(Hypodermic INTERFACE)
add_library(Hypodermic::Hypodermic ALIAS Hypodermic)

set(Hypodermic_headers
    ActivatedRegistrationInfo.h
    ActivationException.h
    ActivationHandler.h
    ActivationHandlers.h
    ActivationRegistry.h
    ActivationResult.h
    AnyArgument.h
    ArgumentPack.h
    ArgumentResolver.h
    As.h
    AsSelf.h
    AutowireableConstructor.h
    AutowireableConstructorRegistrationDescriptor.h
    Behavior.h
    CircularDependencyException.h
    ComponentContext.h
    Config.h
    ConsoleLoggerSink.h
    ConstructorDescriptor.h
    ConstructorTypologyDeducer.h
    Container.h
    ContainerBuilder.h
    ContainerInstanceRegistration.h
    ContainerInstanceRegistrationActivator.h
    DependencyActivationException.h
    DependencyFactories.h
    DependencyFactory.h
    DependencyFactoryTag.h
    EnforceBaseOf.h
    EnforceDependencyNotAlreadyRegistered.h
    ExceptionBase.h
    FactoryWrapper.h
    Hypodermic.h
    ILoggerSink.h
    IMutableRegistrationScope.h
    InstanceAlreadyActivatingException.h
    InstanceFactory.h
    InstanceLifetime.h
    InstanceRegistrationTags.h
    IntegerSequence.h
    IRegistration.h
    IRegistrationActivator.h
    IRegistrationDescriptor.h
    IRegistrationRegistry.h
    IRegistrationScope.h
    IResolutionContainer.h
    IResolutionContext.h
    IRuntimeRegistrationBuilder.h
    IsComplete.h
    IsSupportedArgument.h
    ITypeAlias.h
    Log.h
    Logger.h
    LogLevel.h
    MetaContains.h
    MetaForEach.h
    MetaIdentity.h
    MetaInsert.h
    MetaMap.h
    MetaPair.h
    Named.h
    NamedTypeAlias.h
    NestedRegistrationScope.h
    NoopLoggerSink.h
    OnActivated.h
    PersistentInstanceRegistration.h
    PointerAlignment.h
    Pragmas.h
    ProvidedDependencyTag.h
    ProvidedInstanceDependencyTag.h
    ProvidedInstanceFactoryRegistrationDescriptor.h
    ProvidedInstanceRegistration.h
    ProvidedInstanceRegistrationActivator.h
    ProvidedInstanceRegistrationDescriptor.h
    Registration.h
    RegistrationActivator.h
    RegistrationBuilder.h
    RegistrationContext.h
    RegistrationDescriptorBase.h
    RegistrationDescriptorBuilder.h
    RegistrationDescriptorInfo.h
    RegistrationDescriptorInfoToString.h
    RegistrationException.h
    RegistrationScope.h
    ResolutionContainer.h
    ResolutionContext.h
    ResolutionException.h
    ResolutionInfo.h
    RuntimeRegistrationBuilder.h
    SingleInstance.h
    TypeAlias.h
    TypeAliases.h
    TypeAliasKey.h
    TypeInfo.h
    UseIfNone.h
    With.h
)

target_compile_features(Hypodermic INTERFACE cxx_std_11)
target_compile_options(Hypodermic
  INTERFACE
    $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:AppleClang>,$<CXX_COMPILER_ID:GNU>>:-Wall>
    $<$<CXX_COMPILER_ID:MSVC>:-Wall>
)
set_target_properties(Hypodermic
  PROPERTIES
    CXX_EXTENSIONS OFF
    POSITION_INDEPENDENT_CODE ON
)
target_include_directories(Hypodermic
  INTERFACE
    $<BUILD_INTERFACE:${Hypodermic_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME AND BUILD_TESTING)
  install(
    FILES ${Hypodermic_headers}
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  )
  install(
    TARGETS Hypodermic
    EXPORT  HypodermicTargets
  )

  #########################################
  # pkgconfig support
  #########################################
  set(VERSION "${HYPODERMIC_VERSION}")
  configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/libhypodermic.pc.in
    ${CMAKE_CURRENT_BINARY_DIR}/libhypodermic.pc
    @ONLY
  )

  install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/libhypodermic.pc
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
  )
endif()
