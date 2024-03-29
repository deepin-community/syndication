add_library(KF5Syndication)
add_library(KF5::Syndication ALIAS KF5Syndication)

set_target_properties(KF5Syndication PROPERTIES
    VERSION     ${SYNDICATION_VERSION}
    SOVERSION   ${SYNDICATION_SOVERSION}
    EXPORT_NAME Syndication
)

target_sources(KF5Syndication PRIVATE
    # librss2local
    rss2/category.cpp
    rss2/cloud.cpp
    rss2/document.cpp
    rss2/enclosure.cpp
    rss2/image.cpp
    rss2/item.cpp
    rss2/parser.cpp
    rss2/source.cpp
    rss2/textinput.cpp
    rss2/tools_p.cpp

    # librdflocal
    rdf/contentvocab.cpp
    rdf/document.cpp
    rdf/dublincore.cpp
    rdf/dublincorevocab.cpp
    rdf/image.cpp
    rdf/item.cpp
    rdf/literal.cpp
    rdf/model.cpp
    rdf/modelmaker.cpp
    rdf/node.cpp
    rdf/nodevisitor.cpp
    rdf/parser.cpp
    rdf/property.cpp
    rdf/rdfvocab.cpp
    rdf/resource.cpp
    rdf/resourcewrapper.cpp
    rdf/rssvocab.cpp
    rdf/sequence.cpp
    rdf/statement.cpp
    rdf/syndicationinfo.cpp
    rdf/syndicationvocab.cpp
    rdf/textinput.cpp

    # libatomlocal
    atom/atomtools.cpp
    atom/category.cpp
    atom/constants.cpp
    atom/content.cpp
    atom/document.cpp
    atom/entry.cpp
    atom/generator.cpp
    atom/link.cpp
    atom/parser.cpp
    atom/person.cpp
    atom/source.cpp

    # libmapperrss2local
    mapper/categoryrss2impl.cpp
    mapper/enclosurerss2impl.cpp
    mapper/feedrss2impl.cpp
    mapper/imagerss2impl.cpp
    mapper/itemrss2impl.cpp

    # libmapperrdflocal
    mapper/feedrdfimpl.cpp
    mapper/imagerdfimpl.cpp
    mapper/itemrdfimpl.cpp

    # libmapperatomlocal
    mapper/categoryatomimpl.cpp
    mapper/enclosureatomimpl.cpp
    mapper/feedatomimpl.cpp
    mapper/itematomimpl.cpp

    # libsyndicationlocal
    abstractparser.cpp
    category.cpp
    constants.cpp
    dataretriever.cpp
    documentsource.cpp
    documentvisitor.cpp
    elementwrapper.cpp
    enclosure.cpp
    feed.cpp
    global.cpp
    image.cpp
    item.cpp
    loader.cpp
    person.cpp
    personimpl.cpp
    specificdocument.cpp
    specificitem.cpp
    specificitemvisitor.cpp
    tools.cpp
    loaderutil.cpp
)

ecm_qt_declare_logging_category(KF5Syndication
    HEADER syndication_debug.h
    IDENTIFIER SYNDICATION_LOG
    CATEGORY_NAME kf.syndication
    OLD_CATEGORY_NAMES kf5.syndication
    DESCRIPTION "syndication"
    EXPORT SYNDICATION
)

generate_export_header(KF5Syndication BASE_NAME syndication)

target_include_directories(KF5Syndication
    INTERFACE
        "$<INSTALL_INTERFACE:${KDE_INSTALL_INCLUDEDIR_KF}/Syndication;${KDE_INSTALL_INCLUDEDIR_KF}/Syndication/syndication>"
    PUBLIC
        "$<BUILD_INTERFACE:${Syndication_SOURCE_DIR}/src;${Syndication_BINARY_DIR}/src;${Syndication_BINARY_DIR}>"
)

kde_target_enable_exceptions(KF5Syndication PUBLIC)

target_link_libraries(KF5Syndication
PUBLIC
    Qt${QT_MAJOR_VERSION}::Core
PRIVATE
    Qt${QT_MAJOR_VERSION}::Xml
    KF5::Codecs
)

add_subdirectory(atom)
add_subdirectory(rdf)
add_subdirectory(rss2)

ecm_generate_headers(Syndication_CamelCase_HEADERS
  HEADER_NAMES
  AbstractParser
  Category
  Constants
  DataRetriever
  DocumentSource
  DocumentVisitor
  ElementWrapper
  Enclosure
  Feed
  Global
  Image
  Item
  Loader
  Mapper
  ParserCollection
  Person
  SpecificDocument
  SpecificItem
  SpecificItemVisitor
  Syndication
  Tools

  PREFIX Syndication
  REQUIRED_HEADERS Syndication_HEADERS
)


install(TARGETS KF5Syndication EXPORT KF5SyndicationTargets ${KF_INSTALL_TARGETS_DEFAULT_ARGS})

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/syndication_export.h
    ${Syndication_HEADERS}
    DESTINATION ${KDE_INSTALL_INCLUDEDIR_KF}/Syndication/syndication
    COMPONENT Devel
)

install(FILES
    ${Syndication_CamelCase_HEADERS}
    DESTINATION ${KDE_INSTALL_INCLUDEDIR_KF}/Syndication/Syndication
    COMPONENT Devel

)

ecm_qt_install_logging_categories(
    EXPORT SYNDICATION
    FILE syndication.categories
    DESTINATION ${KDE_INSTALL_LOGGINGCATEGORIESDIR}
)

if(BUILD_QCH)
    ecm_add_qch(
        KF5Syndication_QCH
        NAME Syndication
        BASE_NAME KF5Syndication
        VERSION ${KF_VERSION}
        ORG_DOMAIN org.kde
        SOURCES # using only public headers, to cover only public API
            ${Syndication_HEADERS}
            ${Syndication_Atom_APIDOX_SRCS}
            ${Syndication_Rdf_APIDOX_SRCS}
            ${Syndication_Rss2_APIDOX_SRCS}
        MD_MAINPAGE "${CMAKE_SOURCE_DIR}/Mainpage.dox"
        LINK_QCHS
            Qt5Core_QCH
        INCLUDE_DIRS
            ${CMAKE_CURRENT_BINARY_DIR}
        BLANK_MACROS
            SYNDICATION_EXPORT
            SYNDICATION_DEPRECATED
        TAGFILE_INSTALL_DESTINATION ${KDE_INSTALL_QTQCHDIR}
        QCH_INSTALL_DESTINATION ${KDE_INSTALL_QTQCHDIR}
        COMPONENT Devel
    )
endif()

ecm_generate_pri_file(BASE_NAME Syndication LIB_NAME KF5Syndication DEPS "" FILENAME_VAR PRI_FILENAME)
install(FILES ${PRI_FILENAME} DESTINATION ${ECM_MKSPECS_INSTALL_DIR})
