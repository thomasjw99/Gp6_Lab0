# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

# == DO NEVER EDIT THE NEXT LINES for Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
   set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()

if(DEFINED ENV{PICO_SDK_PATH})
    set(PICO_SDK_PATH $ENV{PICO_SDK_PATH})
else()
    set(PICO_SDK_PATH ${USERHOME}/.pico-sdk/sdk/2.0.0)
endif()

if(DEFINED ENV{PICO_TOOLCHAIN_PATH})
    set(PICO_TOOLCHAIN_PATH $ENV{PICO_TOOLCHAIN_PATH})
else()
    set(PICO_TOOLCHAIN_PATH ${USERHOME}/.pico-sdk/toolchain/13_2_Rel1)
endif()

set(pioasm_HINT ${USERHOME}/.pico-sdk/tools/2.0.0/pioasm)
if(EXISTS ${pioasm_HINT})
    set(pioasm_DIR ${pioasm_HINT})
endif()
set(picotool_HINT ${USERHOME}/.pico-sdk/picotool/2.0.0/picotool)
if(EXISTS ${picotool_HINT})
    set(picotool_DIR ${picotool_HINT})
endif()
if(PICO_TOOLCHAIN_PATH MATCHES "RISCV")
    set(PICO_PLATFORM rp2350-riscv CACHE STRING "Pico Platform")
    if(PICO_TOOLCHAIN_PATH MATCHES "COREV")
        set(PICO_COMPILER pico_riscv_gcc_zcb_zcmp)
    endif()
endif()
# ====================================================================================
set(PICO_BOARD pico_w CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

if (PICO_SDK_VERSION_STRING VERSION_LESS "1.4.0")
  message(FATAL_ERROR "Raspberry Pi Pico SDK version 1.4.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()

project(testing C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(testing test.c )

pico_set_program_name(testing "test")
pico_set_program_version(testing "0.1")

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(testing 0)
pico_enable_stdio_usb(testing 0)

# Add the standard library to the build
target_link_libraries(testing
        pico_stdlib)

# Add the standard include files to the build
target_include_directories(testing PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
  ${CMAKE_CURRENT_LIST_DIR}/.. # for our common lwipopts or any other standard includes, if required
)

pico_add_extra_outputs(testing)

# include(CTest)
# #include(${PICO_TOOLCHAIN_PATH}/lib/cmake/unity/unityConfig.cmake)
# add_executable(mytest test/test_unity.c test/unity_config.c ${PICO_TOOLCHAIN_PATH}/../../Unity/src/unity.c)
# target_link_libraries(mytest pico_stdlib)
# #target_link_libraries(mytest unity::framework)
# target_include_directories(mytest PRIVATE ${PICO_TOOLCHAIN_PATH}/../../Unity/src test)
# pico_set_binary_type(mytest no_flash)
# find_program(RENODE renode)

# set(RENODE_FLAGS
#   --disable-xwt
#   --port -2
#   --pid-file renode.pid
#   --console
#   )

# add_test(NAME runmytest COMMAND
#     ${RENODE}
#      ${RENODE_FLAGS}
#     -e "$elf=@$<TARGET_FILE:mytest>; $cwd=@${CMAKE_SOURCE_DIR}; include @${CMAKE_SOURCE_DIR}/hello_world.resc"
#     )