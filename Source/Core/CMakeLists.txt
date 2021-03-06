include_directories(
.. 
../../Include
)

#if(BUILD_SHARED)
	add_definitions(-DBUILD_SHARED_LIB)
#endif()


set(ASSET_SRCS
AssetManager.h
AssetManager.cpp
AsynMeshTask.h
AsynMeshTask.cpp
Image.h
Image.cpp
Mesh.h
Mesh.cpp
ObjectMesh.h
ObjectMesh.cpp
)

source_group(
Asset FILES
${ASSET_SRCS}
)

set(UTIL_SRCS
LogUtil.h
LogUtil.cpp
Utils/StringUtils.h
Utils/StringUtils.cpp
Utils/MD5.h
Utils/MD5.cpp
Utils/farmhash.h
Utils/farmhash.cc
)

source_group(
Utils FILES
${UTIL_SRCS}
)

source_group(
	Factory FILES
	ReflectFactory.h
	ReflectFactory.cpp
)

set(CONCURR_SRCS
    TaskWrapper.h
    TaskWrapper.cpp
    Thread/ThreadPool.h 
    Thread/ThreadPool.cpp
    Thread/ThreadCommon.h
    Thread/Thread.h
    Thread/Thread.cpp
	Thread/ConditionVariable.h
	Thread/ConditionVariable.cpp
    Dispatch/Dispatcher.h
    Dispatch/WorkGroup.cpp
    Dispatch/WorkGroup.h
    Dispatch/WorkItem.cpp
    Dispatch/WorkItem.h
    Dispatch/WorkQueue.cpp
    Dispatch/WorkQueue.h
)

if(WIN32 OR LINUX)
set(CONCURR_SRCS 
${CONCURR_SRCS}
)
endif()

source_group(
Concurrent FILES
${CONCURR_SRCS}
)

set(CORE_SRCS
${ASSET_SRCS}
${CONCURR_SRCS}
${UTIL_SRCS}
)

set(IO_SRCS
File.h
File.cpp
Archive.h
Archive.cpp
)

source_group(
IO FILES
${IO_SRCS}
)

set(MSG_SRCS
Message.h
InputDevice.h
InputDevice.cpp
)

source_group(
Message FILES
${MSG_SRCS}
)

set(COMMON_SRCS
ModuleCore.h
ModuleCore.cpp
Timer.h
Timer.cpp
Variant.h)

source_group(
Common FILES
${COMMON_SRCS}
)

set(APP_SRCS
Window.h
Looper.h
Looper.cpp
App.h
App.cpp
)

source_group(
Application FILES
${APP_SRCS}
)

####################Platform Specified###################
if(WIN32 OR LINUX)
set(CORE_SRCS ${CORE_SRCS} ${APP_SRCS} ${MSG_SRCS} ${IO_SRCS} ${COMMON_SRCS})
endif()

if(WIN32)
file(GLOB WINDOWS_IMPL_SRCS "../Platform/Windows/*.cpp" "../Platform/Windows/*.h")
source_group(
Windows FILES
${WINDOWS_IMPL_SRCS}
)
set(CORE_SRCS ${CORE_SRCS} ${WINDOWS_IMPL_SRCS})
endif()

if(IOS)

file(GLOB IOS_IMPL_SRCS "iOS/*.h" "iOS/*.mm")
source_group(
iOS FILES
${IOS_IMPL_SRCS}
)
set(CORE_SRCS ${CORE_SRCS} ${IOS_IMPL_SRCS} ${IO_SRCS})
endif()


add_library(Core ${LIB_TYPE} 
	${CORE_SRCS}
	${IOS_IMPL_SRCS}
)

add_precompiled_header(Core "Kaleido3D.h" "${Kaleido3D_ROOT_DIR}/Include" "${Kaleido3D_ROOT_DIR}/Source/Core/App.cpp")
set_target_properties(Core PROPERTIES FOLDER "Runtime")

install(TARGETS Core ARCHIVE DESTINATION lib)