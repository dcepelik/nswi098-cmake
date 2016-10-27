#
# CMakeLists.txt:
# CMake configuration for the evaluation framework of NSWI098 Compiler Principles
#
# NOTE: This was written for assignment Lexical Analysis I. It probably won't
#       work with future assignments. Clone github.com/dcepelik/nswi098-cmake
#       and keep it updated to get the most recent version of this file.
#
# Written by David Čepelík <david@cepelik.eu> in 2016
#
# If you find any bugs or if you can help to improve this config, please
# do send me an e-mail. All suggestions are appreciated.
# 

PROJECT(mlc)
CMAKE_MINIMUM_REQUIRED(VERSION 3.6)

SET(base_path ${CMAKE_SOURCE_DIR})
SET(public_ro "${base_path}/public-ro")
SET(public_rw "${base_path}/public-rw")
SET(xml "${public_ro}/common/xml")
SET(build "${base_path}/build")
SET(generated "${build}/generated")

INCLUDE_DIRECTORIES(
	public-ro/common/h
	public-ro/mlc/h
	public-ro/mlc/du1/h
	${generated})

ADD_EXECUTABLE(
	mlc
	${generated}/du1l.cpp
	${generated}/dummyg.cpp
	${generated}/gen_aifactory.cpp
	${generated}/gen_ainstr.hpp
	${generated}/gen_icfactory.cpp
	${generated}/gen_icinstr.hpp
	${generated}/gen_icinstrtab.cpp
	${generated}/gen_icopcodes.hpp
	${public_ro}/common/src/abstract_ic.cpp
	${public_ro}/common/src/flat_icblock.cpp
	${public_ro}/common/src/ic_instr.cpp
	${public_ro}/common/src/labeled_icblock.cpp
	${public_ro}/common/src/uid_gen.cpp
	${public_ro}/mlc/du1/src/du1.cpp
	${public_ro}/mlc/src/duerr.cpp
	${public_ro}/mlc/src/dusememptyerrtab.cpp
	${public_ro}/mlc/src/dutables.cpp)

ADD_CUSTOM_COMMAND(
	OUTPUT ${generated}/du1l.cpp
	COMMAND flex -Cf -b -p -p -o ${generated}/du1l.cpp ${public_rw}/SWI098/du1/du1l.lex
	DEPENDS ${public_rw}/SWI098/du1/du1l.lex)

ADD_CUSTOM_COMMAND(
	OUTPUT ${generated}/dummyg.cpp ${generated}/dummyg.hpp ${generated}/stack.hh ${generated}/dummyg.output
	COMMAND bison --report=all -d -o ${generated}/dummyg.cpp ${public_ro}/mlc/src/dummyg.y
	DEPENDS ${public_ro}/mlc/src/dummyg.y)

ADD_CUSTOM_COMMAND(
	OUTPUT ${generated}/gen_ainstr.hpp
	COMMAND xsltproc --output ${generated}/gen_ainstr.hpp ${xml}/genainstrh.xslt ${xml}/icminstr.xml
	DEPENDS ${xml}/genainstrh.xslt ${xml}/icminstr.xml)

ADD_CUSTOM_COMMAND(
	OUTPUT ${generated}/gen_icopcodes.hpp
	COMMAND xsltproc --output ${generated}/gen_icopcodes.hpp ${xml}/genicoph.xslt ${xml}/icminstr.xml
	DEPENDS ${xml}/genicoph.xslt ${xml}/icminstr.xml)

ADD_CUSTOM_COMMAND(
	OUTPUT ${generated}/gen_icinstr.hpp
	COMMAND xsltproc --output ${generated}/gen_icinstr.hpp ${xml}/genicinstrh.xslt ${xml}/icminstr.xml
	DEPENDS ${xml}/genicinstrh.xslt ${xml}/icminstr.xml)

ADD_CUSTOM_COMMAND(
	OUTPUT ${generated}/gen_aifactory.cpp
	COMMAND xsltproc --output ${generated}/gen_aifactory.cpp ${xml}/genaifctrc.xslt ${xml}/icminstr.xml
	DEPENDS ${xml}/genaifctrc.xslt ${xml}/icminstr.xml)

ADD_CUSTOM_COMMAND(
	OUTPUT ${generated}/gen_icfactory.cpp
	COMMAND xsltproc --output ${generated}/gen_icfactory.cpp ${xml}/genicfctrc.xslt ${xml}/icminstr.xml
	DEPENDS ${xml}/genicfctrc.xslt ${xml}/icminstr.xml)

ADD_CUSTOM_COMMAND(
	OUTPUT ${generated}/gen_icinstrtab.cpp
	COMMAND xsltproc --output ${generated}/gen_icinstrtab.cpp ${xml}/genictabc.xslt ${xml}/icminstr.xml
	DEPENDS ${xml}/genictabc.xslt ${xml}/icminstr.xml)
