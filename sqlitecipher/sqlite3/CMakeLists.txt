set(HEADERS
    ${SQL_DIR}/codec.h
    ${SQL_DIR}/fastpbkdf2.h
    ${SQL_DIR}/rijndael.h
    ${SQL_DIR}/sha1.h
    ${SQL_DIR}/sha2.h
    ${SQL_DIR}/sqlite3.h
    ${SQL_DIR}/sqlite3ext.h
    ${SQL_DIR}/sqlite3secure.h
    ${SQL_DIR}/sqlite3userauth.h
    ${SQL_DIR}/test_windirent.h
	)

set(SOURCES
    ${SQL_DIR}/carray.c
    ${SQL_DIR}/chacha20poly1305.c
    ${SQL_DIR}/codec.c
    ${SQL_DIR}/codecext.c
    ${SQL_DIR}/csv.c
    ${SQL_DIR}/extensionfunctions.c
    ${SQL_DIR}/fastpbkdf2.c
    ${SQL_DIR}/fileio.c
    ${SQL_DIR}/md5.c
    ${SQL_DIR}/regexp.c
    ${SQL_DIR}/rekeyvacuum.c
    ${SQL_DIR}/rijndael.c
    ${SQL_DIR}/series.c
    ${SQL_DIR}/sha1.c
    ${SQL_DIR}/sha2.c
    ${SQL_DIR}/shathree.c
    ${SQL_DIR}/sqlite3.c
    ${SQL_DIR}/sqlite3secure.c
    ${SQL_DIR}/test_windirent.c
    ${SQL_DIR}/userauth.c
	)

if (WIN32)
    ENABLE_LANGUAGE(RC)
    set(SOURCES ${SQL_DIR}/sqlite3.rc ${SQL_DIR}/sqlite3shell.rc ${SOURCES})
endif()

set ( DEFFILES
    ${SQL_DIR}/sqlite3.def
)

target_sources(sqlitecipher PRIVATE ${HEADERS} ${SOURCES} ${DEFFILES})