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
    ${SQL_DIR}/regexp.c
    ${SQL_DIR}/sqlite3secure.c
	)

target_sources(sqlitecipher PRIVATE ${HEADERS} ${SOURCES} ${DEFFILES})