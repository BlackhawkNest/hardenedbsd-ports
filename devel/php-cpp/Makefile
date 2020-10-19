# $FreeBSD$

PORTNAME=	php-cpp
PORTVERSION=	2.2.0
CATEGORIES=	devel
PKGNAMESUFFIX=	${PHP_PKGNAMESUFFIX}

MAINTAINER=	opensource@blackhawknest.com
COMMENT=	Create PHP C++ bindings with ease

USES=		php:flavors,build uidfix

USE_GITHUB=	yes
#GH_ACCOUNT=	CopernicaMarketingSoftware
GH_ACCOUNT=	lattera
GH_PROJECT=	PHP-CPP
GH_COMMIT=	21f04a5007d649abd7151e137a5ad69eed326da7
GH_TAGNAME=	${GH_COMMIT}

do-build:
	${MAKE} -C ${WRKSRC} -f Makefile.FreeBSD

do-install:
	${MKDIR} ${STAGEDIR}${PREFIX}/include/phpcpp
	(cd ${WRKSRC}/include && \
	    ${COPYTREE_SHARE} \*.h ${STAGEDIR}${PREFIX}/include/phpcpp)
	(cd ${WRKSRC}/ && \
	    ${COPYTREE_SHARE} \*.h ${STAGEDIR}${PREFIX}/include/phpcpp)
	${INSTALL_LIB} ${WRKSRC}/obj/libphpcpp.[sa]* ${STAGEDIR}${PREFIX}/lib
	${LN} -sf libphpcpp.so.${PORTVERSION} ${STAGEDIR}${PREFIX}/lib/libphpcpp.so

.include <bsd.port.mk>
