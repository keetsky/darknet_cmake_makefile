cmake_minimum_required(VERSION 2.8.12)
project(darknet)

FIND_PACKAGE(Threads)
find_package(CUDA REQUIRED)
find_package(OpenCV REQUIRED)
find_package( cv_bridge
)



###############################################################################################################
#set(
#    CUDA_NVCC_FLAGS
#    ${CUDA_NVCC_FLAGS};
#    -O3 -gencode arch=compute_50,code=sm_50
#    )
FIND_PACKAGE(CUDA)
if (CUDA_FOUND)
    message(STATUS "CUDA Version: " ${CUDA_VERSION_STRINGS})
    message(STATUS "CUDA Libararies: " ${CUDA_LIBRARIES})
    include_directories(SYSTEM ${CUDA_INCLUDE_DIRS})
    list(APPEND LIBRARIES ${CUDA_LIBRARIES} ${CUDA_CUBLAS_LIBRARIES} ${CUDA_curand_LIBRARY} ${CUDA_cusparse_LIBRARY})
    list(APPEND CUDA_NVCC_FLAGS "-std=c++11;-O2;-Xcompiler \"-fPIC\" ")
    list(APPEND CUDA_NVCC_FLAGS "-gencode arch=compute_20,code=compute_20 ")
    list(APPEND CUDA_NVCC_FLAGS "-gencode arch=compute_30,code=compute_30 ")
    list(APPEND CUDA_NVCC_FLAGS "-gencode arch=compute_35,code=compute_35 ")
    list(APPEND CUDA_NVCC_FLAGS "-gencode arch=compute_50,code=compute_50 ")
    list(APPEND CUDA_NVCC_FLAGS "-gencode arch=compute_52,code=compute_52 ")
    set(CUDA_PROPAGATE_HOST_FLAGS OFF)
    add_definitions(-DGPU)
    cuda_include_directories(src)
else()
    list(APPEND LIBRARIES "m")
endif()
################################################################################################################

add_definitions(-DGPU -DOPENCV)



include_directories(
  /usr/local/cuda/include
  src
  ${OpenCV_INCLUDE_DIRS}
)

link_directories(
  /usr/local/cuda/lib
/usr/local/cuda/lib64
)
link_libraries( 
	-lstdc++ 
	-lcuda 
	-lcudart 
	-lcublas 
	-lcurand
	${OpenCV_LIBS}
		)
#include_directories(src)
#aux_source_directory(./src SRC_DIR)
#set(SRC_DIR src)
#cuda_include_directories(src)
#FILE(GLOB SRC_FILES ${SRC_DIR}/*.c)
#FILE(GLOB CU_SRC_FILES ${SRC_DIR}/*.cu)
#cuda_add_library(dnl SHARED ${SRC_FILES} ${CU_SRC_FILES})

	
#cuda_add_executable(darknet  ${SRC_DIR})
cuda_add_executable(darknet  
	src/gemm.c src/utils.c src/cuda.c src/convolutional_layer.c src/list.c src/image.c src/activations.c src/im2col.c src/col2im.c src/blas.c src/crop_layer.c src/dropout_layer.c src/maxpool_layer.c src/softmax_layer.c src/data.c src/matrix.c src/network.c src/connected_layer.c src/cost_layer.c src/parser.c src/option_list.c src/darknet.c src/detection_layer.c src/captcha.c src/route_layer.c src/writing.c src/box.c src/nightmare.c src/normalization_layer.c src/avgpool_layer.c src/coco.c src/dice.c src/yolo.c src/detector.c src/layer.c src/compare.c src/classifier.c src/local_layer.c src/swag.c src/shortcut_layer.c src/activation_layer.c src/rnn_layer.c src/gru_layer.c src/rnn.c src/rnn_vid.c src/crnn_layer.c src/demo.c src/tag.c src/cifar.c src/go.c src/batchnorm_layer.c src/art.c src/region_layer.c src/reorg_layer.c src/super.c src/voxel.c src/tree.c
#GPU
src/convolutional_kernels.cu src/activation_kernels.cu src/im2col_kernels.cu src/col2im_kernels.cu src/blas_kernels.cu src/crop_layer_kernels.cu src/dropout_layer_kernels.cu src/maxpool_layer_kernels.cu src/network_kernels.cu src/avgpool_layer_kernels.cu	
	)



target_link_libraries(darknet
   m
   pthread
   stdc++
   cuda 
   cudart 
   cublas 
   curand
   ${OpenCV_LIBS}
#   ${CUDA_LIBRARIES} ${CUDA_CUBLAS_LIBRARIES} ${CUDA_curand_LIBRARY} ${CUDA_cusparse_LIBRARY} 
 #  dnl
)


