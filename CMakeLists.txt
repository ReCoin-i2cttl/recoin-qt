cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB recoin)

include_directories(${CMAKE_BINARY_DIR}
    src
    recoin/external
    recoin/include
    recoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("recoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  recoin/external/miniupnpc/connecthostport.c
  recoin/external/miniupnpc/igd_desc_parse.c
  recoin/external/miniupnpc/minisoap.c
  recoin/external/miniupnpc/miniupnpc.c
  recoin/external/miniupnpc/miniwget.c
  recoin/external/miniupnpc/minixml.c
  recoin/external/miniupnpc/portlistingparse.c
  recoin/external/miniupnpc/receivedata.c
  recoin/external/miniupnpc/upnpcommands.c
  recoin/external/miniupnpc/upnpreplyparse.c
  recoin/src/Common/Base58.cpp
  recoin/src/Common/CommandLine.cpp
  recoin/src/Common/Util.cpp
  recoin/src/Common/StringTools.cpp
  recoin/src/Common/JsonValue.cpp
  recoin/src/Common/ConsoleTools.cpp
  recoin/src/Common/MemoryInputStream.cpp
  recoin/src/Common/PathTools.cpp
  recoin/src/Common/StdInputStream.cpp
  recoin/src/Common/StdOutputStream.cpp
  recoin/src/Common/StreamTools.cpp
  recoin/src/Common/StringOutputStream.cpp
  recoin/src/Common/StringView.cpp
  recoin/src/Common/VectorOutputStream.cpp
  recoin/src/crypto/blake256.c
  recoin/src/crypto/chacha8.c
  recoin/src/crypto/crypto-ops-data.c
  recoin/src/crypto/crypto-ops.c
  recoin/src/crypto/crypto.cpp
  recoin/src/crypto/groestl.c
  recoin/src/crypto/hash-extra-blake.c
  recoin/src/crypto/hash-extra-groestl.c
  recoin/src/crypto/hash-extra-jh.c
  recoin/src/crypto/hash-extra-skein.c
  recoin/src/crypto/hash.c
  recoin/src/crypto/jh.c
  recoin/src/crypto/keccak.c
  recoin/src/crypto/oaes_lib.c
  recoin/src/crypto/random.c
  recoin/src/crypto/skein.c
  recoin/src/crypto/slow-hash.c
  recoin/src/crypto/slow-hash.cpp
  recoin/src/crypto/tree-hash.c
  recoin/src/CryptoNoteCore/BlockchainIndices.cpp
  recoin/src/CryptoNoteCore/BlockchainMessages.cpp
  recoin/src/CryptoNoteCore/BlockIndex.cpp
  recoin/src/CryptoNoteCore/CoreConfig.cpp
  recoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  recoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  recoin/src/CryptoNoteCore/Currency.cpp
  recoin/src/CryptoNoteCore/MinerConfig.cpp
  recoin/src/CryptoNoteCore/Transaction.cpp
  recoin/src/CryptoNoteCore/Account.cpp
  recoin/src/CryptoNoteCore/Blockchain.cpp
  recoin/src/CryptoNoteCore/Checkpoints.cpp
  recoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  recoin/src/CryptoNoteCore/Core.cpp
  recoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  recoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  recoin/src/CryptoNoteCore/Difficulty.cpp
  recoin/src/CryptoNoteCore/IBlock.cpp
  recoin/src/CryptoNoteCore/Miner.cpp
  recoin/src/CryptoNoteCore/TransactionExtra.cpp
  recoin/src/CryptoNoteCore/TransactionPool.cpp
  recoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  recoin/src/CryptoNoteCore/TransactionUtils.cpp
  recoin/src/InProcessNode/InProcessNode.cpp
  recoin/src/InProcessNode/InProcessNodeErrors.cpp
  recoin/src/NodeRpcProxy/NodeErrors.cpp
  recoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  recoin/src/P2p/NetNodeConfig.cpp
  recoin/src/Serialization/BinaryInputStreamSerializer.cpp
  recoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  recoin/src/Serialization/JsonInputValueSerializer.cpp
  recoin/src/Serialization/JsonOutputStreamSerializer.cpp
  recoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  recoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  recoin/src/Serialization/SerializationOverloads.cpp
  recoin/src/Transfers/BlockchainSynchronizer.cpp
  recoin/src/Transfers/SynchronizationState.cpp
  recoin/src/Transfers/TransfersConsumer.cpp
  recoin/src/Transfers/TransfersContainer.cpp
  recoin/src/Transfers/TransfersSubscription.cpp
  recoin/src/Transfers/TransfersSynchronizer.cpp
  recoin/src/Wallet/LegacyKeysImporter.cpp
  recoin/src/Wallet/WalletAsyncContextCounter.cpp
  recoin/src/Wallet/WalletErrors.cpp
  recoin/src/WalletLegacy/KeysStorage.cpp
  recoin/src/WalletLegacy/WalletLegacy.cpp
  recoin/src/WalletLegacy/WalletHelper.cpp
  recoin/src/WalletLegacy/WalletLegacySerializer.cpp
  recoin/src/WalletLegacy/WalletLegacySerialization.cpp
  recoin/src/WalletLegacy/WalletTransactionSender.cpp
  recoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  recoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  recoin/src/System/ContextGroup.cpp
  recoin/src/System/Event.cpp
  recoin/src/System/EventLock.cpp
  recoin/src/System/InterruptedException.cpp
  recoin/src/System/Ipv4Address.cpp
  recoin/src/System/TcpStream.cpp
  recoin/src/HTTP/HttpRequest.cpp
  recoin/src/HTTP/HttpParser.cpp
  recoin/src/HTTP/HttpParserErrorCodes.cpp
  recoin/src/HTTP/HttpResponse.cpp
  recoin/src/Rpc/HttpClient.cpp
  recoin/src/Rpc/JsonRpc.cpp
  recoin/src/P2p/NetNode.cpp
  recoin/src/P2p/LevinProtocol.cpp
  recoin/src/P2p/NetNodeConfig.cpp
  recoin/src/P2p/PeerListManager.cpp
  recoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  recoin/src/Logging/ILogger.cpp
  recoin/src/Logging/LoggerGroup.cpp
  recoin/src/Logging/CommonLogger.cpp
  recoin/src/Logging/LoggerManager.cpp
  recoin/src/Logging/FileLogger.cpp
  recoin/src/Logging/StreamLogger.cpp
  recoin/src/Logging/ConsoleLogger.cpp
  recoin/src/Logging/LoggerMessage.cpp
  recoin/src/Logging/LoggerRef.cpp
  recoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  recoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  recoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(recoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} recoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(recoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES recoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
