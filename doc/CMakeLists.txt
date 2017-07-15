# Copyright (c) 2017, Adriaan de Groot <groot@kde.org>
#
# All rights reserved. See file LICENSE for exact terms (2-clause BSD license).
#
# Adriaan de Groot <groot@kde.org>

cmake_minimum_required(VERSION 3.1 FATAL_ERROR)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake)

project(idhub-service)

include(MacroAddUninstallTarget)
include(MacroCreateConfigFiles)
include(MacroEnsureOutOfSourceBuild)
include(MacroGitVersionInfo)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

macro_ensure_out_of_source_build(
  "Do not build Idhub-service in the source directory.")

get_version_from_git(idhub-service 0.0.0)

enable_testing()
add_subdirectory(src)
add_uninstall_target()

#
# PACKAGING
#

set(CPACK_PACKAGE_NAME "Idhub-service")
set(CPACK_PACKAGE_VERSION ${idhub-service_VERSION})
set(CPACK_PACKAGE_VENDOR "ARPA2.net")
set(CPACK_PACKAGE_CONTACT "Adriaan de Groot <groot@kde.org>")
set(CPACK_FREEBSD_PACKAGE_LICENSE "BSD-2-Clause")

include(PackAllPossible)
include(CPack)

create_config_files(Idhub-service)
