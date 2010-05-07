# Base Io build system
# Written by Jeremy Tregunna <jeremy.tregunna@me.com>
#
# Builds the BigNum addon

# Find libgmp
find_package(GMP)

# Create the _build bundle hierarchy if needed.
make_build_bundle(_build)

# Did we find libevent? if so, set up the targets and all the
# support variables.
if(GMP_FOUND)
	# Output our dynamic library to the top-level _build hierarchy
	set(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/_build/dll)

	# Additional include directories
	include_directories(${GMP_INCLUDE_DIR})

	# Our library sources.
	file(GLOB SRCS "${CMAKE_CURRENT_SOURCE_DIR}/source/*.c")

	# Now build the shared library
	add_library(IoBigNum SHARED ${SRCS})
	add_dependencies(IoBigNum iovmall)
	target_link_libraries(IoBigNum iovmall ${GMP_LIBRARIES})

	# Install the addon to our global addons hierarchy.
	install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR} DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/io/addons)
endif(GMP_FOUND)