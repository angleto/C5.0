INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR})

SET(C5_SRCS
    global.c
    c50.c
    construct.c
    formtree.c
    info.c
    discr.c
    contin.c
    subset.c
    prune.c
    p-thresh.c
    trees.c
    siftrules.c
    ruletree.c
    rules.c
    getdata.c
    implicitatt.c
    mcost.c
    confmat.c
    sort.c
    update.c
    attwinnow.c
    classify.c
    formrules.c
    ${COMMON_SRC}/getnames.c
    modelfiles.c
    utility.c
    xval.c
)

ADD_EXECUTABLE(c5.0
     ${C5_SRCS}
)

TARGET_LINK_LIBRARIES(c5.0
    m
)

ADD_EXECUTABLE(report
	 report.c
)

TARGET_LINK_LIBRARIES(report
	m
)

