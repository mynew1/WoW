
if(NOT BUILD_EVENTSSCRIPTS )
  message("\tEventsScripts was skipped because BUILD_EVENTSSCRIPTS is unchecked.")
  return()
endif(NOT BUILD_EVENTSSCRIPTS)

PROJECT(EventsScripts)

SET( prefix ${SCRIPTS_PATH}/${PROJECT_NAME} )

#sources -contains source files for our library.
SET( sources
Setup.cpp
Event_DarkPortal.cpp
../Common/Instance_Base.cpp
../Common/Base.cpp )

SET( headers
Setup.h
Event_DarkPortal.h
../Common/Base.h
../Common/EasyFunctions.h
../Common/Instance_Base.h )

foreach(src IN ITEMS ${sources} )
  SET( SRCS ${SRCS} ${prefix}/${src} )
endforeach(src)
foreach(src IN ITEMS ${headers} )
  SET( HEADERS ${HEADERS} ${prefix}/${src} )
endforeach(src)

include_directories( ${include_dirs} ${prefix} )
ADD_LIBRARY( ${PROJECT_NAME} SHARED ${SRCS} ${HEADERS} )
ADD_DEPENDENCIES( ${PROJECT_NAME} ${dependancies} )
target_link_libraries( ${PROJECT_NAME} ${linkages} )

include(PCHSupport)
IF( CMAKE_GENERATOR MATCHES Visual*)
	ADD_NATIVE_PRECOMPILED_HEADER(${PROJECT_NAME} Setup.h ${prefix}/Setup.cpp)
ENDIF()

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION ${SCRIPTLIB_PATH} LIBRARY DESTINATION ${SCRIPTLIB_PATH} )
#Cleap up our variables(Not required.)
unset( prefix)
unset( SRCS )
unset( sources )
