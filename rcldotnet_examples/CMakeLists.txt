cmake_minimum_required(VERSION 3.5)

project(rcldotnet_examples C)

find_package(ament_cmake REQUIRED)
find_package(rcldotnet REQUIRED)

find_package(rcldotnet_common REQUIRED)

find_package(builtin_interfaces REQUIRED)
find_package(std_msgs REQUIRED)
find_package(std_srvs REQUIRED)
find_package(test_msgs REQUIRED)
find_package(rcldotnet REQUIRED)

find_package(dotnet_cmake_module REQUIRED)

set(CSHARP_TARGET_FRAMEWORK "netcoreapp6.0")
find_package(DotNETExtra REQUIRED)

if (BUILD_STANDALONE)
  set(CSBUILD_BUILD_FLAGS "build")
endif()

set(_assemblies_dep_dlls
    ${rcldotnet_common_ASSEMBLIES_DLL}
    ${rcldotnet_ASSEMBLIES_DLL}
    ${builtin_interfaces_ASSEMBLIES_DLL}
    ${rcl_interfaces_ASSEMBLIES_DLL}
    ${std_msgs_ASSEMBLIES_DLL}
    ${std_srvs_ASSEMBLIES_DLL}
    ${test_msgs_ASSEMBLIES_DLL}
)

add_dotnet_executable(rcldotnet_talker
  RCLDotnetTalker.cs
  INCLUDE_DLLS
  ${_assemblies_dep_dlls}
)
add_dotnet_executable(rcldotnet_listener
  RCLDotnetListener.cs
  INCLUDE_DLLS
  ${_assemblies_dep_dlls}
)
add_dotnet_executable(rcldotnet_example_service
  RCLDotnetService.cs
  INCLUDE_DLLS
  ${_assemblies_dep_dlls}
)
# avoid naming conflict with rcldotnet_client library
add_dotnet_executable(rcldotnet_example_client
  RCLDotnetClient.cs
  INCLUDE_DLLS
  ${_assemblies_dep_dlls}
)

add_dotnet_executable(rcldotnet_guard_condition
  RCLDotnetGuardCondition.cs
  INCLUDE_DLLS
  ${_assemblies_dep_dlls}
)

add_dotnet_executable(rcldotnet_example_action_server
  RCLDotnetActionServer.cs
  INCLUDE_DLLS
  ${_assemblies_dep_dlls}
)

add_dotnet_executable(rcldotnet_example_action_client
  RCLDotnetActionClient.cs
  INCLUDE_DLLS
  ${_assemblies_dep_dlls}
)

macro(install_example _target)
  if (BUILD_STANDALONE)
    set_target_properties(${_target}
      PROPERTIES
        OUTPUT_PATH ${CSHARP_BUILDER_OUTPUT_PATH}/${CSHARP_TARGET_FRAMEWORK}/
    )
  endif()

  install_dotnet(${_target} DESTINATION lib/${PROJECT_NAME}/dotnet)
endmacro()

install_example(rcldotnet_talker DESTINATION lib/${PROJECT_NAME}/dotnet)
install_example(rcldotnet_listener DESTINATION lib/${PROJECT_NAME}/dotnet)
install_example(rcldotnet_example_service DESTINATION lib/${PROJECT_NAME}/dotnet)
install_example(rcldotnet_example_client DESTINATION lib/${PROJECT_NAME}/dotnet)
install_example(rcldotnet_guard_condition DESTINATION lib/${PROJECT_NAME}/dotnet)
install_example(rcldotnet_example_action_server DESTINATION lib/${PROJECT_NAME}/dotnet)
install_example(rcldotnet_example_action_client DESTINATION lib/${PROJECT_NAME}/dotnet)

ament_package()
