#
# Copyright 2018, Data61
# Commonwealth Scientific and Industrial Research Organisation (CSIRO)
# ABN 41 687 119 230.
#
# This software may be distributed and modified according to the terms of
# the BSD 2-Clause license. Note that NO WARRANTY is provided.
# See "LICENSE_BSD2.txt" for details.
#
# @TAG(DATA61_BSD)
#

include(${SEL4_TUTORIALS_DIR}/settings.cmake)
sel4_tutorials_regenerate_tutorial(${CMAKE_CURRENT_SOURCE_DIR})

cmake_minimum_required(VERSION 3.7.2)
# declare the dynamic-1 CMake project and the languages it is written in
project(dynamic-1 C ASM)

sel4_tutorials_setup_roottask_tutorial_environment()

add_executable(dynamic-1 main.c)

target_link_libraries(dynamic-1
    sel4
    muslc utils sel4tutorials
    sel4muslcsys sel4platsupport sel4utils sel4debug sel4allocman)

include(rootserver)
DeclareRootserver(dynamic-1)
/*? macros.cmake_check_script(state) ?*/
