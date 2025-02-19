# Cap threadpools to 4 threads.
export MAX_CONCURRENCY=4
AUTOMAKE_OPTION = serial-tests

# unittest: tests that do not need loolwsd running, and that are run during a
# normal build
# test: tests that need loolwsd running, and that are run via 'make check'
check_PROGRAMS = test fakesockettest

noinst_PROGRAMS = test fakesockettest unittest

AM_CXXFLAGS = $(CPPUNIT_CFLAGS) -DTDOC=\"$(abs_top_srcdir)/test/data\" \
	-I${top_srcdir}/common -I${top_srcdir}/net -I${top_srcdir}/wsd -I${top_srcdir}/kit

noinst_LTLIBRARIES = \
	unit-convert.la unit-typing.la unit-copy-paste.la \
	unit-timeout.la unit-prefork.la \
	unit-storage.la unit-client.la \
	unit-admin.la unit-tilecache.la \
	unit-fuzz.la unit-oob.la unit-http.la unit-oauth.la \
	unit-wopi.la unit-wopi-saveas.la \
	unit-wopi-ownertermination.la unit-wopi-versionrestore.la \
	unit-wopi-documentconflict.la unit_wopi_renamefile.la \
	unit-tiff-load.la \
	unit-large-paste.la \
	unit-wopi-loadencoded.la unit-wopi-temp.la

MAGIC_TO_FORCE_SHLIB_CREATION = -rpath /dummy
AM_LDFLAGS = -pthread -module $(MAGIC_TO_FORCE_SHLIB_CREATION) $(ZLIB_LIBS)

if ENABLE_SSL
AM_LDFLAGS += -lssl -lcrypto
endif

# We work around some of the mess of using the same sources both on
# the server side and here in unit tests with conditional compilation
# based on BUILDING_TESTS

AM_CPPFLAGS = -pthread -I$(top_srcdir) -DBUILDING_TESTS

wsd_sources = \
            ../common/FileUtil.cpp \
            ../common/SigUtil.cpp \
            ../common/IoUtil.cpp \
            ../common/Log.cpp \
            ../common/Protocol.cpp \
            ../common/Session.cpp \
            ../common/SpookyV2.cpp \
            ../common/Util.cpp \
            ../common/MessageQueue.cpp \
            ../common/Authorization.cpp \
            ../kit/Kit.cpp \
            ../kit/TestStubs.cpp \
            ../wsd/Auth.cpp \
            ../wsd/TileCache.cpp \
            ../wsd/TestStubs.cpp \
            ../common/Unit.cpp \
            ../net/Socket.cpp

if ENABLE_SSL
wsd_sources += ../net/Ssl.cpp
endif

test_base_source = \
	TileQueueTests.cpp \
	WhiteBoxTests.cpp \
	DeltaTests.cpp \
	$(wsd_sources)

test_all_source = \
	$(test_base_source) \
	TileCacheTests.cpp \
	integration-http-server.cpp \
	httpwstest.cpp \
	httpcrashtest.cpp \
	httpwserror.cpp

unittest_CPPFLAGS = -I$(top_srcdir) -DBUILDING_TESTS
unittest_SOURCES = $(test_base_source) test.cpp
unittest_LDADD = $(CPPUNIT_LIBS)

test_CPPFLAGS = -I$(top_srcdir) -DBUILDING_TESTS
test_SOURCES = $(test_all_source) test.cpp
test_LDADD = $(CPPUNIT_LIBS)

fakesockettest_SOURCES = fakesockettest.cpp ../net/FakeSocket.cpp
fakesockettest_LDADD = $(CPPUNIT_LIBS)

