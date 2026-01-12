# Generated from gamepad.pro.

#####################################################################
## Gamepad Module:
#####################################################################

qt_internal_add_module(GamepadLegacy
    NO_GENERATE_METATYPES
    PLUGIN_TYPES legacygamepads
    SOURCES
        qgamepad.cpp qgamepad.h
        qgamepadbackend.cpp qgamepadbackend_p.h
        qgamepadbackendfactory.cpp qgamepadbackendfactory_p.h
        qgamepadbackendplugin.cpp qgamepadbackendplugin_p.h
        qgamepadkeynavigation.cpp qgamepadkeynavigation.h
        qgamepadmanager.cpp qgamepadmanager.h
        qtgamepadglobal.h
    DEFINES
        QT_BUILD_GAMEPAD_LIB
    LIBRARIES
        Qt::CorePrivate
    PUBLIC_LIBRARIES
        Qt::Core
        Qt::Gui
    PRIVATE_MODULE_INTERFACE
        Qt::CorePrivate
)

# When building this module as a vendored subproject, Qt's header syncing (syncqt)
# infrastructure isn't available, but some sources include headers via the
# installed-style prefix (e.g. <QtGamepadLegacy/qgamepad.h>).
# Provide a minimal generated include tree in the build dir to satisfy those includes.
set(_qtgamepad_include_root "${CMAKE_CURRENT_BINARY_DIR}/include")
set(_qtgamepad_include_dir "${_qtgamepad_include_root}/QtGamepadLegacy")
file(MAKE_DIRECTORY "${_qtgamepad_include_dir}")
set(QTGAMEPADLEGACY_INCLUDE_ROOT "${_qtgamepad_include_root}" CACHE INTERNAL
    "Build-time include root that provides QtGamepadLegacy/<...> headers for vendored qtgamepad")
foreach(_hdr IN ITEMS
    qgamepad.h
    qgamepadkeynavigation.h
    qgamepadmanager.h
    qtgamepadglobal.h
)
    configure_file("${CMAKE_CURRENT_SOURCE_DIR}/${_hdr}"
                   "${_qtgamepad_include_dir}/${_hdr}"
                   COPYONLY)
endforeach()

# Provide the Qt-style CamelCase forwarding headers expected by the sources.
file(WRITE "${_qtgamepad_include_dir}/QGamepadManager"
"#pragma once\n#include <QtGamepadLegacy/qgamepadmanager.h>\n")
file(WRITE "${_qtgamepad_include_dir}/QGamepadKeyNavigation"
"#pragma once\n#include <QtGamepadLegacy/qgamepadkeynavigation.h>\n")
file(WRITE "${_qtgamepad_include_dir}/QGamepad"
"#pragma once\n#include <QtGamepadLegacy/qgamepad.h>\n")

# Provide the header the QML type registration generator looks for:
#   __has_include(<private/qtgamepadquicktypes_p.h>)
file(MAKE_DIRECTORY "${_qtgamepad_include_root}/private")
configure_file("${CMAKE_CURRENT_SOURCE_DIR}/../imports/gamepad/qtgamepadquicktypes_p.h"
               "${_qtgamepad_include_root}/private/qtgamepadquicktypes_p.h"
               COPYONLY)

target_include_directories(GamepadLegacy
    PUBLIC
        "$<BUILD_INTERFACE:${_qtgamepad_include_root}>"
)

# When building this legacy module as a subproject, Qt's internal plugin helpers
# expect the module target to exist in the export namespace (e.g. Qt6::GamepadLegacy).
# Depending on how the repo is embedded, the module might be available as
# GamepadLegacy and/or Qt::GamepadLegacy. Create alias targets to satisfy
# QtPluginHelpers' lookup.
set(_qtgamepad_export_ns "${QT_CMAKE_EXPORT_NAMESPACE}")
if(_qtgamepad_export_ns STREQUAL "")
    # Qt6's default export namespace.
    set(_qtgamepad_export_ns "Qt6")
endif()

foreach(_qtgamepad_target IN ITEMS GamepadLegacy GamepadLegacyPrivate)
    if(NOT TARGET "${_qtgamepad_export_ns}::${_qtgamepad_target}")
        if(TARGET "Qt::${_qtgamepad_target}")
            add_library("${_qtgamepad_export_ns}::${_qtgamepad_target}" ALIAS "Qt::${_qtgamepad_target}")
        elseif(TARGET "${_qtgamepad_target}")
            add_library("${_qtgamepad_export_ns}::${_qtgamepad_target}" ALIAS "${_qtgamepad_target}")
        endif()
    endif()
endforeach()

if(ANDROID)
    set_property(TARGET GamepadLegacy APPEND PROPERTY QT_ANDROID_BUNDLED_JAR_DEPENDENCIES
        jar/QtAndroidGamepad.jar
    )
    set_property(TARGET GamepadLegacy APPEND PROPERTY QT_ANDROID_LIB_DEPENDENCIES
        plugins/legacygamepads/libplugins_gamepads_androidgamepad.so
    )
endif()

#### Keys ignored in scope 12:.:.:gamepad.pro:<TRUE>:
# ANDROID_FEATURES = "android.hardware.gamepad"
qt_internal_add_docs(GamepadLegacy
    doc/qtgamepad.qdocconf
)

