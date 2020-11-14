#
# Copyright 2020 Staysail Systems, Inc. <info@staystail.tech>
#
# This software is supplied under the terms of the MIT License, a
# copy of which should be located in the distribution where this
# file was obtained (LICENSE.txt).  A copy of the license may also be
# found online at https://opensource.org/licenses/MIT.
#

#  POSIX.

# We cannot use nng_sources_if because these tests don't go into
# the static library unless they also go into the dynamic.
if (NNG_PLATFORM_POSIX)

    find_package(Threads REQUIRED)
    nng_link_libraries(Threads::Threads)

    nng_sources(
            posix_impl.h
            posix_aio.h
            posix_ipc.h
            posix_config.h
            posix_pollq.h
            posix_tcp.h

            posix_alloc.c
            posix_atomic.c
            posix_clock.c
            posix_debug.c
            posix_file.c
            posix_ipcconn.c
            posix_ipcdial.c
            posix_ipclisten.c
            posix_pipe.c
            posix_resolv_gai.c
            posix_sockaddr.c
            posix_tcpconn.c
            posix_tcpdial.c
            posix_tcplisten.c
            posix_thread.c
            posix_udp.c
    )

    if (NNG_HAVE_PORT_CREATE)
        nng_sources(posix_pollq_port.c)
    elseif (NNG_HAVE_KQUEUE)
        nng_sources(posix_pollq_kqueue.c)
    elseif (NNG_HAVE_EPOLL AND NNG_HAVE_EVENTFD)
        nng_sources(posix_pollq_epoll.c)
    else ()
        nng_sources(posix_pollq_poll.c)
    endif ()

    if (NNG_HAVE_ARC4RANDOM)
        nng_sources(posix_rand_arc4random.c)
    elseif (NNG_HAVE_GETRANDOM)
        nng_sources(posix_rand_getrandom.c)
    else ()
        nng_sources(posix_rand_urandom.c)
    endif ()
endif ()