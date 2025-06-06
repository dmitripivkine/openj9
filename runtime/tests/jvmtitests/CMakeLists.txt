################################################################################
# Copyright IBM Corp. and others 2018
#
# This program and the accompanying materials are made available under
# the terms of the Eclipse Public License 2.0 which accompanies this
# distribution and is available at https://www.eclipse.org/legal/epl-2.0/
# or the Apache License, Version 2.0 which accompanies this distribution and
# is available at https://www.apache.org/licenses/LICENSE-2.0.
#
# This Source Code may also be made available under the following
# Secondary Licenses when the conditions for such availability set
# forth in the Eclipse Public License, v. 2.0 are satisfied: GNU
# General Public License, version 2 with the GNU Classpath
# Exception [1] and GNU General Public License, version 2 with the
# OpenJDK Assembly Exception [2].
#
# [1] https://www.gnu.org/software/classpath/license.html
# [2] https://openjdk.org/legal/assembly-exception.html
#
# SPDX-License-Identifier: EPL-2.0 OR Apache-2.0 OR GPL-2.0-only WITH Classpath-exception-2.0 OR GPL-2.0-only WITH OpenJDK-assembly-exception-1.0
################################################################################

j9vm_add_library(j9vm_jvmtitest_includes INTERFACE)
target_include_directories(j9vm_jvmtitest_includes
	INTERFACE
		"${CMAKE_CURRENT_SOURCE_DIR}/include"
)

# If building with OpenXL on AIX, we need to skip the strip step for libjvmtitest.so
# (https://github.com/eclipse-openj9/openj9/issues/21528).
if (OMR_OS_AIX AND CMAKE_C_COMPILER_IS_OPENXL)
	j9vm_add_library(jvmtitest SHARED SKIP_STRIP
		fakeref.c
	)
else()
	j9vm_add_library(jvmtitest SHARED
		fakeref.c
	)
endif()

target_link_libraries(jvmtitest
	PRIVATE
		j9vm_interface
		jvmti_test_src
		jvmti_test_agent
)
include("exports.cmake")

add_subdirectory(agent)
add_subdirectory(src)
