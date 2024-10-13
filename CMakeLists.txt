cmake_minimum_required(VERSION 3.24)
PROJECT(LZ4)


add_library(${PROJECT_NAME} STATIC lz4.c lz4file.c lz4frame.c lz4hc.c lz4io.c xxhash.c)
target_include_directories(${PROJECT_NAME} PRIVATE .)
target_compile_definitions(${PROJECT_NAME} PRIVATE -DXXH_NAMESPACE=LZ4_)
install(TARGETS ${PROJECT_NAME} ARCHIVE DESTINATION lib)

install(FILES lz4.h lz4file.h lz4frame.h lz4frame_static.h lz4hc.h lz4io.h xxhash.h DESTINATION include)
install(FILES LZ4Config.cmake DESTINATION lib)

install(
	TARGETS ${PROJECT_NAME}
	EXPORT ${PROJECT_NAME}Targets
	PUBLIC_HEADER DESTINATION include
	ARCHIVE DESTINATION lib
 	LIBRARY DESTINATION lib
	RUNTIME DESTINATION bin
)

install(
	EXPORT ${PROJECT_NAME}Targets
	FILE ${PROJECT_NAME}Targets.cmake
	DESTINATION lib/cmake/
)

set(INCLUDE_DIRS include)
set(LIBRARIES lz4)
set(LIB_DIR lib)

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
	${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
	VERSION 1.1.1
	COMPATIBILITY SameMajorVersion
)


configure_package_config_file(
	${PROJECT_SOURCE_DIR}/${PROJECT_NAME}Config.cmake.in
	${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
	INSTALL_DESTINATION lib/cmake/
	PATH_VARS INCLUDE_DIRS LIBRARIES LIB_DIR
	INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX}
)

install(
	FILES ${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake ${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
	DESTINATION lib/cmake/
)
