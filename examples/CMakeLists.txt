add_subdirectory(pingserver)
add_subdirectory(redis_dict)

add_executable(raw_echo_server raw_echo_server.cc)
cxx_link(raw_echo_server base fibers2 TRDP::uring TRDP::gperf)

add_executable(echo_server echo_server.cc)
if (USE_FB2)
    cxx_link(echo_server base fibers2 http_server_lib TRDP::gperf)
else()
    cxx_link(echo_server base uring_fiber_lib epoll_fiber_lib http_server_lib)
endif()


# add_executable(proactor_stress proactor_stress.cc)
# cxx_link(proactor_stress base uring_fiber_lib http_server_lib)

add_executable(s3_demo s3_demo.cc)
if (USE_FB2)
cxx_link(s3_demo aws_lib)
else()
cxx_link(s3_demo uring_fiber_lib aws_lib)
endif()

add_executable(https_client_cli https_client_cli.cc)
cxx_link(https_client_cli base uring_fiber_lib http_client_lib tls_lib)
