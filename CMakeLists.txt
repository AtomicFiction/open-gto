CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

include(ExternalProject)

set(make_args VERBOSE=1)

ExternalProject_add(
    gto_build
    PREFIX gto
    SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}
    CONFIGURE_COMMAND ./bootstrap && ./configure --prefix=${CMAKE_INSTALL_PREFIX}  --with-pic --enable-static
    INSTALL_COMMAND make install ${make_args}
    BUILD_IN_SOURCE 1
    BUILD_COMMAND make ${make_args}
)

ExternalProject_Get_Property(gto_build source_dir)
set(PYTHON_SRC_DIR ${source_dir}/plugins/python)

install(
    CODE
    "execute_process(
        WORKING_DIRECTORY ${PYTHON_SRC_DIR}
        COMMAND python ./setup.py install --prefix=${CMAKE_INSTALL_PREFIX}/python
    )"
)