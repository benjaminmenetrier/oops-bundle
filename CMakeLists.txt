cmake_minimum_required( VERSION 3.12 FATAL_ERROR )

find_package( ecbuild 3.6 REQUIRED HINTS ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/../ecbuild)

project( oops-bundle VERSION 0.0.1 LANGUAGES C CXX Fortran )

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include( ecbuild_bundle )

# Default release mode
set( ECBUILD_DEFAULT_BUILD_TYPE Release )

# Enable OpenMP and MPI
set( ENABLE_MPI ON CACHE BOOL "Compile with MPI" )
set( ENABLE_OMP ON CACHE BOOL "Compile with OpenMP" )

# Define bundle
ecbuild_bundle_initialize()

# add the automatically determined parts of the RPATH
# which point to directories outside the build tree to the install RPATH
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH ON)

# when building, already use the install RPATH
set(CMAKE_BUILD_WITH_INSTALL_RPATH ON)

# ECMWF tools
ecbuild_bundle( PROJECT eckit    GIT "https://github.com/ecmwf/eckit.git" TAG 1.23.0 )
ecbuild_bundle( PROJECT fckit    GIT "https://github.com/ecmwf/fckit.git" TAG 0.10.1 )
ecbuild_bundle( PROJECT fiat     GIT "https://github.com/ecmwf-ifs/fiat.git" BRANCH main )
ecbuild_bundle( PROJECT ectrans  GIT "https://github.com/ecmwf-ifs/ectrans.git" BRANCH main )
ecbuild_bundle( PROJECT atlas    GIT "https://github.com/ecmwf/atlas.git" TAG 0.33.0 )

# OOPS
ecbuild_bundle( PROJECT oops )

# ECSABER
ecbuild_bundle( PROJECT ecsaber  GIT "https://github.com/benjaminmenetrier/ecsaber.git" BRANCH develop UPDATE RECURSIVE )
ecbuild_bundle( PROJECT ecsaber/oops-patch )
ecbuild_bundle( PROJECT ecsaber/vader )
ecbuild_bundle( PROJECT ecsaber/saber )

ecbuild_bundle_finalize()
