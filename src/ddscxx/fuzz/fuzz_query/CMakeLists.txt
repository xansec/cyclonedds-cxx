#
# Copyright(c) 2006 to 2022 ZettaScale Technology and others
#
# This program and the accompanying materials are made available under the
# terms of the Eclipse Public License v. 2.0 which is available at
# http://www.eclipse.org/legal/epl-2.0, or the Eclipse Distribution License
# v. 1.0 which is available at
# http://www.eclipse.org/org/documents/edl-v10.php.
#
# SPDX-License-Identifier: EPL-2.0 OR BSD-3-Clause
#

idlcxx_generate(TARGET ddscxx_test_types_fuzz FILES
  ../../tests/data/Space.idl
  WARNINGS no-implicit-extensibility)

add_executable(fuzz_query fuzz_query.cpp)

set_property(TARGET fuzz_query PROPERTY CXX_STANDARD ${cyclonedds_cpp_std_to_use})

target_include_directories(
  fuzz_query PRIVATE
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../../include/dds>")

target_link_libraries(fuzz_query ddscxx_test_types_fuzz CycloneDDS-CXX::ddscxx $ENV{LIB_FUZZING_ENGINE})

target_compile_options(fuzz_query PUBLIC -fsanitize=fuzzer)
target_link_options(fuzz_query PUBLIC -fsanitize=fuzzer)
