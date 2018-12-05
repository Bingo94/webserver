JANSSON 2.10编译：(for 2.11, skip step 1 & 2)
1.//apt-get install libtool
2.//autoreconf -i 
3../configure --host=arm-none-linux CC=arm-none-linux-gnueabi-gcc CXX=arm-none-linux-gnueabi-g++
4.make 
5.make install
6.编译生成libjansson动态库在/usr/local/lib下 一共5个，把相应库文件拷贝到3rdparty下的lib下，3rdparty下的include下新建jansson文件夹存放jansson的头文件

Orcania库编译：
1.cd  ulfius/lib/orcania/src
2.修改Makefile文件：
CC=arm-none-linux-gnueabi-gcc
LIBJANSSON_INCLUDE=../../../../include/jansson
LIBJANSSON_LOCATION=../../../../
LIBS=-L$(LIBJANSSON_LOCATION)lib -lc $(LJANSSON)
CFLAGS+=-c -fPIC -Wall -Werror -Wextra -D_REENTRANT -I ../../../../include/jansson $(ADDITIONALFLAGS) $(JANSSONFLAG)

3.Make
4.复制相应的库到3rdparty/lib下，3rdparty下的include下新建orcania文件夹存放orcania的头文件

Yder库编译：
1.cd  ulfius/lib/yder/src
2. 修改makefile：
CC=arm-none-linux-gnueabi-gcc
CFLAGS+=-c -fPIC -Wall -D_REENTRANT -I../../../../include/orcania -I../../../../include/jansson $(ADDITIONALFLAGS)
LIBS=-L../../../../lib -lc -lorcania $(ADDITIONALLIBS)
2.make Y_DISABLE_JOURNALD=1
3.复制相应的库到3rdparty/lib下，3rdparty下的include下新建yder文件夹存放yder

Ulfius库编译：
1. cd  /ulfius/src
2.修改makefile：
LIBMHD_INCLUDE=../../include/mhd 
        LIBJANSSON_INCLUDE=../../include/jansson
        DESTDIR=../../
CC=arm-none-linux-gnueabi-gcc
CFLAGS+=-c -pedantic -std=gnu99 -fPIC -Wall -D_REENTRANT -I$(LIBMHD_INCLUDE) -I$(ULFIUS_INCLUDE) -I$(LIBJANSSON_INCLUDE) -I$(LIBORCANIA_LOCATION) -I$(LIBYDER_LOCATION) $(ADDITIONALFLAGS) $(JANSSONFLAG) $(CURLFLAG) $(WEBSOCKETFLAG) $(CPPFLAGS)

LIBS=-L$(DESTDIR)/lib -L$(LIBORCANIA_LOCATION) -L$(LIBYDER_LOCATION) -lc -lmicrohttpd -lyder -lorcania -lpthread -ljansson $(LDFLAGS)
3 编译 make  Y_DISABLE_JOURNALD=1 CURLFLAG=-DU_DISABLE_CURL WEBSOCKETFLAG=-DU_DISABLE_WEBSOCKET
4 复制相应的库到3rdparty/lib下，3rdparty下的include下新建ulfius文件夹存放ulfius 

Sheep_counter例子编译：
修改makefile：CC=arm-none-linux-gnueabi-gcc
ADDITIONALFLAGS =-I../../../include/orcania -I ../../../include/mhd \
 -I ../../../include/yder -I../../../include/jansson -I../../../include/ulfius
CFLAGS=-c -Wall -D_REENTRANT $(ADDITIONALFLAGS)
ULFIUS_LOCATION=../../../../lib
#LIBS=-lc -lulfius -lyder -lorcania -ljansson -L$(ULFIUS_LOCATION)
LIBS=-L../../../lib -lc -lulfius -lyder -lorcania -lmicrohttpd -ljansson
make command:
make CC=arm-none-linux-gnueabi-gcc Y_DISABLE_JOURNALD=1 CURLFLAG=-DU_DISABLE_CURL WEBSOCKETFLAG=-DU_DISABLE_WEBSOCKET







