#gcc xml.c url.c main.c -I/usr/include/libxml2/ -L/usr/lib -lxml2 -lz -lm  -lcurl  -lssl -lcrypto -ldl && ./a.out
CFLAGS?=	
CFLAGS+=	`pkg-config --cflags libxml-2.0` -L/usr/lib -I/home/boitho/.root/include 
CFLAGS+=	-O0 -g -BLACK_BOX -DIIACL  -DEMAIL_ADRESS_AS_ATTRIBUTE -fPIC
LDFLAGS=	`pkg-config --libs libxml-2.0` -lz -lm  -lcurl  -lssl -lcrypto -ldl -L/home/boitho/.root/lib
PROG?=		crawlExchange
CC=		gcc

# Hack to get common lib files from boitho
COMMONDIR=	../common

VPATH+=		.:$(COMMONDIR):$(COMMONDIR)/../crawl/:../dictionarywordsLot/

OBJS=		xml.o webdav.o main.o
#COMMONSOBJ=	adultWeight.o chtbl.o debug.o iconv.o ir.o logs.o poprank.o sha1.o timediff.o \
#		bfileutil.o config.o DocumentIndex.o iindex.o langdetect.o lot.o reposetory.o  stdlib.o url.o \
#		bsocket.o crc32.o error.o integerindex.o langToNr.o lotlist.o reposetoryNET.o strlcat.o vid.o \
#		bstr.o daemon.o exeoc.o ipdb.o list.o mgsort.o revindex.o strlcpy.o io.o sid.o

OBJS+=		$(COMMONSOBJ)
OBJS+=		crawl.o ../base64/base64.o ../crawl/mailsubject.o set.o

include ../../mk/setup.mk

all: mail_analyze $(PROG)

mail_analyze:
	flex -8 -f -Panalyze_header -i ../crawlExchange/analyze_header.l
	$(CC) -c lex.analyze_header.c -fPIC

$(PROG): $(OBJS)
	libtool --mode=execute $(CC) -o $(PROG).so $(LDFLAGS) $(OBJS) lex.analyze_header.o ../../lib/libds.a -fPIC -shared -D BLACK_BOX -g $(CRAWL_STATIC)
	-mkdir -p ../..//crawlers/$(PROG)
	cp $(PROG).so ../..//crawlers/$(PROG)

clean:
	rm -f ${OBJS} ${PROG}.so lex.analyze_header.[co]
