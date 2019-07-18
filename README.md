本自述文件只是一个快速入门的快速文档。您可以在redis.io上找到更详细的文档。

什么是Redis？
Redis通常被称为数据结构服务器。这意味着Redis通过一组命令提供对可变数据结构的访问，这些命令使用带有TCP套接字和简单协议的服务器 - 客户端模型发送。因此，不同的进程可以以共享方式查询和修改相同的数据结构。

Redis中实现的数据结构有一些特殊属性：

Redis关心将它们存储在磁盘上，即使它们总是被提供并修改到服务器内存中。这意味着Redis速度很快，但这也是非易失性的。
数据结构的实现强调内存效率，因此与使用高级编程语言建模的相同数据结构相比，Redis内部的数据结构可能使用更少的内存。
Redis提供了许多在数据库中自然可以找到的功能，如复制，可调节的持久性级别，群集，高可用性。
另一个很好的例子是将Redis视为memcached的更复杂版本，其中操作不仅仅是SET和GET，而是用于处理复杂数据类型（如Lists，Sets，有序数据结构等）的操作。

如果您想了解更多信息，请参阅选定的起点列表：

Redis数据类型简介。http://redis.io/topics/data-types-intro
直接在浏览器中尝试Redis。http://try.redis.io
Redis命令的完整列表。http://redis.io/commands
Redis官方文档中还有更多内容。http://redis.io/documentation
建立Redis
Redis可以在Linux，OSX，OpenBSD，NetBSD，FreeBSD上编译和使用。我们支持大端和小端架构，以及32位和64位系统。

它可以在Solaris派生系统（例如SmartOS）上编译，但我们对此平台的支持是最好的，并且不保证Redis在Linux，OSX和* BSD中工作得很好。

它很简单：

% make
您可以使用以下命令运行32位Redis二进制文件：

% make 32bit
在构建Redis之后，最好使用以下方法对其进行测试：

% make test
修复依赖项或缓存构建选项的构建问题
Redis有一些包含在deps目录中的依赖项。 make即使依赖项的源代码中的某些内容发生更改，也不会自动重建依赖项。

当您使用git pull或以任何其他方式修改依赖项树中的代码更新源代码时，请确保使用以下命令以便真正清理所有内容并从头开始重建：

make distclean
这将清洁：jemalloc，lua，hiredis，linenoise。

此外，如果您强制某些构建选项（如32位目标，没有C编译器优化（用于调试目的））和其他类似的构建时选项，那么这些选项将无限期地缓存，直到您发出make distclean 命令。

修复构建32位二进制文​​件的问题
如果在使用32位目标构建Redis之后需要使用64位目标重建它，或者相反，则需要make distclean在Redis分发的根目录中执行a 。

如果在尝试构建Redis的32位二进制文​​件时出现构建错误，请尝试以下步骤：

安装包libc6-dev-i386（也可以尝试g ++ - multilib）。
尝试使用以下命令行而不是make 32bit： make CFLAGS="-m32 -march=native" LDFLAGS="-m32"
分配器
在构建Redis时选择非默认内存分配器是通过设置MALLOC环境变量来完成的。默认情况下，Redis是针对libc malloc编译和链接的，除了jemalloc是Linux系统上的默认设置。选择此默认值是因为jemalloc已证明比libc malloc具有更少的碎片问题。

要强制编译libc malloc，请使用：

% make MALLOC=libc
要在Mac OS X系统上针对jemalloc进行编译，请使用：

% make MALLOC=jemalloc
详细的构建
默认情况下，Redis将使用用户友好的彩色输出进行构建。如果要查看更详细的输出，请使用以下命令：

% make V=1
运行Redis
要使用默认配置运行Redis，只需键入：

% cd src
% ./redis-server
如果要提供redis.conf，则必须使用其他参数（配置文件的路径）运行它：

% cd src
% ./redis-server /path/to/redis.conf
可以通过使用命令行直接将参数作为选项传递来更改Redis配置。例子：

