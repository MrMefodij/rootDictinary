cmake_minimum_required(VERSION 3.9)
project (readwrite_twoints CXX)

# Look for ROOT. As long as an installation is available in the environment, this should succeed.
message(STATUS "Looking for ROOT")
find_package(ROOT REQUIRED COMPONENTS RIO)
message(STATUS "ROOT ${ROOT_VERSION} found at ${ROOT_BINDIR}")

AUX_SOURCE_DIRECTORY(./oaEvent                 SOURCE_FILES)

add_executable(readwrite_twoints readwrite_twoints.cpp ${SOURCE_FILES} )
message(STATUS ${SOURCE_FILES} )
target_link_libraries(readwrite_twoints ROOT::RIO)

# attach dictionaries to the executable. first, tell it where to look for headers required by the dictionaries:
target_include_directories(readwrite_twoints PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/oaEvent)
# then generate dictionaries and add them as a dependency of the executable (via the MODULE parameter):
ROOT_GENERATE_DICTIONARY(oaEvents_dict oaEvent/twoints.hpp oaEvent/TChannelId.hxx MODULE readwrite_twoints LINKDEF oaEvent/LinkDef.h)
