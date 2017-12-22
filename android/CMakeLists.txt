
set(TARGET_NAME vulkan_examples)

set(SHADER_ROOT_DIR "${PROJECT_SOURCE_DIR}/data/shaders")
set(SHADER_DIR "${SHADER_ROOT_DIR}/triangle")
file(GLOB SHADERS
    ${SHADER_DIR}/*.vert
    ${SHADER_DIR}/*.frag
    ${SHADER_DIR}/*.comp
    ${SHADER_DIR}/*.tesc
    ${SHADER_DIR}/*.tese
    ${SHADER_DIR}/*.geom
    ${SHADER_ROOT_DIR}/base/*.vert
    ${SHADER_ROOT_DIR}/base/*.frag
)

source_group("Shaders" FILES ${SHADERS})
foreach(SHADER ${SHADERS})
    compile_spirv_shader(${SHADER})
    list(APPEND COMPILED_SHADERS ${COMPILE_SPIRV_SHADER_RETURN})
endforeach()

# grab the implementation and header files
file(GLOB_RECURSE LIB_SRCS "src/main/cpp/*.h" "src/main/cpp/*.hpp" "src/main/cpp/*.cpp" "src/main/cpp/*.c")
list(APPEND ${TARGET_NAME}_SRCS ${LIB_SRCS})
add_library(${TARGET_NAME} SHARED ${LIB_SRCS} ${COMPILED_SHADERS})
target_include_directories(${TARGET_NAME} PUBLIC "${PROJECT_SOURCE_DIR}/../base")
target_link_libraries(${TARGET_NAME} android log m vulkan app-glue)
