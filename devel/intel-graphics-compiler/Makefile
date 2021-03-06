# $FreeBSD$

PORTNAME=	intel-graphics-compiler
DISTVERSIONPREFIX=	igc-
DISTVERSION=	1.0.5186
CATEGORIES=	devel

PATCH_SITES=	https://github.com/${GH_ACCOUNT}/${GH_PROJECT}/commit/
PATCHFILES+=	d878880cbb3b.patch:-p1 # https://github.com/intel/intel-graphics-compiler/pull/129

MAINTAINER=	jbeich@FreeBSD.org
COMMENT=	Intel Graphics Compiler for OpenCL

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE.md

ONLY_FOR_ARCHS=	amd64 i386
ONLY_FOR_ARCHS_REASON=	Only Intel GPUs on x86 are supported

LIB_DEPENDS=	libopencl-clang.so:devel/opencl-clang

USES=		bison cmake compiler:c++17-lang python:build
USE_GITHUB=	yes
USE_LDCONFIG=	yes
GH_ACCOUNT=	intel
GH_TUPLE=	intel:vc-intrinsics:82711ce:vcintrin/../vc-intrinsics
CMAKE_OFF=	INSTALL_SPIRVDLL
CFLAGS+=	-Wno-macro-redefined # __fastcall
PLIST_SUB=	ARCH_SUFX=${ARCH:S/i386/32/:S/amd//:S/x86_//}

.if (exists(/usr/lib/clang/10.0.0) || exists(/usr/lib/clang/10.0.1) || exists(/usr/lib/clang/11.0.0)) && ${CXX} == c++
# https://github.com/intel/intel-graphics-compiler/issues/126
BUILD_DEPENDS+=	clang++90:devel/llvm90
USES:=		${USES:Ncompiler*}
CXX=		clang++90
.endif

post-patch:
# lang/python* don't install unsuffixed symlinks
	@${REINPLACE_CMD} -i .python \
		-e '/EXECUTABLE/s/"python"/"${PYTHON_CMD:T}"/' \
		-e '/IGC_PYTHON/s/"python"/"${PYTHON_CMD:T}"/' \
		${WRKSRC}/IGC/CMakeLists.txt \
		${WRKSRC}/IGC/BiFModule/CMakeLists.txt
# devel/llvm* have different naming from upstream under PATH
	@${REINPLACE_CMD} -e '/clang-\$$/s//clang$$/' \
		${WRKSRC}/IGC/BiFModule/CMakeLists.txt
# No need to abort on every benign warning enabled by default
	@${REINPLACE_CMD} -e 's/"-Werror"//; /-Werror/d' \
		${WRKSRC}/IGC/CMakeLists.txt

pre-configure:
# Only one libLLVM can be loaded, so use the same version as devel/opencl-clang
	@for f in ${LOCALBASE}/lib/libopencl-clang.so.*; do \
		${REINPLACE_CMD} "/set.*LLVM_VERSION/s/\".*\"/\"$${f##*so.}\"/" \
			${WRKSRC}/IGC/CMakeLists.txt; \
	done

.include <bsd.port.mk>
