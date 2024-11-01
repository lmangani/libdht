include(GNUInstallDirs)

# Set the directory for public headers relative to the current directory
set(PUBLIC_HEADERS_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../include)

# List of public headers
set(PUBLIC_HEADERS ${PUBLIC_HEADERS_DIR}/dht/bencode.h
                   ${PUBLIC_HEADERS_DIR}/dht/node.h
                   ${PUBLIC_HEADERS_DIR}/dht/peers.h
                   ${PUBLIC_HEADERS_DIR}/dht/put.h
                   ${PUBLIC_HEADERS_DIR}/dht/utils.h)

# Define the dht library and its source files
add_library(dht STATIC
    bencode.c
    crc32c.c
    hexdump.c
    hmac.c
    ip_counter.c
    node.c
    peers.c
    put.c
    random.c
    sha1.c
    time.c
    utils.c
    ed25519/add_scalar.c
    ed25519/fe.c
    ed25519/ge.c
    ed25519/key_exchange.c
    ed25519/keypair.c
    ed25519/sc.c
    ed25519/seed.c
    ed25519/sha512.c
    ed25519/sign.c
    ed25519/verify.c
    ${PUBLIC_HEADERS}
)

# Include the headers for the dht library
target_include_directories(dht PUBLIC ${PUBLIC_HEADERS_DIR})

# Set compiler options
if(CMAKE_COMPILER_IS_GNUCC)
    target_compile_options(dht PRIVATE -W -Wall)
endif()

# Compile definitions
target_compile_definitions(dht PRIVATE ED25519_NO_SEED)
target_compile_definitions(dht PRIVATE "$<$<CONFIG:DEBUG>:DHT_DEBUG>")

# Installation rules
install(TARGETS dht
        RUNTIME DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(FILES ${PUBLIC_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/dht)
