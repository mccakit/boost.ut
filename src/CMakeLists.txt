add_library(boost_ut STATIC)
file(GLOB_RECURSE srcs CONFIGURE_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)
target_sources(boost_ut PUBLIC FILE_SET CXX_MODULES FILES ${srcs})
target_include_directories(boost_ut PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
include(GNUInstallDirs)
install(
    TARGETS boost_ut
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    CXX_MODULES_BMI DESTINATION ${CMAKE_INSTALL_LIBDIR}/cxx/bmi
)
configure_file(${CMAKE_SOURCE_DIR}/packaging/boost_ut.pc.in boost_ut.pc @ONLY)
install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/boost_ut.pc
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)
