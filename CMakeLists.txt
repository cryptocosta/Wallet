cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB CostaCoinre)

include_directories(${CMAKE_BINARY_DIR}
    src
    CostaCoinre/external
    CostaCoinre/include
    CostaCoinre/src)

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
configure_file("CostaCoinre/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  CostaCoinre/external/miniupnpc/connecthostport.c
  CostaCoinre/external/miniupnpc/igd_desc_parse.c
  CostaCoinre/external/miniupnpc/minisoap.c
  CostaCoinre/external/miniupnpc/miniupnpc.c
  CostaCoinre/external/miniupnpc/miniwget.c
  CostaCoinre/external/miniupnpc/minixml.c
  CostaCoinre/external/miniupnpc/portlistingparse.c
  CostaCoinre/external/miniupnpc/receivedata.c
  CostaCoinre/external/miniupnpc/upnpcommands.c
  CostaCoinre/external/miniupnpc/upnpreplyparse.c
  CostaCoinre/src/Common/Base58.cpp
  CostaCoinre/src/Common/CommandLine.cpp
  CostaCoinre/src/Common/Util.cpp
  CostaCoinre/src/Common/StringTools.cpp
  CostaCoinre/src/Common/JsonValue.cpp
  CostaCoinre/src/Common/ConsoleTools.cpp
  CostaCoinre/src/Common/MemoryInputStream.cpp
  CostaCoinre/src/Common/PathTools.cpp
  CostaCoinre/src/Common/StdInputStream.cpp
  CostaCoinre/src/Common/StdOutputStream.cpp
  CostaCoinre/src/Common/StreamTools.cpp
  CostaCoinre/src/Common/StringOutputStream.cpp
  CostaCoinre/src/Common/StringView.cpp
  CostaCoinre/src/Common/VectorOutputStream.cpp
  CostaCoinre/src/crypto/blake256.c
  CostaCoinre/src/crypto/chacha8.c
  CostaCoinre/src/crypto/crypto-ops-data.c
  CostaCoinre/src/crypto/crypto-ops.c
  CostaCoinre/src/crypto/crypto.cpp
  CostaCoinre/src/crypto/groestl.c
  CostaCoinre/src/crypto/hash-extra-blake.c
  CostaCoinre/src/crypto/hash-extra-groestl.c
  CostaCoinre/src/crypto/hash-extra-jh.c
  CostaCoinre/src/crypto/hash-extra-skein.c
  CostaCoinre/src/crypto/hash.c
  CostaCoinre/src/crypto/jh.c
  CostaCoinre/src/crypto/keccak.c
  CostaCoinre/src/crypto/oaes_lib.c
  CostaCoinre/src/crypto/random.c
  CostaCoinre/src/crypto/skein.c
  CostaCoinre/src/crypto/slow-hash.c
  CostaCoinre/src/crypto/slow-hash.cpp
  CostaCoinre/src/crypto/tree-hash.c
  CostaCoinre/src/CryptoNoteCore/BlockchainIndices.cpp
  CostaCoinre/src/CryptoNoteCore/BlockchainMessages.cpp
  CostaCoinre/src/CryptoNoteCore/BlockIndex.cpp
  CostaCoinre/src/CryptoNoteCore/CoreConfig.cpp
  CostaCoinre/src/CryptoNoteCore/CryptoNoteBasic.cpp
  CostaCoinre/src/CryptoNoteCore/CryptoNoteTools.cpp
  CostaCoinre/src/CryptoNoteCore/Currency.cpp
  CostaCoinre/src/CryptoNoteCore/MinerConfig.cpp
  CostaCoinre/src/CryptoNoteCore/Transaction.cpp
  CostaCoinre/src/CryptoNoteCore/Account.cpp
  CostaCoinre/src/CryptoNoteCore/Blockchain.cpp
  CostaCoinre/src/CryptoNoteCore/Checkpoints.cpp
  CostaCoinre/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  CostaCoinre/src/CryptoNoteCore/Core.cpp
  CostaCoinre/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  CostaCoinre/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  CostaCoinre/src/CryptoNoteCore/Difficulty.cpp
  CostaCoinre/src/CryptoNoteCore/IBlock.cpp
  CostaCoinre/src/CryptoNoteCore/Miner.cpp
  CostaCoinre/src/CryptoNoteCore/TransactionExtra.cpp
  CostaCoinre/src/CryptoNoteCore/TransactionPool.cpp
  CostaCoinre/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  CostaCoinre/src/CryptoNoteCore/TransactionUtils.cpp
  CostaCoinre/src/InProcessNode/InProcessNode.cpp
  CostaCoinre/src/InProcessNode/InProcessNodeErrors.cpp
  CostaCoinre/src/NodeRpcProxy/NodeErrors.cpp
  CostaCoinre/src/NodeRpcProxy/NodeRpcProxy.cpp
  CostaCoinre/src/P2p/NetNodeConfig.cpp
  CostaCoinre/src/Serialization/BinaryInputStreamSerializer.cpp
  CostaCoinre/src/Serialization/BinaryOutputStreamSerializer.cpp
  CostaCoinre/src/Serialization/JsonInputValueSerializer.cpp
  CostaCoinre/src/Serialization/JsonOutputStreamSerializer.cpp
  CostaCoinre/src/Serialization/KVBinaryInputStreamSerializer.cpp
  CostaCoinre/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  CostaCoinre/src/Serialization/SerializationOverloads.cpp
  CostaCoinre/src/Transfers/BlockchainSynchronizer.cpp
  CostaCoinre/src/Transfers/SynchronizationState.cpp
  CostaCoinre/src/Transfers/TransfersConsumer.cpp
  CostaCoinre/src/Transfers/TransfersContainer.cpp
  CostaCoinre/src/Transfers/TransfersSubscription.cpp
  CostaCoinre/src/Transfers/TransfersSynchronizer.cpp
  CostaCoinre/src/Wallet/LegacyKeysImporter.cpp
  CostaCoinre/src/Wallet/WalletAsyncContextCounter.cpp
  CostaCoinre/src/Wallet/WalletErrors.cpp
  CostaCoinre/src/WalletLegacy/KeysStorage.cpp
  CostaCoinre/src/WalletLegacy/WalletLegacy.cpp
  CostaCoinre/src/WalletLegacy/WalletHelper.cpp
  CostaCoinre/src/WalletLegacy/WalletLegacySerializer.cpp
  CostaCoinre/src/WalletLegacy/WalletLegacySerialization.cpp
  CostaCoinre/src/WalletLegacy/WalletTransactionSender.cpp
  CostaCoinre/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  CostaCoinre/src/WalletLegacy/WalletUserTransactionsCache.cpp
  CostaCoinre/src/System/ContextGroup.cpp
  CostaCoinre/src/System/Event.cpp
  CostaCoinre/src/System/EventLock.cpp
  CostaCoinre/src/System/InterruptedException.cpp
  CostaCoinre/src/System/Ipv4Address.cpp
  CostaCoinre/src/System/TcpStream.cpp
  CostaCoinre/src/HTTP/HttpRequest.cpp
  CostaCoinre/src/HTTP/HttpParser.cpp
  CostaCoinre/src/HTTP/HttpParserErrorCodes.cpp
  CostaCoinre/src/HTTP/HttpResponse.cpp
  CostaCoinre/src/Rpc/HttpClient.cpp
  CostaCoinre/src/Rpc/JsonRpc.cpp
  CostaCoinre/src/P2p/NetNode.cpp
  CostaCoinre/src/P2p/LevinProtocol.cpp
  CostaCoinre/src/P2p/NetNodeConfig.cpp
  CostaCoinre/src/P2p/PeerListManager.cpp
  CostaCoinre/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  CostaCoinre/src/Logging/ILogger.cpp
  CostaCoinre/src/Logging/LoggerGroup.cpp
  CostaCoinre/src/Logging/CommonLogger.cpp
  CostaCoinre/src/Logging/LoggerManager.cpp
  CostaCoinre/src/Logging/FileLogger.cpp
  CostaCoinre/src/Logging/StreamLogger.cpp
  CostaCoinre/src/Logging/ConsoleLogger.cpp
  CostaCoinre/src/Logging/LoggerMessage.cpp
  CostaCoinre/src/Logging/LoggerRef.cpp
  CostaCoinre/src/BlockchainExplorer/BlockchainExplorer.cpp
  CostaCoinre/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  CostaCoinre/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(CostaCoinre/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} CostaCoinre/external/miniupnpc/minissdpc.c)
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

    set(MACOSX_BUNDLE_ICON_FILE CostaCoinre.icns)
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

include_directories(CostaCoinre/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES CostaCoinre/src/Platform/${PLATFORM_DIR}/System/*)
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