% ./redis-server --port 9999 --replicaof 127.0.0.1 6379
% ./redis-server /etc/redis/6379.conf --loglevel debug
使用命令行支持redis.conf中的所有选项作为选项，名称完全相同。

和Redis一起玩
您可以使用redis-cli与Redis一起玩。启动redis-server实例，然后在另一个终端中尝试以下操作：

% cd src
% ./redis-cli
redis> ping
PONG
redis> set foo bar
OK
redis> get foo
"bar"
redis> incr mycounter
(integer) 1
redis> incr mycounter
(integer) 2
redis>
您可以在http://redis.io/commands找到所有可用命令的列表。

安装Redis
要将Redis二进制文件安装到/ usr / local / bin中，只需使用：

% make install
make PREFIX=/some/other/directory install如果您希望使用其他目的地，则可以使用。

make install只会在系统中安装二进制文件，但不会在适当的位置配置init脚本和配置文件。如果您只想与Redis一起玩，则不需要这样做，但如果您正在为生产系统安装它，我们有一个脚本为Ubuntu和Debian系统执行此操作：

% cd utils
% ./install_server.sh
注意：install_server.sh不适用于Mac OSX; 它仅适用于Linux。

该脚本将向您提出几个问题，并将设置您正确运行Redis作为后台守护程序所需的一切，该守护程序将在系统重新启动时重新启动。

您可以停止并使用名为脚本启动Redis的 /etc/init.d/redis_<portnumber>，例如/etc/init.d/redis_6379。

代码贡献
注意：通过以任何形式向Redis项目提供代码，包括通过Github发送拉取请求，通过私人电子邮件或公共讨论组发送代码片段或补丁，您同意根据BSD许可条款发布您的代码，您可以在Redis源代码分发中包含的COPYING文件中查找。

有关详细信息，请参阅此源代码分发中的CONTRIBUTING文件。

Redis内部
如果您正在阅读本自述文件，您可能会在Github页面前面或者您只是解开Redis分发tar球。在这两种情况下，您距离源代码基本上只有一步之遥，因此我们在这里解释Redis源代码布局，每个文件中的一般概念，Redis服务器内部最重要的功能和结构等等。我们将所有的讨论保持在高水平而不深入细节，因为这个文档会很大，否则我们的代码库会不断变化，但是一般的想法应该是了解更多内容的一个好的起点。此外，大多数代码都有大量注释并且易于遵循。

源代码布局
Redis根目录只包含此README，Makefile调用src目录中的实际Makefile 以及Redis和Sentinel的示例配置。您可以找到一些用于执行Redis，Redis Cluster和Redis Sentinel单元测试的shell脚本，这些测试是在tests 目录中实现的。

根目录中有以下重要目录：

src：包含用C编写的Redis实现。
tests：包含单元测试，在Tcl中实现。
deps：包含Redis使用的库。编译Redis所需的一切都在这个目录中; 您的系统只需要提供libcPOSIX兼容接口和C编译器。值得注意deps的是jemalloc，它包含一个副本，它是Linux下Redis的默认分配器。请注意，deps还有一些东西是从Redis项目开始的，但主存储库却没有antirez/redis。
还有一些目录，但它们对我们的目标不是很重要。我们将主要关注src包含Redis实现的地方，探索每个文件中的内容。文件被公开的顺序是遵循的逻辑顺序，以便逐步地公开不同的复杂层。

注意：最近Redis被重构了很多。函数名称和文件名已更改，因此您可能会发现此文档unstable更密切地反映了 分支。例如在Redis的3.0 server.c 和server.h文件被命名为redis.c和redis.h。但总体结构是一样的。请记住，所有新开发和拉取请求都应针对unstable分支执行。

server.h
理解程序如何工作的最简单方法是理解它使用的数据结构。所以我们将从Redis的主头文件开始，即server.h。

所有服务器配置和一般所有共享状态都在名为servertype 的全局结构中定义struct redisServer。该结构中的一些重要领域是：

