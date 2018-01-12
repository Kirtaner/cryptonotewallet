cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    stankcoin/external
    stankcoin/include
    stankcoin/src)

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
configure_file("stankcoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  stankcoin/external/miniupnpc/connecthostport.c
  stankcoin/external/miniupnpc/igd_desc_parse.c
  stankcoin/external/miniupnpc/minisoap.c
  stankcoin/external/miniupnpc/miniupnpc.c
  stankcoin/external/miniupnpc/miniwget.c
  stankcoin/external/miniupnpc/minixml.c
  stankcoin/external/miniupnpc/portlistingparse.c
  stankcoin/external/miniupnpc/receivedata.c
  stankcoin/external/miniupnpc/upnpcommands.c
  stankcoin/external/miniupnpc/upnpreplyparse.c
  stankcoin/src/Common/Base58.cpp
  stankcoin/src/Common/CommandLine.cpp
  stankcoin/src/Common/Util.cpp
  stankcoin/src/Common/StringTools.cpp
  stankcoin/src/Common/JsonValue.cpp
  stankcoin/src/Common/ConsoleTools.cpp
  stankcoin/src/Common/MemoryInputStream.cpp
  stankcoin/src/Common/PathTools.cpp
  stankcoin/src/Common/StdInputStream.cpp
  stankcoin/src/Common/StdOutputStream.cpp
  stankcoin/src/Common/StreamTools.cpp
  stankcoin/src/Common/StringOutputStream.cpp
  stankcoin/src/Common/StringView.cpp
  stankcoin/src/Common/VectorOutputStream.cpp
  stankcoin/src/crypto/blake256.c
  stankcoin/src/crypto/chacha8.c
  stankcoin/src/crypto/crypto-ops-data.c
  stankcoin/src/crypto/crypto-ops.c
  stankcoin/src/crypto/crypto.cpp
  stankcoin/src/crypto/groestl.c
  stankcoin/src/crypto/hash-extra-blake.c
  stankcoin/src/crypto/hash-extra-groestl.c
  stankcoin/src/crypto/hash-extra-jh.c
  stankcoin/src/crypto/hash-extra-skein.c
  stankcoin/src/crypto/hash.c
  stankcoin/src/crypto/jh.c
  stankcoin/src/crypto/keccak.c
  stankcoin/src/crypto/oaes_lib.c
  stankcoin/src/crypto/random.c
  stankcoin/src/crypto/skein.c
  stankcoin/src/crypto/slow-hash.c
  stankcoin/src/crypto/slow-hash.cpp
  stankcoin/src/crypto/tree-hash.c
  stankcoin/src/CryptoNoteCore/BlockchainIndices.cpp
  stankcoin/src/CryptoNoteCore/BlockchainMessages.cpp
  stankcoin/src/CryptoNoteCore/BlockIndex.cpp
  stankcoin/src/CryptoNoteCore/CoreConfig.cpp
  stankcoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  stankcoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  stankcoin/src/CryptoNoteCore/Currency.cpp
  stankcoin/src/CryptoNoteCore/MinerConfig.cpp
  stankcoin/src/CryptoNoteCore/Transaction.cpp
  stankcoin/src/CryptoNoteCore/Account.cpp
  stankcoin/src/CryptoNoteCore/Blockchain.cpp
  stankcoin/src/CryptoNoteCore/Checkpoints.cpp
  stankcoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  stankcoin/src/CryptoNoteCore/Core.cpp
  stankcoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  stankcoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  stankcoin/src/CryptoNoteCore/Difficulty.cpp
  stankcoin/src/CryptoNoteCore/IBlock.cpp
  stankcoin/src/CryptoNoteCore/Miner.cpp
  stankcoin/src/CryptoNoteCore/TransactionExtra.cpp
  stankcoin/src/CryptoNoteCore/TransactionPool.cpp
  stankcoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  stankcoin/src/CryptoNoteCore/TransactionUtils.cpp
  stankcoin/src/InProcessNode/InProcessNode.cpp
  stankcoin/src/InProcessNode/InProcessNodeErrors.cpp
  stankcoin/src/NodeRpcProxy/NodeErrors.cpp
  stankcoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  stankcoin/src/P2p/NetNodeConfig.cpp
  stankcoin/src/Serialization/BinaryInputStreamSerializer.cpp
  stankcoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  stankcoin/src/Serialization/JsonInputValueSerializer.cpp
  stankcoin/src/Serialization/JsonOutputStreamSerializer.cpp
  stankcoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  stankcoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  stankcoin/src/Serialization/SerializationOverloads.cpp
  stankcoin/src/Transfers/BlockchainSynchronizer.cpp
  stankcoin/src/Transfers/SynchronizationState.cpp
  stankcoin/src/Transfers/TransfersConsumer.cpp
  stankcoin/src/Transfers/TransfersContainer.cpp
  stankcoin/src/Transfers/TransfersSubscription.cpp
  stankcoin/src/Transfers/TransfersSynchronizer.cpp
  stankcoin/src/Wallet/LegacyKeysImporter.cpp
  stankcoin/src/Wallet/WalletAsyncContextCounter.cpp
  stankcoin/src/Wallet/WalletErrors.cpp
  stankcoin/src/WalletLegacy/KeysStorage.cpp
  stankcoin/src/WalletLegacy/WalletLegacy.cpp
  stankcoin/src/WalletLegacy/WalletHelper.cpp
  stankcoin/src/WalletLegacy/WalletLegacySerializer.cpp
  stankcoin/src/WalletLegacy/WalletLegacySerialization.cpp
  stankcoin/src/WalletLegacy/WalletTransactionSender.cpp
  stankcoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  stankcoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  stankcoin/src/System/ContextGroup.cpp
  stankcoin/src/System/Event.cpp
  stankcoin/src/System/EventLock.cpp
  stankcoin/src/System/InterruptedException.cpp
  stankcoin/src/System/Ipv4Address.cpp
  stankcoin/src/System/TcpStream.cpp
  stankcoin/src/HTTP/HttpRequest.cpp
  stankcoin/src/HTTP/HttpParser.cpp
  stankcoin/src/HTTP/HttpParserErrorCodes.cpp
  stankcoin/src/HTTP/HttpResponse.cpp
  stankcoin/src/Rpc/HttpClient.cpp
  stankcoin/src/Rpc/JsonRpc.cpp
  stankcoin/src/P2p/NetNode.cpp
  stankcoin/src/P2p/LevinProtocol.cpp
  stankcoin/src/P2p/NetNodeConfig.cpp
  stankcoin/src/P2p/PeerListManager.cpp
  stankcoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  stankcoin/src/Logging/ILogger.cpp
  stankcoin/src/Logging/LoggerGroup.cpp
  stankcoin/src/Logging/CommonLogger.cpp
  stankcoin/src/Logging/LoggerManager.cpp
  stankcoin/src/Logging/FileLogger.cpp
  stankcoin/src/Logging/StreamLogger.cpp
  stankcoin/src/Logging/ConsoleLogger.cpp
  stankcoin/src/Logging/LoggerMessage.cpp
  stankcoin/src/Logging/LoggerRef.cpp
  stankcoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  stankcoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  stankcoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(stankcoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} stankcoin/external/miniupnpc/minissdpc.c)
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

include_directories(stankcoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES stankcoin/src/Platform/${PLATFORM_DIR}/System/*)
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
