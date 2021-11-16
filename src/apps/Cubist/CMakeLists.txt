SET(CUBIST_SRCS
        global.c
        xval.c
        cubist.c
        sort.c
        construct.c
        predict.c
        stats.c
        discr.c
        rules.c
        contin.c
        formrules.c
        formtree.c
        getdata.c
        getnames.c
        implicitatt.c
        instance.c
        modelfiles.c
        prunetree.c
        regress.c
        trees.c
        update.c
        utility.c
	defns.i
	text.i
)

ADD_EXECUTABLE(cubist
     ${CUBIST_SRCS}
)

TARGET_LINK_LIBRARIES(cubist
    m
)

ADD_EXECUTABLE(summary
	summary.c
)

TARGET_LINK_LIBRARIES(summary
    m
)

