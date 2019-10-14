cmake_minimum_required(VERSION 3.0)


set(SDK_LIBRARY ${PROJECT_NAME}_static)
add_library(${SDK_LIBRARY} STATIC "")

set(LIVOX_SDK_MAJOR_VERSION "1")
set(LIVOX_SDK_MINOR_VERSION "2")
set(LIVOX_SDK_PATCH_VERSION "1")
set(LIVOX_SDK_VERSION_STRING "${LIVOX_SDK_MAJOR_VERSION}.${LIVOX_SDK_MINOR_VERSION}.${LIVOX_SDK_PATCH_VERSION}")




target_link_libraries(${SDK_LIBRARY}
        PUBLIC
        CONAN_PKG::apache-apr)


find_package(Boost REQUIRED COMPONENTS system function atomic smart_ptr thread)
target_link_libraries(${SDK_LIBRARY}
    PUBLIC
    CONAN_PKG::boost_smart_ptr
    CONAN_PKG::boost_system
    CONAN_PKG::boost_thread
    CONAN_PKG::boost_atomic
    CONAN_PKG::boost_function)

target_include_directories(${SDK_LIBRARY}
        PUBLIC
        include
        include/third_party/FastCRC
        include/third_party/spdlog
        PRIVATE
        src)

set_target_properties(${SDK_LIBRARY} PROPERTIES PUBLIC_HEADER "include/livox_def.h;include/livox_sdk.h")

target_compile_options(${SDK_LIBRARY}
        PRIVATE $<$<CXX_COMPILER_ID:GNU>:-Wall -Werror -Wno-c++11-long-long>
        PRIVATE $<$<CXX_COMPILER_ID:AppleClang>:-Wno-unknown-pragmas -Wall -Werror -Wno-c++11-long-long>
        PRIVATE $<$<CXX_COMPILER_ID:Clang>:-Wno-unknown-pragmas -Wall -Werror -Wno-c++11-long-long>)

target_sources(${SDK_LIBRARY}
        PRIVATE
        src/third_party/FastCRC/FastCRC_tables.hpp
        src/third_party/FastCRC/FastCRCsw.cpp
        src/base/io_loop.h
        src/base/io_loop.cpp
        src/base/thread_base.h
        src/base/thread_base.cpp
        src/base/io_thread.h
        src/base/io_thread.cpp
        src/base/network_util.h
        src/base/network_util.cpp
        src/base/logging.h
        src/base/logging.cpp
        src/base/noncopyable.h
        src/base/util.cpp
        src/livox_sdk.cpp
        src/device_discovery.h
        src/device_discovery.cpp
        src/device_manager.h
        src/device_manager.cpp
        src/comm/comm_port.cpp
        src/comm/sdk_protocol.h
        src/comm/sdk_protocol.cpp
        src/data_handler/data_handler.h
        src/data_handler/data_handler.cpp
        src/data_handler/hub_data_handler.h
        src/data_handler/hub_data_handler.cpp
        src/data_handler/lidar_data_handler.h
        src/data_handler/lidar_data_handler.cpp
        src/command_handler/command_handler.h
        src/command_handler/command_handler.cpp
        src/command_handler/command_channel.h
        src/command_handler/command_channel.cpp
        src/command_handler/hub_command_handler.h
        src/command_handler/hub_command_handler.cpp
        src/command_handler/lidar_command_handler.h
        src/command_handler/lidar_command_handler.cpp
        src/command_handler/command_impl.cpp
        src/command_handler/command_impl.h)


install(TARGETS ${SDK_LIBRARY}
        PUBLIC_HEADER DESTINATION include
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib)
