set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR})

include(ECMMarkAsTest)

find_package(Qt${QT_MAJOR_VERSION}Test CONFIG REQUIRED)

########### next target ###############

add_executable(testlibsyndication testlibsyndication.cpp)
target_link_libraries(testlibsyndication KF5Syndication)

########### next target ###############

add_executable(testpersonimpl testpersonimpl.cpp)
add_test(NAME testpersonimpl COMMAND testpersonimpl)
ecm_mark_as_test(testpersonimpl)
target_link_libraries(testpersonimpl KF5Syndication Qt${QT_MAJOR_VERSION}::Test)

#########
add_executable(syndicationtest syndicationtest.cpp)
target_compile_definitions(syndicationtest PRIVATE -DSYNDICATION_DATA_DIR="${CMAKE_CURRENT_SOURCE_DIR}")
add_test(NAME syndicationtest COMMAND syndicationtest)
ecm_mark_as_test(syndicationtest)
target_link_libraries(syndicationtest KF5Syndication Qt${QT_MAJOR_VERSION}::Test)

#########
add_executable(loaderutiltest loaderutiltest.cpp)
target_compile_definitions(loaderutiltest PRIVATE -DSYNDICATION_DATA_FEEDPARSING_DIR="${CMAKE_CURRENT_SOURCE_DIR}/feeds/")
add_test(NAME loaderutiltest COMMAND loaderutiltest)
ecm_mark_as_test(loaderutiltest)
target_link_libraries(loaderutiltest KF5Syndication Qt${QT_MAJOR_VERSION}::Test)
