

set(CC_UNZIP_SOURCES
  ${CMAKE_CURRENT_LIST_DIR}/unzip/crypt.h
  ${CMAKE_CURRENT_LIST_DIR}/unzip/ioapi.cpp
  ${CMAKE_CURRENT_LIST_DIR}/unzip/ioapi.h
  ${CMAKE_CURRENT_LIST_DIR}/unzip/ioapi_mem.cpp
  ${CMAKE_CURRENT_LIST_DIR}/unzip/ioapi_mem.h
  ${CMAKE_CURRENT_LIST_DIR}/unzip/unzip.cpp
  ${CMAKE_CURRENT_LIST_DIR}/unzip/unzip.h
)

set(CC_TINYDIR_SOURCES
  ${CMAKE_CURRENT_LIST_DIR}/tinyxml2/tinyxml2.cpp
  ${CMAKE_CURRENT_LIST_DIR}/tinyxml2/tinyxml2.h
  ${CMAKE_CURRENT_LIST_DIR}/tinydir/tinydir.h
)

set(CC_UTILS_SOURCES
  ${CMAKE_CURRENT_LIST_DIR}/ConvertUTF/ConvertUTF.c
  ${CMAKE_CURRENT_LIST_DIR}/ConvertUTF/ConvertUTF.h
  ${CMAKE_CURRENT_LIST_DIR}/ConvertUTF/ConvertUTFWrapper.cpp
  ${CMAKE_CURRENT_LIST_DIR}/xxtea/xxtea.cpp
  ${CMAKE_CURRENT_LIST_DIR}/xxtea/xxtea.h
)

set(CC_EXTERNAL_SOURCES

  ${CMAKE_CURRENT_LIST_DIR}/tommyds/tommy.c
  ${CMAKE_CURRENT_LIST_DIR}/tommyds/tommy.h
)

set (CC_MESHOPT_SOURCES
  ${CMAKE_CURRENT_LIST_DIR}/meshopt/meshoptimizer.h
  ${CMAKE_CURRENT_LIST_DIR}/meshopt/vertexcodec.cpp
  ${CMAKE_CURRENT_LIST_DIR}/meshopt/indexcodec.cpp
)

if(NOT USE_MODULES)
  list(APPEND CC_EXTERNAL_SOURCES
    ${CC_UNZIP_SOURCES}
    ${CC_TINYDIR_SOURCES}
    ${CC_UTILS_SOURCES}
    ${CC_MESHOPT_SOURCES}
  )
endif()

# add dependent boost libs
if((NOT NX) AND (NOT EMSCRIPTEN))
  include(${CMAKE_CURRENT_LIST_DIR}/boost-source/boost.cmake)
endif()

if(ANDROID AND CC_ENABLE_SWAPPY)

include(${CMAKE_CURRENT_LIST_DIR}/Swappy/src/swappy/CMakeLists.txt)
list(APPEND CC_EXTERNAL_LIBS
  swappy
)
add_definitions(-DCC_SWAPPY_ENABLED)

endif()


if(ANDROID AND (NOT SURFACE_LESS_SERVICE))
    set(CC_GAME_ACTIVITY_SOURCES
        ${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include/game-activity/GameActivity.cpp
        ${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include/game-activity/system_utils.cpp
        ${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include/game-activity/native_app_glue/android_native_app_glue.c
        ${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include/game-text-input/gametextinput.cpp
    )

    include(${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameController/CMakeLists.txt)
    list(APPEND CC_EXTERNAL_SOURCES
        ${CC_GAME_ACTIVITY_SOURCES}
    )
    include_directories(${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include)
endif()

if(ANDROID OR OHOS OR NX)

  include(${CMAKE_CURRENT_LIST_DIR}/pvmp3dec/CMakeLists.txt)
  include(${CMAKE_CURRENT_LIST_DIR}/tremolo/CMakeLists.txt)

  list(APPEND CC_EXTERNAL_LIBS
    pvmp3dec
    vorbisidec
  )

elseif(WINDOWS)
  set_source_files_properties(${CMAKE_CURRENT_LIST_DIR}/ConvertUTF/ConvertUTF.c PROPERTIES
    LANGUAGE C
  )
elseif(APPLE)
  if(USE_SOCKET)
      include(${CMAKE_CURRENT_LIST_DIR}/SocketRocket/CMakeLists.txt)
  endif()
elseif(NX_WINDOWS)
  include(${CMAKE_CURRENT_LIST_DIR}/pvmp3dec/CMakeLists.txt)
  list(APPEND CC_EXTERNAL_LIBS
    pvmp3dec
  )
endif()

if((NOT NX_WINDOWS) AND (NOT NX))
  list(APPEND CC_EXTERNAL_INCLUDES
  ${CMAKE_CURRENT_LIST_DIR}
  )
endif()

if(USE_PHYSICS_PHYSX)
  list(APPEND CC_EXTERNAL_INCLUDES
    ${CMAKE_CURRENT_LIST_DIR}/PhysX/PhysX/include
    ${CMAKE_CURRENT_LIST_DIR}/PhysX/PhysX/source
    ${CMAKE_CURRENT_LIST_DIR}/PhysX/PxShared/include
  )
endif()

# box2d
if(USE_BOX2D_JSB)
  set(BOX2D_SOURCE_FILES
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_broad_phase.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_chain_shape.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_circle_shape.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_collide_circle.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_collide_edge.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_collide_polygon.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_collision.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_distance.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_dynamic_tree.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_edge_shape.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_polygon_shape.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/collision/b2_time_of_impact.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/common/b2_block_allocator.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/common/b2_draw.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/common/b2_math.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/common/b2_settings.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/common/b2_stack_allocator.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/common/b2_timer.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_body.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_chain_circle_contact.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_chain_circle_contact.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_chain_polygon_contact.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_chain_polygon_contact.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_circle_contact.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_circle_contact.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_contact.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_contact_manager.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_contact_solver.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_contact_solver.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_distance_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_rope_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_edge_circle_contact.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_edge_circle_contact.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_edge_polygon_contact.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_edge_polygon_contact.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_fixture.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_friction_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_gear_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_island.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_island.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_motor_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_mouse_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_polygon_circle_contact.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_polygon_circle_contact.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_polygon_contact.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_polygon_contact.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_prismatic_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_pulley_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_revolute_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_weld_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_wheel_joint.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_world.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/dynamics/b2_world_callbacks.cpp
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src/rope/b2_rope.cpp
  )

  set(BOX2D_HEADER_FILES
    # ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_api.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_block_allocator.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_body.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_broad_phase.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_chain_shape.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_circle_shape.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_collision.h
    # ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_common.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_contact.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_contact_manager.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_distance.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_distance_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_rope_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_draw.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_dynamic_tree.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_edge_shape.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_fixture.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_friction_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_gear_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_growable_stack.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_math.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_motor_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_mouse_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_polygon_shape.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_prismatic_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_pulley_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_revolute_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_rope.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_settings.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_shape.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_stack_allocator.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_time_of_impact.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_timer.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_time_step.h
    # ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_types.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_weld_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_wheel_joint.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_world.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/b2_world_callbacks.h
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include/box2d/box2d.h
  )


  list(APPEND CC_EXTERNAL_SOURCES
    ${BOX2D_HEADER_FILES}
    ${BOX2D_SOURCE_FILES}
  )

  list(APPEND CC_EXTERNAL_INCLUDES
    ${CMAKE_CURRENT_LIST_DIR}/box2d/include
    ${CMAKE_CURRENT_LIST_DIR}/box2d/src
  )
endif()
