# $FreeBSD$

PORTNAME=	oksh
DISTVERSION=	6.7
PORTEPOCH=	1
CATEGORIES=	shells
MASTER_SITES=	https://github.com/ibara/oksh/releases/download/oksh-${DISTVERSION}/

MAINTAINER=	ports@FreeBSD.org
COMMENT=	Portable OpenBSD Korn shell

LICENSE=	PD

USE_HARDENING=	cfi safestack

HAS_CONFIGURE=	yes

OPTIONS_DEFINE=		CURSES STATIC
OPTIONS_DEFAULT=	CURSES

CURSES_DESC=	Use ncurses for screen clearing routines

CURSES_USES=			ncurses
CURSES_CONFIGURE_ENABLE=	curses

STATIC_CONFIGURE_ENABLE=	static

.include <bsd.port.mk>
