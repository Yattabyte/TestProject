#################
### Math Test ###
#################
set(Module MathTest)

# Create Library using the supplied files
set(FILES
    ${PROJECT_SOURCE_DIR}/src/mat.hpp
    ${PROJECT_SOURCE_DIR}/src/vec.hpp
    mathTest.cpp
)
add_executable(${Module} ${FILES})
target_include_directories(${Module}
    PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
    PUBLIC ${PROJECT_SOURCE_DIR}/src
)

# Add library dependencies
target_compile_features(${Module} PRIVATE cxx_std_17)
target_link_libraries(${Module} PUBLIC ${CMAKE_THREAD_LIBS_INIT})
target_link_libraries(${Module} PUBLIC glfw OpenGL::GL)
if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang" AND "${CXX_COMPILER_VERSION}" LESS_EQUAL "9.0")
    target_link_libraries(${Module} PRIVATE c++experimental stdc++fs>)
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    target_link_libraries(${Module} PRIVATE stdc++fs)
endif()

# Set all project settings
target_compile_Definitions(${Module} PRIVATE $<$<CONFIG:DEBUG>:DEBUG>)
set_target_properties(${Module} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}
    LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}
    ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}
    PDB_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}
    VERSION ${PROJECT_VERSION}
)

add_test(NAME MathTest COMMAND ${Module})