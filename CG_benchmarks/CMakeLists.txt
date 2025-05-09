find_package(TBB CONFIG REQUIRED COMPONENTS tbbmalloc_proxy)
find_package(OpenVDB CONFIG REQUIRED)
find_package(OpenMP REQUIRED)
add_executable(CG_benchmarks  main.cpp pcgsolver/blas_wrapper.h pcgsolver/pcg_solver.h pcgsolver/sparse_matrix.h npyio/vdb_npy_io.h) 

target_link_libraries(CG_benchmarks PRIVATE FLIP_vdb OpenVDB::openvdb TBB::tbbmalloc_proxy SIMD_UAAMG singletons OpenMP::OpenMP_CXX)
target_include_directories(CG_benchmarks PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_definitions(CG_benchmarks PRIVATE AMGCL_NO_BOOST)

if (WIN32)
target_compile_options(CG_benchmarks PRIVATE "/arch:AVX2" "/bigobj")
else()
target_compile_options(CG_benchmarks PRIVATE "-march=native")
endif()

install(TARGETS CG_benchmarks DESTINATION "CG_benchmarks")
install(FILES "scene/config.yaml" DESTINATION "CG_benchmarks")