server.db 是一个Redis数据库数组，其中存储了数据。
server.commands 是命令表。
server.clients 是连接到服务器的客户端的链接列表。
server.master 如果实例是副本，则是特殊客户端，即主服务器。
还有很多其他领域。大多数字段都直接在结构定义中注释。

另一个重要的Redis数据结构是定义客户端的数据结构。在过去它被称为redisClient，现在只是client。结构有很多领域，这里我们只展示主要的领域：

struct client {
    int fd;
    sds querybuf;
    int argc;
    robj **argv;
    redisDb *db;
    int flags;
    list *reply;
    char buf[PROTO_REPLY_CHUNK_BYTES];
    ... many other fields ...
}
客户端结构定义了连接的客户端：

该fd字段是客户端套接字文件描述符。
argc并argv使用客户端正在执行的命令填充，以便实现给定Redis命令的函数可以读取参数。
querybuf 累积来自客户端的请求，Redis服务器根据Redis协议解析请求，并通过调用客户端正在执行的命令的实现来执行。
reply并且buf是动态和静态缓冲区，用于累积服务器发送给客户端的回复。只要文件描述符可写，这些缓冲区就会逐渐写入套接字。
正如您在上面的客户端结构中所看到的，命令中的参数被描述为robj结构。以下是完整robj 结构，它定义了Redis对象：

typedef struct redisObject {
    unsigned type:4;
    unsigned encoding:4;
    unsigned lru:LRU_BITS; /* lru time (relative to server.lruclock) */
    int refcount;
    void *ptr;
} robj;
基本上，这个结构可以表示所有基本的Redis数据类型，如字符串，列表，集合，有序集等。有趣的是它有一个type字段，因此可以知道给定对象具有什么类型，以及a refcount，以便可以在多个地方引用同一个对象而无需多次分配它。最后，该ptr 字段指向对象的实际表示，即使对于相同类型，也可能会有所不同，具体取决于所encoding使用的。

Redis对象在Redis内部广泛使用，但为了避免间接访问的开销，最近在很多地方我们只使用未包装在Redis对象中的普通动态字符串。

server.c
这是Redis服务器的入口点，其中main()定义了该功能。以下是启动Redis服务器的最重要步骤。

initServerConfig()设置server结构的默认值。
initServer() 分配操作，设置监听套接字等所需的数据结构。
aeMain() 启动侦听新连接的事件循环。
事件循环定期调用两个特殊函数：

serverCron()定期调用（根据server.hz频率），并执行必须不时执行的任务，例如检查timedout客户端。
beforeSleep() 每次事件循环触发时都会调用，Redis会提供一些请求，并返回到事件循环中。
在server.c中，您可以找到处理Redis服务器其他重要事项的代码：

call() 用于在给定客户端的上下文中调用给定命令。
activeExpireCycle()通过EXPIRE命令处理设置时间的钥匙驱逐。
freeMemoryIfNeeded()当应该执行新的写命令时调用，但根据maxmemory指令Redis的内存不足。
全局变量redisCommandTable定义所有Redis命令，指定命令的名称，实现命令的函数，所需的参数数量以及每个命令的其他属性。
networking.c
此文件定义了客户端，主服务器和副本的所有I / O功能（在Redis中只是特殊客户端）：

createClient() 分配并初始化一个新客户端。
该addReply*()系列函数，以便将数据追加到所述客户端的结构，将被传输到客户端作为用于执行给定的命令的回应，使用命令实现。
writeToClient()将输出缓冲区中待处理的数据传输到客户端，并由可写事件处理程序 调用sendReplyToClient()。
readQueryFromClient()是可读事件处理程序，并将从客户端读取的数据累积到查询缓冲区中。
processInputBuffer()是根据Redis协议解析客户端查询缓冲区的入口点。一旦准备好处理命令，就调用processCommand()内部定义server.c的命令以实际执行命令。
freeClient() 解除分配，断开连接并删除客户端。
aof.c和rdb.c
您可以从名称中猜出这些文件为Redis实现RDB和AOF持久性。Redis使用基于fork() 系统调用的持久性模型，以创建具有Redis主线程相同（共享）内存内容的线程。此辅助线程转储磁盘上的内存内容。这用于rdb.c在磁盘上创建快照，以及aof.c在仅附加文件太大时执行AOF重写。

