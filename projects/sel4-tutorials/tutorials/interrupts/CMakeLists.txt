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
project(interrupts C ASM)

sel4_tutorials_setup_capdl_tutorial_environment()

/*? write_manifest(manifest=".manifest.obj", allocator=".allocator.obj") ?*/
cdl_pp(${CMAKE_CURRENT_SOURCE_DIR}/.manifest.obj cdl_pp_target
	/*- for (elf, file) in state.stash.elfs.items() -*/
    ELF "/*?elf?*/"
    CFILE "${CMAKE_CURRENT_BINARY_DIR}/cspace_/*?elf?*/.c"
    /*- endfor -*/
)

/*- for (elf, file) in state.stash.elfs.items() -*/
/*- if file['filename'] == "timer.c" -*/
add_executable(/*?elf?*/ EXCLUDE_FROM_ALL /*?file['filename']?*/ cspace_/*?elf?*/.c ${SEL4_TUTORIALS_DIR}/zynq_timer_driver/src/driver.c)
target_include_directories(/*?elf?*/ PUBLIC ${SEL4_TUTORIALS_DIR}/zynq_timer_driver/include)
/*- else -*/
add_executable(/*?elf?*/ EXCLUDE_FROM_ALL /*?file['filename']?*/ cspace_/*?elf?*/.c)
/*- endif -*/
add_dependencies(/*?elf?*/ cdl_pp_target)
target_link_libraries(/*?elf?*/ sel4tutorials)

list(APPEND elf_files "$<TARGET_FILE:/*?elf?*/>")
list(APPEND elf_targets "/*?elf?*/")

/*- endfor -*/


cdl_ld("${CMAKE_CURRENT_BINARY_DIR}/spec.cdl" capdl_spec
    MANIFESTS ${CMAKE_CURRENT_SOURCE_DIR}/.allocator.obj
    ELF ${elf_files}
    KEYS ${elf_targets}
    DEPENDS ${elf_targets})

DeclareCDLRootImage("${CMAKE_CURRENT_BINARY_DIR}/spec.cdl" capdl_spec ELF ${elf_files} ELF_DEPENDS ${elf_targets})


/*? macros.cmake_check_script(state) ?*/
