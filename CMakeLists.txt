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

# Use external jedi-cmake or build in bundle
ecbuild_bundle( PROJECT jedicmake GIT "https://github.com/jcsda-internal/jedi-cmake.git" BRANCH develop UPDATE RECURSIVE )
include( jedicmake/cmake/Functions/git_functions.cmake )

# ECMWF toolboxes
ecbuild_bundle( PROJECT eckit    GIT "https://github.com/ecmwf/eckit.git" TAG 1.20.2 )
ecbuild_bundle( PROJECT fckit    GIT "https://github.com/ecmwf/fckit.git" TAG 0.10.0 )
ecbuild_bundle( PROJECT fiat     GIT "https://github.com/ecmwf-ifs/fiat.git" BRANCH main )
ecbuild_bundle( PROJECT ectrans  GIT "https://github.com/ecmwf-ifs/ectrans.git" BRANCH main )
ecbuild_bundle( PROJECT atlas    GIT "https://github.com/ecmwf/atlas.git" TAG 0.32.1 )

# OOPS
#ecbuild_bundle( PROJECT oops    GIT "https://git.ecmwf.int/scm/oops/oops" BRANCH feature/develop-IFS )
ecbuild_bundle( PROJECT oops    GIT "https://git.ecmwf.int/scm/~sbbp/oops" BRANCH feature/saber_interface )

# OOPS-JEDI
ecbuild_bundle( PROJECT oopsjedi GIT "https://github.com/benjaminmenetrier/oopsjedi" BRANCH develop )

# JCSDA background error covariance libraries
ecbuild_bundle( PROJECT gsibec   GIT "https://github.com/geos-esm/GSIbec" TAG 1.1.3 UPDATE )
#ecbuild_bundle( PROJECT vader    GIT "https://github.com/jcsda/vader.git" BRANCH develop UPDATE ) # For all users
ecbuild_bundle( PROJECT vader    GIT "https://github.com/jcsda-internal/vader.git" BRANCH feature/use_oops_ecmwf UPDATE ) # For JEDI users only
#ecbuild_bundle( PROJECT saber    GIT "https://github.com/jcsda/saber.git" BRANCH develop UPDATE ) # For all users
ecbuild_bundle( PROJECT saber    GIT "https://github.com/jcsda-internal/saber.git" BRANCH feature/use_oops_ecmwf_2 UPDATE ) # For JEDI users only

ecbuild_bundle_finalize()
