cmake_minimum_required(VERSION 3.5)
project(mqtt-c LANGUAGES C)

# MQTT-C build options
option(MQTT_C_OpenSSL_SUPPORT "Build MQTT-C with OpenSSL support?" OFF)
option(MQTT_C_EXAMPLES "Build MQTT-C examples?" ON)
option(MQTT_C_TESTS "Build MQTT-C tests?" OFF)

# MQTT-C library
add_library(mqttc STATIC 
    lib/mqtt_pal.c
    lib/mqtt.c
)
target_include_directories(mqttc PUBLIC include)
target_link_libraries(mqttc )


# Configure with OpenSSL support
if(MQTT_C_OpenSSL_SUPPORT)
    find_package(OpenSSL REQUIRED)
    target_link_libraries(mqttc INTERFACE OpenSSL::SSL)
    target_compile_definitions(mqttc PUBLIC MQTT_USE_BIO)
endif()

# Build examples
if(MQTT_C_EXAMPLES)
    add_executable(pub sample/pub.c)
    target_link_libraries(pub mqttc)
    
    add_executable(sub sample/sub.c)
    target_link_libraries(sub mqttc)
endif()
