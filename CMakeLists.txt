cmake_minimum_required(VERSION 3.20)
project(NANIGameEngine)

# 设置第三方库路径
set(THIRDPARTY_DIR ${CMAKE_SOURCE_DIR}/TP)

# 添加主可执行文件
add_executable(${PROJECT_NAME} src/main.cpp)

# 包含目录
target_include_directories(${PROJECT_NAME} PRIVATE
    ${THIRDPARTY_DIR}/glfw/include
    ${THIRDPARTY_DIR}/glad/include
    ${THIRDPARTY_DIR}/imgui/include
    ${THIRDPARTY_DIR}/glm/include
    ${THIRDPARTY_DIR}/stb/include
    ${THIRDPARTY_DIR}/assimp/include
    ${THIRDPARTY_DIR}/bullet3/include
)

# 链接库
target_link_directories(${PROJECT_NAME} PRIVATE
    ${THIRDPARTY_DIR}/glfw/lib/$<CONFIG>
    ${THIRDPARTY_DIR}/imgui/lib/$<CONFIG>
    ${THIRDPARTY_DIR}/assimp/lib/$<CONFIG>
    ${THIRDPARTY_DIR}/bullet3/lib/$<CONFIG>
)

target_link_libraries(${PROJECT_NAME} PRIVATE
    glfw3
    ImGui
    assimp-vc143-mt
    BulletDynamics
    BulletCollision
    LinearMath
    opengl32
)

# 添加GLAD源文件
target_sources(${PROJECT_NAME} PRIVATE
    ${THIRDPARTY_DIR}/glad/src/glad.c
)

# 复制运行时DLL
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
        ${THIRDPARTY_DIR}/assimp/bin/$<CONFIG>/assimp-vc143-mt.dll
        $<TARGET_FILE_DIR:${PROJECT_NAME}>
)