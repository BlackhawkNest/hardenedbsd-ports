# Created by: Dryice Liu	<dryice@dryice.name>
# $FreeBSD$

PORTNAME=	PasteScript
PORTVERSION=	2.0.2
PORTREVISION=	1
CATEGORIES=	www python
MASTER_SITES=	CHEESESHOP
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}

MAINTAINER=	python@FreeBSD.org
COMMENT=	Pluggable command-line frontend

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/docs/license.txt

RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}cheetah>=0:devel/py-cheetah@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}flup6>=0:www/py-flup6@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}Paste>=1.3:www/py-paste@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pastedeploy>=0:www/py-pastedeploy@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}six>=0:devel/py-six@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}wsgiutils>=0:devel/py-wsgiutils@${PY_FLAVOR}

USES=		python:2.7
USE_PYTHON=	autoplist distutils
NO_ARCH=	yes

post-extract:
	@${RM} -r ${WRKSRC}/tests

.include <bsd.port.mk>
