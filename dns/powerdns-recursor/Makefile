# Created by: sten@blinkenlights.nl
# $FreeBSD$

PORTNAME=	recursor
DISTVERSION=	4.3.5
CATEGORIES=	dns
MASTER_SITES=	http://downloads.powerdns.com/releases/
PKGNAMEPREFIX=	powerdns-
DISTNAME=	pdns-${PORTNAME}-${DISTVERSION}

MAINTAINER=	tremere@cainites.net
COMMENT=	Advanced DNS recursor

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

BROKEN_armv6=	fails to compile: use of overloaded operator << is ambiguous
BROKEN_armv7=	fails to compile: use of overloaded operator << is ambiguous
BROKEN_i386=	crashes on startup (SIGSEGV)
BROKEN_FreeBSD_12_powerpc64=	fails to compile: Assembler messages: unaligned opcodes detected in executable segment

USE_HARDENING=	pie:off

BUILD_DEPENDS=	${LOCALBASE}/include/boost/shared_ptr.hpp:devel/boost-libs
LIB_DEPENDS=	libboost_context.so:devel/boost-libs \
		libprotobuf.so:devel/protobuf

USES=		compiler:c++11-lib cpe gmake localbase:ldflags pkgconfig ssl \
		tar:bzip2

CPE_VENDOR=	powerdns
USE_RC_SUBR=	pdns-recursor

GNU_CONFIGURE=	YES
CONFIGURE_ARGS=	--sysconfdir=${PREFIX}/etc/pdns \
		--with-protobuf \
		--without-net-snmp

SUB_FILES=	pkg-message

USERS=		pdns_recursor
GROUPS=		pdns

OPTIONS_DEFAULT=	LUA
OPTIONS_SINGLE=		EXTLUA
OPTIONS_SINGLE_EXTLUA=	LUA LUAJIT LUAJITOR

LUAJITOR_DESC=	Use lang/luajit-openresty
LUAJIT_DESC=	Use lang/luajit
LUA_DESC=	Use lang/lua

LUAJITOR_LIB_DEPENDS=	libluajit-5.1.so.2:lang/luajit-openresty
LUAJITOR_CONFIGURE_ON=	--with-lua=luajit

LUAJIT_LIB_DEPENDS=	libluajit-5.1.so.2:lang/luajit
LUAJIT_CONFIGURE_ON=	--with-lua=luajit

LUA_USES=	lua

.include <bsd.port.pre.mk>

.if ${OPSYS} == FreeBSD && ${OSVERSION} < 1200085 && ${SSL_DEFAULT} != openssl
CONFIGURE_ARGS+=	--enable-libdecaf \
			--enable-libsodium
LIB_DEPENDS+=		libdecaf.so:security/libdecaf \
			libsodium.so:security/libsodium
.endif

post-install:
	@${MKDIR} ${STAGEDIR}/var/run/pdns-recursor

.include <bsd.port.post.mk>
