# $FreeBSD$

PORTNAME=	gocheese
PORTVERSION=	2.4.1
CATEGORIES=	devel python
MASTER_SITES=	http://www.gocheese.cypherpunks.ru/

MAINTAINER=	swills@FreeBSD.org
COMMENT=	Python private package repository and caching proxy

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

USES=		go:no_targets tar:xz

INFO=		${PORTNAME}
PLIST_FILES=	${DOCSDIR}/README \
		bin/${PORTNAME} \
		bin/pyshop2packages.sh

do-build:
	@cd ${WRKSRC} ; ${ENV} GOPATH=${WRKSRC} go build -ldflags="-s -w" -o gocheese go.cypherpunks.ru/gocheese/v2

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/${PORTNAME} ${STAGEDIR}${PREFIX}/bin/
	${INSTALL_SCRIPT} ${WRKSRC}/contrib/pyshop2packages.sh ${STAGEDIR}${PREFIX}/bin/
	${INSTALL_DATA} ${WRKSRC}/${PORTNAME}.info ${STAGEDIR}${PREFIX}/${INFO_PATH}
	${MKDIR} ${STAGEDIR}${DOCSDIR}
	${INSTALL_DATA} ${WRKSRC}/README ${STAGEDIR}${DOCSDIR}

.include <bsd.port.mk>
