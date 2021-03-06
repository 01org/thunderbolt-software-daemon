set(FILE_NAME tbtadm)
set(T2T_SOURCE "${FILE_NAME}.t2t")
set(MAN_OUTPUT "${FILE_NAME}.1.gz")

find_program(TXT2TAGS txt2tags)
if(${TXT2TAGS} STREQUAL "TXT2TAGS-NOTFOUND")
    message(FATAL_ERROR "txt2tags is needed for generating the man page")
endif()

add_custom_command(OUTPUT ${MAN_OUTPUT}
                   COMMAND txt2tags -t man -i "${CMAKE_CURRENT_SOURCE_DIR}/${T2T_SOURCE}" -o ${MAN_OUTPUT}.tmp
                   COMMAND gzip -9nc ${MAN_OUTPUT}.tmp > ${MAN_OUTPUT}
                   WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
                   MAIN_DEPENDENCY ${T2T_SOURCE}
                   COMMENT "Generating man page...")
add_custom_target(manpage ALL DEPENDS ${MAN_OUTPUT})

install(FILES        "${CMAKE_CURRENT_BINARY_DIR}/${MAN_OUTPUT}"
        DESTINATION   ${CMAKE_INSTALL_MANDIR}/man1)
