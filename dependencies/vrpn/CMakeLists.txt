# 2009-2010 Ryan Pavlik <rpavlik@iastate.edu> <abiryan@ryandnet>
# http://academic.cleardefinition.com/
# Iowa State University HCI Graduate Program/VRAC

set(_url http://www.hci.iastate.edu/~rpavlik/downloads/vrpn/rpavlik-vrpn-wmhtgui-sensitivity-0-gabeebb0.zip)

# Disable everything except just what we need
set(_args)
foreach(_option BUILD_TESTING
	VRPN_BUILD_CLIENTS
	VRPN_BUILD_EXTRA_COMPILER_WARNINGS
	VRPN_BUILD_PROFILING_SUPPORT
	VRPN_BUILD_PYTHON
	VRPN_BUILD_SERVERS
	VRPN_BUILD_TEST_RPC_GENERATION
	VRPN_INCLUDE_INTERSENSE
	VRPN_INCLUDE_PHASESPACE
	VRPN_INCLUDE_TIMECODE_SERVER
	VRPN_USE_FREESPACE
	VRPN_USE_GPM_MOUSE
	VRPN_USE_MICROSCRIBE
	VRPN_USE_MOTIONNODE
	VRPN_USE_NATIONAL_INSTRUMENTS
	VRPN_USE_NATIONAL_INSTRUMENTS_MX
	VRPN_USE_NIDAQ
	VRPN_USE_PHANTOM_SERVER
	VRPN_USE_USDIGITAL)
	list(APPEND _args -D${_option}=OFF)
endforeach()

ExternalProject_Add(vrpn
	DEPENDS wiiuse
	URL "${_url}"
	CONFIGURE_COMMAND
		"${CMAKE_COMMAND}"
		"-G${CMAKE_GENERATOR}"
		-DCMAKE_BUILD_TYPE=Release
		-DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
		${_args}
		-DVRPN_BUILD_CLIENT_LIBRARY=ON
		-DVRPN_BUILD_SERVER_LIBRARY=ON
		-DVRPN_USE_WIIUSE=ON
		-DVRPN_GPL_SERVER=ON
		"-DWIIUSE_ROOT_DIR=${WIIUSE_ROOT_DIR}"
		"-DWIIUSE_LIBRARY=${WIIUSE_LIBRARY}"
		"-DWIIUSE_RUNTIME_LIBRARY=${WIIUSE_RUNTIME_LIBRARY}"
		"-DWIIUSE_INCLUDE_DIR=${WIIUSE_INCLUDE_DIR}"
		<SOURCE_DIR>
	INSTALL_DIR ${DEPENDENCY_INSTALL_DIR}
)

ExternalProject_Add_Step(vrpn create_fake_docs
		COMMAND ${CMAKE_COMMAND}
			-E make_directory "<BINARY_DIR>/doxygen/docs-generated/html"
		DEPENDEES build
		DEPENDERS install)

set(VRPN_ROOT_DIR "${DEPENDENCY_INSTALL_DIR}" PARENT_SCOPE)