# unit test modules:
unit_oob_la_SOURCES = UnitOOB.cpp
unit_http_la_SOURCES = UnitHTTP.cpp
unit_fuzz_la_SOURCES = UnitFuzz.cpp
unit_admin_la_SOURCES = UnitAdmin.cpp
unit_admin_la_LIBADD = $(CPPUNIT_LIBS)
unit_client_la_SOURCES = UnitClient.cpp ${test_all_source}
unit_client_la_LIBADD = $(CPPUNIT_LIBS)
unit_typing_la_SOURCES = UnitTyping.cpp
unit_typing_la_LIBADD = $(CPPUNIT_LIBS)
unit_copy_paste_la_SOURCES = UnitCopyPaste.cpp
unit_copy_paste_la_LIBADD = $(CPPUNIT_LIBS)
unit_convert_la_SOURCES = UnitConvert.cpp
unit_timeout_la_SOURCES = UnitTimeout.cpp
unit_prefork_la_SOURCES = UnitPrefork.cpp
unit_storage_la_SOURCES = UnitStorage.cpp
unit_tilecache_la_SOURCES = UnitTileCache.cpp
unit_oauth_la_SOURCES = UnitOAuth.cpp
unit_oauth_la_LIBADD = $(CPPUNIT_LIBS)
unit_wopi_la_SOURCES = UnitWOPI.cpp
unit_wopi_la_LIBADD = $(CPPUNIT_LIBS)
unit_wopi_saveas_la_SOURCES = UnitWOPISaveAs.cpp
unit_wopi_saveas_la_LIBADD = $(CPPUNIT_LIBS)
unit_wopi_ownertermination_la_SOURCES = UnitWopiOwnertermination.cpp
unit_wopi_ownertermination_la_LIBADD = $(CPPUNIT_LIBS)
unit_wopi_versionrestore_la_SOURCES = UnitWOPIVersionRestore.cpp
unit_wopi_versionrestore_la_LIBADD = $(CPPUNIT_LIBS)
unit_wopi_documentconflict_la_SOURCES = UnitWOPIDocumentConflict.cpp
unit_wopi_documentconflict_la_LIBADD = $(CPPUNIT_LIBS)
unit_wopi_renamefile_la_SOURCES = UnitWOPIRenameFile.cpp
unit_wopi_renamefile_la_LIBADD = $(CPPUNIT_LIBS)
unit_wopi_loadencoded_la_SOURCES = UnitWOPILoadEncoded.cpp
unit_wopi_loadencoded_la_LIBADD = $(CPPUNIT_LIBS)
unit_wopi_temp_la_SOURCES = UnitWOPITemplate.cpp
unit_wopi_temp_la_LIBADD = $(CPPUNIT_LIBS)
unit_tiff_load_la_SOURCES = UnitTiffLoad.cpp
unit_tiff_load_la_LIBADD = $(CPPUNIT_LIBS)
unit_large_paste_la_SOURCES = UnitLargePaste.cpp
unit_large_paste_la_LIBADD = $(CPPUNIT_LIBS)

if HAVE_LO_PATH
SYSTEM_STAMP = @SYSTEMPLATE_PATH@/system_stamp
else
SYSTEM_STAMP =
endif

if HAVE_LO_PATH
check-local:
	./fakesockettest
	@fc-cache "@LO_PATH@"/share/fonts/truetype
	./run_unit.sh --log-file test.log --trs-file test.trs
# FIXME 2: unit-oob.la fails with symbol undefined:
# UnitWSD::testHandleRequest(UnitWSD::TestRequest, UnitHTTPServerRequest&, UnitHTTPServerResponse&) ,
TESTS = unit-copy-paste.la unit-typing.la unit-convert.la unit-prefork.la unit-tilecache.la unit-timeout.la \
        unit-oauth.la unit-wopi.la unit-wopi-saveas.la \
        unit-wopi-ownertermination.la unit-wopi-versionrestore.la \
        unit-wopi-documentconflict.la unit_wopi_renamefile.la \
	unit-http.la \
	unit-tiff-load.la \
	unit-large-paste.la \
	unit-wopi-loadencoded.la unit-wopi-temp.la
# TESTS = unit-client.la
# TESTS += unit-admin.la
# TESTS += unit-storage.la
else
TESTS = ${top_builddir}/test/test
endif

TEST_EXTENSIONS = .la
LA_LOG_DRIVER = ${top_srcdir}/test/run_unit.sh

EXTRA_DIST = data/delta-text.png data/delta-text2.png data/hello.odt data/hello.txt $(test_SOURCES) $(unittest_SOURCES) run_unit.sh

check_valgrind: all
	@fc-cache "@LO_PATH@"/share/fonts/truetype
	./run_unit.sh --log-file test.log --trs-file test.trs --valgrind

# run unittest during the normal build
all-local: unittest
	@echo
	@echo "Running build-time unit tests.  For more thorough testing, please run 'make check'."
	@echo
	@fc-cache "@LO_PATH@"/share/fonts/truetype
	@UNITTEST=1 ${top_builddir}/test/unittest
