# $FreeBSD$

PORTNAME=	prometheus
DISTVERSIONPREFIX=	v
DISTVERSION=	2.21.0
CATEGORIES=	net-mgmt

MAINTAINER=	dor.bsd@xm0.uk
COMMENT=	Systems monitoring and alerting toolkit

LICENSE=	APACHE20
LICENSE_FILE=	${WRKSRC}/LICENSE

USES=		go:modules gmake

USE_GITHUB=	yes
GH_TUPLE=	ports-assets:net-mgmt_prometheus2:${DISTVERSIONFULL}:assets

USE_RC_SUBR=	prometheus

OPTIONS_DEFINE=	DOCS

PROMETHEUS_USER?=	prometheus
PROMETHEUS_GROUP?=	prometheus

USERS=		${PROMETHEUS_USER}
GROUPS=		${PROMETHEUS_GROUP}

PROMETHEUS_SHARE_DIR=			${DATADIR}
PROMETHEUS_CONSOLES_DIR=		${PROMETHEUS_SHARE_DIR}/consoles
PROMETHEUS_CONSOLE_LIBRARIES_DIR=	${PROMETHEUS_SHARE_DIR}/console_libraries
PROMETHEUS_DB_DIR=			/var/db/${PORTNAME}
PROMETHEUS_LOG_DIR=			/var/log/prometheus
PROMETHEUS_LOG_FILE=			${PROMETHEUS_LOG_DIR}/prometheus.log

SUB_FILES=	prometheus

SUB_LIST+=	PROMETHEUS_USER=${PROMETHEUS_USER} \
		PROMETHEUS_GROUP=${PROMETHEUS_GROUP} \
		PROMETHEUS_CONSOLE_LIBRARIES_DIR=${PROMETHEUS_CONSOLE_LIBRARIES_DIR} \
		PROMETHEUS_CONSOLES_DIR=${PROMETHEUS_CONSOLES_DIR} \
		PROMETHEUS_DB_DIR=${PROMETHEUS_DB_DIR} \
		PROMETHEUS_LOG_DIR=${PROMETHEUS_LOG_DIR} \
		PROMETHEUS_LOG_FILE=${PROMETHEUS_LOG_FILE} \
		PROMETHEUS_SHARE_DIR=${PROMETHEUS_SHARE_DIR}

PLIST_SUB+=	PORTNAME=${PORTNAME} \
		PROMETHEUS_CONSOLE_LIBRARIES_DIR=${PROMETHEUS_CONSOLE_LIBRARIES_DIR} \
		PROMETHEUS_CONSOLES_DIR=${PROMETHEUS_CONSOLES_DIR} \
		PROMETHEUS_DB_DIR=${PROMETHEUS_DB_DIR} \
		PROMETHEUS_LOG_DIR=${PROMETHEUS_LOG_DIR}

BUILD_USER?=	${USER}
LD_FLAG_X_PREFIX=	-X github.com/prometheus/common/version
LD_FLAG_STRING=		-s \
		${LD_FLAG_X_PREFIX}.Version=${PORTVERSION} \
		${LD_FLAG_X_PREFIX}.Revision=${PORTREVISION} \
		${LD_FLAG_X_PREFIX}.Branch=release-${PORTVERSION:R} \
		${LD_FLAG_X_PREFIX}.BuildUser=${BUILD_USER} \
		${LD_FLAG_X_PREFIX}.BuildDate=${SOURCE_DATE_EPOCH:U${SOURCE_DATE_EPOCH_CMD:sh}}

GO_BUILDFLAGS=	-v -ldflags "${LD_FLAG_STRING}" -tags netgo,builtinassets

GO_TARGET=	./cmd/prometheus \
		./cmd/promtool

# Bring DISTINFO_FILE into scope so we can get the timestamp.
.include <bsd.port.pre.mk>

SOURCE_DATE_EPOCH_CMD=	date -ur $$(${GREP} TIMESTAMP ${DISTINFO_FILE} | ${SED} -e 's/[^0-9]//g') '+%Y%m%d-%T'

post-patch:
	@${LN} -s ${WRKSRC_assets}/web/ui/assets_vfsdata.go \
		${WRKSRC}/web/ui/assets_vfadata.go

post-install:
	${INSTALL_DATA} \
		${WRKSRC}/documentation/examples/prometheus.yml \
		${STAGEDIR}${LOCALBASE}/etc/prometheus.yml.sample
	@${MKDIR} ${STAGEDIR}${DESTDIR}${PROMETHEUS_DB_DIR}
	@${MKDIR} ${STAGEDIR}${DESTDIR}${PROMETHEUS_CONSOLES_DIR}
	@${MKDIR} ${STAGEDIR}${DESTDIR}${PROMETHEUS_CONSOLE_LIBRARIES_DIR}
	@${MKDIR} ${STAGEDIR}${PROMETHEUS_LOG_DIR}
	( cd ${WRKSRC}/console_libraries \
		&& ${COPYTREE_SHARE} . ${STAGEDIR}${PROMETHEUS_CONSOLE_LIBRARIES_DIR} \
	)
	( cd ${WRKSRC}/consoles \
		&& ${COPYTREE_SHARE} . ${STAGEDIR}${PROMETHEUS_CONSOLES_DIR} \
	)

post-install-DOCS-on:
	${MKDIR} ${STAGEDIR}${DOCSDIR}
	cd ${WRKSRC}/docs && \
		${COPYTREE_SHARE} . ${STAGEDIR}${DOCSDIR}

.include <bsd.port.post.mk>
