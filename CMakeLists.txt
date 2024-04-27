cmake_minimum_required(VERSION 3.10)
set(CMAKE_CXX_STANDARD 17)

project(FluidSimulation)

# [third_party]设置三方库包含路径
set(THIRD_PARTY ${CMAKE_SOURCE_DIR}/third_party)
set(GLAD_DIR ${THIRD_PARTY}/glad/include)
set(GLM_DIR ${THIRD_PARTY}/glm)
set(GLFW_DIR ${THIRD_PARTY}/glfw-3.3.8.bin.WIN64)
set(STB_DIR ${THIRD_PARTY}/stb)

# [common]设置头文件路径
set(COMMON_INC_DIR ${CMAKE_SOURCE_DIR}/common/inc)
set(COMMON_INC_FILES
    ${COMMON_INC_DIR}/ComputeShader.h
    ${COMMON_INC_DIR}/Shader.h
    ${COMMON_INC_DIR}/WCubicSpline.h
    ${COMMON_INC_DIR}/Global.h
    )
# [common]设置源文件路径
set(COMMON_SRC_DIR ${CMAKE_SOURCE_DIR}/common/src)
set(COMMON_SRC_FILES
    ${COMMON_SRC_DIR}/ComputeShader.cpp
    ${COMMON_SRC_DIR}/Shader.cpp
    ${COMMON_SRC_DIR}/WCubicSpline.cpp
    )

# [Fluid3d]设置头文件路径
set(FLUID_INC_DIR ${CMAKE_SOURCE_DIR}/inc)
set(FLUID_INC_FILES
    ${FLUID_INC_DIR}/DepthFilter.h
    ${FLUID_INC_DIR}/FluidShadowMap.h
    ${FLUID_INC_DIR}/Material.h
    ${FLUID_INC_DIR}/Parameters3d.h
    ${FLUID_INC_DIR}/ParticalSystem3d.h
    ${FLUID_INC_DIR}/RenderCamera.h
    ${FLUID_INC_DIR}/RenderWidget.h
    ${FLUID_INC_DIR}/SkyBox.h
    )
# [Fluid3d]设置源文件路径
set(FLUID_SRC_DIR ${CMAKE_SOURCE_DIR}/src)
set(FLUID_SRC_FILES
    ${FLUID_SRC_DIR}/DepthFilter.cpp
    ${FLUID_SRC_DIR}/FluidShadowMap.cpp
    ${FLUID_SRC_DIR}/Material.cpp
    ${FLUID_SRC_DIR}/ParticalSystem3d.cpp
    ${FLUID_SRC_DIR}/RenderCamera.cpp
    ${FLUID_SRC_DIR}/RenderWidget.cpp
    ${FLUID_SRC_DIR}/SkyBox.cpp
    ${CMAKE_SOURCE_DIR}/main.cpp
    )
# [Fluid3d]设置Shader路径
set(FLUID_SHADER_DIR ${CMAKE_SOURCE_DIR}/shaders)
set(FLUID_SHADER_FILES
    ${FLUID_SHADER_DIR}/BlurZ.comp
    ${FLUID_SHADER_DIR}/CausticMap.frag
    ${FLUID_SHADER_DIR}/CausticMap.vert
    ${FLUID_SHADER_DIR}/ComputeParticals.comp
    ${FLUID_SHADER_DIR}/DrawColor3d.frag
    ${FLUID_SHADER_DIR}/DrawColor3d.vert
    ${FLUID_SHADER_DIR}/DrawFluidColor.frag
    ${FLUID_SHADER_DIR}/DrawFluidColor.vert
    ${FLUID_SHADER_DIR}/DrawModel.frag
    ${FLUID_SHADER_DIR}/DrawModel.vert
    ${FLUID_SHADER_DIR}/PointSprite.geom
    ${FLUID_SHADER_DIR}/PointSprite.vert
    ${FLUID_SHADER_DIR}/PointSpriteThickness.frag
    ${FLUID_SHADER_DIR}/PointSpriteZValue.frag
    ${FLUID_SHADER_DIR}/ScreenQuad.frag
    ${FLUID_SHADER_DIR}/ScreenQuad.vert
    ${FLUID_SHADER_DIR}/SkyBox.frag
    ${FLUID_SHADER_DIR}/SkyBox.vert
    )

# 全局包含目录
include_directories(${GLAD_DIR} ${GLM_DIR} ${GLFW_DIR}/include ${STB_DIR} ${COMMON_INC_DIR})

# -----------3D项目-----------
set(PROJECT 3DFluidSimulation)
# 生成exe
add_executable(${PROJECT} 
    ${COMMON_INC_FILES} 
    ${COMMON_SRC_FILES}
    ${FLUID_INC_FILES} 
    ${FLUID_SRC_FILES}
    ${THIRD_PARTY}/glad/src/glad.c
    ${FLUID_SHADER_FILES}
    )
# 包含目录
target_include_directories(${PROJECT} PUBLIC ${FLUID_INC_DIR})
# [third_party]链接glfw静态库
target_link_libraries(${PROJECT} 
    ${GLFW_DIR}/lib-vc2022/glfw3.lib
    )

set(ALL_FILES
    ${COMMON_INC_FILES} 
    ${COMMON_SRC_FILES}
    ${FLUID_INC_FILES} 
    ${FLUID_SRC_FILES}
    ${FLUID_SHADER_FILES}
    )
source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${ALL_FILES})

# [vs]设置启动项目
set_property (DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT})



