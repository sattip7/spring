SET(headlessStubsSources
		"glewstub"
		"glstub"
		"glustub"
		"sdlstub"
		"sdlstub_cppbit"
	)

IF    (WIN32)
	# We still need these header files,
	# even if we are not going to link with SDL.
	# We have them available anyway (mingwlibs).
	FIND_PACKAGE(SDL REQUIRED)
	INCLUDE_DIRECTORIES(${SDL_INCLUDE_DIR})
ELSE  (WIN32)
	# Use a direct copy of the GL and SDL headers,
	# as these may not be available on headless systems.
	INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/include)
	INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/include/SDL)
ENDIF (WIN32)

ADD_LIBRARY(headlessStubs STATIC EXCLUDE_FROM_ALL ${headlessStubsSources})
TARGET_LINK_LIBRARIES(headlessStubs ${Boost_THREAD_LIBRARY} ${Boost_CHRONO_LIBRARY_WITH_RT})
SET_TARGET_PROPERTIES(headlessStubs PROPERTIES COMPILE_FLAGS "${PIC_FLAG}")

