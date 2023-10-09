cmake_minimum_required(VERSION 3.17)

set(CMAKE_CXX_STANDARD 17)
project(fmatpp)

# ------
# Kokkos
# ------
# set(Kokkos_ENABLE_CUDA ON)
set(Kokkos_ENABLE_OPENMP ON)
add_subdirectory(external/kokkos)

# --------------------------
# Format (advanced printing)
# --------------------------
add_subdirectory(external/fmt)

# -------
# DocTest
# -------
add_subdirectory(external/doctest)

# ---
# MPI
# ---
find_package(MPI)

# --------
# HighFive
# --------
set(HIGHFIVE_PARALLEL_HDF5 ON)
set(HIGHFIVE_USE_BOOST OFF)
add_subdirectory(external/HighFive)

# -------------
# Documentation
# -------------
find_package(Doxygen)
option(BUILD_DOCUMENTATION "Documentation (requires Doxygen)" ${DOXYGEN_FOUND})

if(BUILD_DOCUMENTATION)
    doxygen_add_docs(
        ${PROJECT_NAME}_doc
        ${CMAKE_SOURCE_DIR}/src
        COMMENT "Generate documentation"
    )
endif()


add_library(${PROJECT_NAME} ${source_list})
target_link_libraries(${PROJECT_NAME} kokkos HighFive fmt::fmt)
target_include_directories(${PROJECT_NAME} PUBLIC kokkos HighFive MPI::MPI_CXX fmt::fmt)