内部实现aof.c具有附加功能，以便实现允许命令在客户端执行时将命令附加到AOF文件中的API。

call()内部定义的函数server.c负责调用函数，而函数又将命令写入AOF。

db.c
某些Redis命令对特定数据类型进行操作，其他命令则是通用的。通用命令的示例是DEL和EXPIRE。它们专注于键，而不是它们的值。所有这些通用命令都在里面定义db.c。

此外，还db.c实现了一个API，以便在不直接访问内部数据结构的情况下对Redis数据集执行某些操作。

db.c在许多命令实现中使用的最重要的函数如下：

lookupKeyRead()并且lookupKeyWrite()用于获取指向与给定键关联的值的指针，或者NULL如果该键不存在。
dbAdd()及其更高级别的对应物setKey()在Redis数据库中创建一个新键。
dbDelete() 删除键及其关联值。
emptyDb() 删除整个单个数据库或定义的所有数据库。
该文件的其余部分实现了暴露给客户端的通用命令。

object.c
robj已经描述了定义Redis对象的结构。里面 object.c有所有在基本级别上使用Redis对象操作的函数，比如分配新对象的函数，处理引用计数等等。此文件中的显着功能：

incrRefcount()并decrRefCount()用于增加或减少对象引用计数。当它下降到0时，最终释放对象。
createObject()分配一个新对象。还有专门的函数来分配具有特定内容的字符串对象，例如createStringObjectFromLongLong()类似的功能。
该文件还实现了该OBJECT命令。

replication.c
这是Redis中最复杂的文件之一，建议只有在熟悉其余代码库之后才能接近它。在此文件中，实现了Redis的主副本角色。

此文件中最重要的功能之一是replicationFeedSlaves()将命令写入表示连接到主服务器的副本实例的客户端，以便副本可以获取客户端执行的写入：这样，他们的数据集将保持与主。

此文件还同时实现SYNC和PSYNC被以执行主机和副本之间的第一同步，还是继续断开后复制使用的命令。

其他C文件
t_hash.c，t_list.c，t_set.c，t_string.c，t_zset.c和t_stream.c包含Redis的数据类型的实现。它们实现了用于访问给定数据类型的API，以及用于这些数据类型的客户端命令实现。
ae.c 实现了Redis事件循环，它是一个独立的库，易于阅读和理解。
sds.c是Redis字符串库，请访问http://github.com/antirez/sds以获取更多信息。
anet.c 与内核公开的原始接口相比，它是一种以更简单的方式使用POSIX网络的库。
dict.c 是一个非阻塞哈希表的实现，它以递增方式重新进行。
scripting.c实现Lua脚本。它完全独立于Redis实现的其余部分，并且很容易理解您是否熟悉Lua API。
cluster.c实现Redis集群。在熟悉Redis代码库的其余部分之后，可能只有很好的读取。如果要阅读，请cluster.c务必阅读Redis Cluster规范。
解剖Redis命令
所有Redis命令都按以下方式定义：

void foobarCommand(client *c) {
    printf("%s",c->argv[1]->ptr); /* Do something with the argument. */
    addReply(c,shared.ok); /* Reply something to the client. */
}
然后server.c在命令表中引用该命令：

{"foobar",foobarCommand,2,"rtF",0,NULL,0,0,0,0,0},
在上面的示例2中，命令采用的参数数量"rtF"是命令标志，如命令表顶部注释中所述server.c。

在命令以某种方式操作之后，它返回对客户端的回复，通常使用addReply()或内部定义的类似函数networking.c。

Redis源代码中有大量的命令实现，可以作为实际命令实现的示例。编写一些玩具命令可以很好地熟悉代码库。

还有许多其他文件没有在这里描述，但它涵盖了一切都没用。我们希望帮助您完成第一步。最终你会在Redis代码库中找到自己的方式:-)

请享用！
