1、Web服务器的种类及优缺点，常用的有哪些
①目前主流的Web服务器有五种，Apache、Nginx、IIS、Lighttpd、Tomact，
②Apache是Apache软件基金会创建的一款Web服务器，具有模块化结构，但是基于多进程的HTTPserver,需要对每个用户请求创建一个子线程|进程来响应，当访问量较大时，会占用系统资源（主要是cpu和内存的资源），所以高并发的处理并不是Apache的强项。
③Nginx是一款轻量级的HTTPserver服务器，同时也是一款非常不错的反向代理，负载均衡服务器，Nginx以事件驱动，专为性能优化而开发，支持内核Poll模型，并且在高负载的情况下具有强大的稳定性，Nginx采用了分阶段资源分配技术，使的它的cpu使用率和内存占用率非常低，Nginx比Lighttpd稳定性要高。Nginx支持热部署，可以做到不间断的运行，而且可以在不间断服务的情况下进行版本升级。
④Lighttpd:是一款轻量级的Web服务器，占用内存小且CPU负荷低，是服务于静态内容的不错选择，从简单性能上来说Nginx比Lighttpd稳点性要高。
⑤IIS作为运行在Windows下Web服务器软件，可以对.NET、PHP进行解析，但是IIS只能运行在Windows操作系统上，而现在的绝大多数开源框架或软件最佳搭配环境为Linux/UNIX,这是由于IIS不具备开源软件低成本、易扩展等特点，从开源、投入、扩展、性能等方面考虑，IIS都不是优先考虑的对象。
⑥Tomcat服务器是一个免费的开源的Web应用服务器，属于轻量级的应用服务器，是开发和调试JSP程序的首选，Tomcat和IIS等其他WEB服务器一样，具有处理HTML页面的能力，但是处理静态页面能力较差。
⑦Lighttpd是个单进程模型的Web服务器，内存使用量非常小，Nginx在内存分配方面表现良好，它使用多线程进行处理请求，这使得多个线程之间可以共享内存资源，从而使得内存使用了大大减少，此外Nginx使用分段内存分配策略，按需进行分配及时释放，总体占用内存很小，可支持较大的并发连接，Apache在运行时使用较大的内存，是多进程模型，使用基于内存池策略的内存管理方法，这使得运行开始时便一次性申请大量的内存作为内存池，可以直接在内存池中直接获取到，因此不适合大并发量的情况。
⑧Nginx内部的HTTP状态跃迁是比较隐性的，它是通过一系列的handler赋值来实现的，如果要追踪一次HTTP请求的处理过程是比较麻烦的，而Lighttpd中是显性的状态跃迁，Nginx完全不支持动态库so,它的所有模块都是需要静态编译，且不能动态加载，要加载那些模块，模块按照怎样的顺序执行，都是在编译期configure指定的，如果要调整，也只能重新编译一遍，无法通过该配置来实现，而Lighttpd是支持动态库so,并且可以在配置文件中调整各个模块的先后顺序，在某些应用场景下，调整模块的先后顺序是很有用的，条件配置语法的灵活性,Lighttpd比Nginx好点，Lighttpd和Nginx都是基于epoll/kqueue/select全异步事件模型，可以维持大量的连接，但是Nginx是多线程模式，而Lighttpd是单线程模型。
⑨常用的Web服务器有Tomcat、Nginx、Apache
2、请讲讲负载均衡
①负载均衡分为两种一种是硬件负载均衡设备，另外一种是软件负载均衡，而从经济上来说，目前的公司主流的是倾向于软件的负载均衡来处理高请求、高并发，硬件负载均衡分为F5 BIG-IP，简称为本地流量管理器，可做4~7层负载均衡，F5 BIG-IP作为负载均衡器的作用：1）、F5 BIG-IP提供了12中算法将所有的流量均衡到各个服务器上，
而对用户而言它就是一台虚拟服务器，2）F5 BIG-IP是可以确认应用程序能否对请求返回对应的数据（当一台服务器死机了，它会去检查并表示该服务器的状态，并且不再向该服务器分发任务，而且死机的服务器一旦恢复正常，F5 BIG-IP会自动将该服务器加入分发的队列），3）F5 BIG-IP具有动态会话session保持功能，4）F5 BIG-IP的iRULES功能可以实现HTTP内容过滤，根据不同的域名、URL,将访问请求传送到不同的服务器。
LVS为目前阿里巴巴公司的主要的负载均衡设备，特点：具有很强的抗负载能力，配置简单，工作稳定，应用范围比较广，LVS的三种工作模式，它们分别是VS/NAT、VS/TUN、VS/DR
软件的负载均衡：Nginx/HaProxy,Nginx可以作为反向代理服务器，配置非常简单，主要是根据头部信息执行负载任务，可对http应用执行分流策略，Nginx对网络的依赖性较小，HaProxy是一个适用于高可用性环境的TCP/UDP反向代理和负载均衡开源软件，支持双机热备，支持虚拟主机，支持健康检查（通过patch可以支持ECV）,同时还提供直观的监控页面，可以清晰的实时的监控服务集群的运行状况，同时还支持Linux内核中的System EPoll 大幅提高了网络的I/O性能。
HaProxy的特点：1）、能够补充Nginx的特点，比如Session的保持，Cookie的引导工作。2）、同时还支持URL检测，如果后端的服务器出了问题，他可以通过URL检测，及时发现进行处理。3）、从效率上来讲，HAProxy会比Nginx有更出色的负载均衡速度。4）HaProxy可以对Mysql进行读操作，对后端的Mysql的节点检测和负载均衡。
可以从访问量上来说、资金预算上、技术储备上等综合考虑，从节约角度来说，前期可以使用Nginx或HaProxy单点负载均衡，后期各方面条件成熟以后再使用LVS或F5.
3、什么是缓存
缓存就是将程序上或系统上经常要调用的对象存在内存中，不必再去创建重复的实例，节约时间和空间，提高运行效率，缓存分为两个部分，文件缓存和数据缓存，而文件缓存指的是静态文件，这些可以通过CDN内容分发网络分发处理，从而提高网站的响应速度，还可以通过varnish(开源的HTTP网络加速器）或squid缓存服务器来处理，同样也可以提高响应速度，数据缓存是指对某些数据进行缓存，这些数据可以是经常要调用的数据对象等，常用的缓存数据的工具有redis,memcache.但是redis是具有持久化的。
客户端缓存：现在越来越大的数据（图片、flash、css、脚本）被嵌入到页面中，当我们访问它们的时候势必要做许多次的http请求，其实，我们可以通过设置网页头部报文中时间属性来缓存这些文件，缓存时间是通过头部报文来指定特定类型的文件在浏览器中缓存时间，这样在下次访问时，可以快速的访问，提高了页面响应的速度。
CDN加速：内容分发网络，为了解决Internet网络拥挤的状况，提高用户访问网站的响应速度，而一些公司选择自建CDN提高网络的响应速度，而中小型企业可以使用第三方的插件，如蓝汛，快网等。
静态文件缓存一般都是通过varnish（开源的HTTP加速器）和squid缓存服务器来实现，它在Web服务器前面添加了一层缓存，可以缓存图片和js格式的文件，如同redis的缓存机制，但是redis的缓存是分为2层，而静态文件缓存分为1层，如果文件在缓存中没有找到，就会去web服务器中去寻找。
redis的持久化方式有几种：两种，一种是快照，它是一种半持久耐用模式，不时的将数据集以异步方式从内存中以RDB格式写入磁盘，另一种是AOP,它是一种只追加的日志类型，Redis很大程度上弥补了Memcache这类key-value存储的不足，在部分场合可以对关系型数据库起到很好的补充作用，Redis也提供了Python、Ruby、Erlang、和PHP的API,且redis支持主从同步，读写分离。
4、数据存储：
      关系型数据库：以Oracle、mysql为代表
      key-value数据库，以Redis和Memcache为代表
      文档型数据库：以MongoDB为代表
      分布式的面向列的数据库，以HBase为代表
Mysql的存储引擎为MyISAM、InnoDB,其中InnoDB提供事物安全表，其他的存储引擎都是非事物安全表，MyISAM是mysql的默认存储引擎，支持事务，也不支持外键，但其访问速度快，InnoDB存储引擎具有提交、回滚、和崩溃恢复能力的事物安全，但是比起MyISAM存储引擎，InnoDB写的处理效率要慢些，并且会占用更多的磁盘空间以保留数据和索引。
内存型数据库的代表为Redis，MongoDB等，它强调高并发，但在事物安全方面不如关系型数据库，MongoDB以多线程的方式通信，主线程监听新的连接，连接后，启动新的线程进行数据的处理操作，在MongoDB中使用了操作系统底层提供的内存映射机制，即mmap,MongoDB调用了mmap把磁盘中数据映射到内存中的，所以必须有一个机制地刷数据到磁盘才能保证可靠性。
分布式数据库：HBase基于列式存储，存储高效且低IO,适用于大数据，同列数据具有很高的相似度，可以通过查询一列，从而大大降低了IO,会增加压缩效率。
Mysql分布式集群，理论上是具有很强大的存储能力，可用性和性能非常高，但是缺点就是维护成本特别高，Mysql Replication 即Mysql的主从复制，主服务器将sql记录到日志文件中，从服务器读取日志文件后将SQL应用到自身，数据库的分离即数据库的读写分离.
文件存储：中小型公司一般可以用NFS网络文件系统进行存储图片等静态文件，在大型平台上，一般使用的是HDFS（分布式文件系统）/FASTDFS(开源的轻量级的分布式文件系统）等，网络存储设备的基本模式都是这三种：SAN(存储区域网络）、DAS(直连式存储）、NAS(网络附属存储）。
说说Int与Integer的区别:①：Integer是Int的包装类，而Int是java中的基本数据类型，②：Integer变量必须被实例化之后才能使用，而int不需要，③：Integer实际是对象的引用，当new一个Integer时，实际上是生成了一个指针指向此对象，而int是存储数据值，④：Integer默认值为null,而Int的默认值为0。
为什么使用包装类：java是一种面向对象的语言，所有操作都是基于对象的，Object类是java中所有对象的基础，所有的java中的类都有一个共同的始祖就是Object类，Object可以表示任意数据类型，但java中的一些基本数据类型，并不是对象，没有对象的操作，如何将对象类型与基本数据类型联系起来呢，这是就需要一个过渡类型数据，就被称为包装类。相当于将基本数据类型包装起来，使的它具有对象的特征，并且为其添加了属性和方法，丰富了基本数据类型的操作。包装类有Double,Integer,Float,Long,Byte,Short,Character,Boolean。
为什么要使用linux开发：①开源：任何人都可以查看代码并研究来判定是否有一些潜在的能够造成安全风险的缺陷，②多用户、多线程、多任务：Linux同时支持多个用户，每个用户对自己的文件设备都有着特殊的权利，能够保证各用户之间互不干扰，每个用户可以同时开启多个任务和多个线程，多个线程同时工作，提高效率，稳定性和高效性，安全性和SeLinux、性能优势。
注意：构造器不能被继承所以不能被重写，能被重载，两个对象之间的hashcode的值相同，对象不一定相等，但是两个对象相等相同，则两个对象的hashcode一定相等，如果hash码频繁的冲突会导致存取性能急剧下降，在set集合中新增元素的能力会大大降低，String类被final修饰的，所以不能被继承。在java中对象被方法当成参数去调用，对象的属性改变了，但是原本的对象属性是没有发生变化的，方法中对象的调用只是原本对象的一个副本，参数的值就是对象引用的值，如果要改变原本对象的值将会是Java代码变得臃肿，耦合度太高。不利开发。StringBuilder和StringBuffer都是可变长字符串，但是StringBuilder在单线程下，底层的任何方法都没有被synchronized修饰，因此它的效率比StringBuffer要高，但是安全性要比StringBuffer要低。方法的重载是发生在编译时的多态性，而方法的重写是发生在运行时的多态性。一个内部类对象是可以访问外部类对象的任何成员的
为什么不能根据返回类型区分重载？
由于方法的重载是编译时的多态特征，而重写是运行时的多态特征，而返回值只能作为函数运行之后的一种状态，它是保持方法的调用者与被调用这之间的通信的关键，并不能作为某个方法的标识，重载发生在一个类中，同名的方法如果有不同的参数列表（参数类型，参数个数，参数顺序）则视为重载，而重写发生在有继承关系网中的子类与父类中，重写要求子类被重写的方法与父类被重写的方法有相同的返回类型，比父类被重写的方法更好访问，不能比父类被重写的方法声明更多的异常。
描述一下JVM加载class文件的原理机制：Jvm中类的装载是由类加载器与它的子类来实现的，java中类加载器是一个重要的运行时系统组件，它负责在运行时查找和装入类文件中的类，由于java的跨平台性，经过编译的java源程序并不是一个可执行的程序，而是一个或多个类文件，当java程序需要某个类时，Jvm会确保这个类已经被加载、连接（验证、准备、解析）和初始化，类的加载是指把类的class文件中的数据读取到内存中，通常是创建一个字节数组来读取.class文件，然后产生与所加载类对应的Class对象，加载完成后，Class对象还不完整，所以此时的类是不能使用的，当类被加载完成后进入连接 状态时，这一阶段包含（验证、准备（为静态变量分配内存并设置默认的初始值）、解析（将符号引用替换为直接引用）），最后Jvm对类进行初始化，包含:1、如果这个类存在直接的父类，并且这个类没有被初始化，那么先初始化父类，2）如果这个类中存在初始化语句，就依次执行这些初始化语句，类的加载是由类加载器完成的，类加载器包含根加载器、扩展加载器、系统加载器和用户自定义类加载器，类的加载首先请求父类的加载器加载，父类的加载器无能为力时才由其子类加载器自行加载，JVM不会向Java程序提供对BootStrap（Jvm负责基础核心类库的加载）的引用。
char型变量能否存储一个中文汉字？
char类型可以存储一个中文，因为java中使用的编码是Unicode（UTF-8）编码，一个char类型占2个字节，所以放一个中文没问题。
抽象类与接口的异同：抽象类和接口都是不能被实例化的，但可以定义抽象类和接口类型的引用，一个类如果继承了某个抽象类或实现了某个接口，抽象类和接口中的抽象方法必须进行实现，否则该类仍然需要被声明为抽象类，接口比抽象类更加抽象，抽象类中可以有构造器，抽象方法和具体方法而接口中是不能存在构造器和具体方法，抽象类中的成员可以声明为private、public、protected、default，而接口中默认是public,接口中的变量为常量，而抽象类中可以有变量，有抽象方法的类必须申明为抽象类，而抽象类未必有抽象方法。
静态嵌套类与内部类的不同？
java中非静态的内部类对象的创建要依赖其外部类对象，静态内部类它可以不依赖与外部类的实例而实例化，而通常的内部类需要在外部类被实例化后才能被实例化。
java中存在内存泄漏吗？
理论是不存在的，由于java中存在GC垃圾处理器（垃圾回收机制）,但是现实中存在这无用但可达的对象，这些对象不能被GC回收，这就造成了内存的泄漏，
静态变量和成员变量的区别：静态变量是属于类的，静态变量在内存中有且仅有一个拷贝，实例变量必须依存于某个实例，需要先创建对象，然后通过对象才能访问到它，成员变量是属于对象的，静态变量可以实现多个对象共享内存。
final关键字的用法？：被final修饰的类不能被继承，修饰的方法不能被重写，修饰变量，该变量被赋值过以后，不能再次被赋值。
怎么将Gb2312编码的字符串转换为ISO-8859-1或UTF-8编码的字符串？
String str = "你好";
String str1 = new String(str.getBytes("GB2312"),"ISO-8859-1");
String str2 = new String(Str.getBytes("GB2312")，“UTF-8”);
java与javaScript的区别？
1、java是面向对象编程的，而JavaScript是解释编译，2、java变量是强类型的，而JavaScript是弱类型的，代码格式不一样，且java是一种静态语言，而javaScript是中动态语言，javaScript是中脚本语言。
什么时候使用Java8中的断言？当希望不满足某些条件时，阻止代码的执行，可以考虑使用断言来阻止它。
Error和Exception的区别：Error是一种系统级的错误和程序不必处理的异常，一般都是恢复不是不可能但很困难的一种严重问题，比如内存溢出，不能指望程序能处理这样的问题，而Exception表示需要捕获或需要程序处理的异常，是一种设计或实现问题，它表示程序运行正常，从不会发生的情况。
try{}里有一个 return 语句，那么紧跟在这个 try 后的 finally{}里的代码会不会被执行，什么时候被执行，在 return 前还是后?
会执行，但是是在方法返回调用者之间执行。
注意：try语句可以嵌套，每当遇到一个try语句，异常的结构就会被放入异常栈中，知道所有的try语句都完成，但是如果下一级的try语句没有对某种异常进行处理，异常栈就会执行出栈操作，直到遇到有处理这样异常的try语句或者最终将异常抛给jvm。
运行时异常与受检异常的区别:运行时异常表示虚拟机的通常操作中可能遇到的异常，是一种常见的运行错误，而受检异常是与程序运行的上下文环境有关，即使程序无误，仍然可能由于使用的问题而引发，java编译器要求方法必须声明抛出可能发生的受检异常，
对异常处理的原则：1、保持异常的原子性，2、优先使用标准的异常，对可以恢复的异常使用受检异常，对编程错误使用运行时异常，3、避免不必要的使用受检异常（可以通过一些状态检测手段来避免异常的发生），不要在catch中忽略掉捕获到的异常，
常见的异常？
ArithmeticException（算术异常）- ClassCastException （类转换异常）- IllegalArgumentException （非法参数异常）- IndexOutOfBoundsException （下标越界异常- NullPointerException （空指针异常）- SecurityException （安全异常），ClassNotFoundException(类未找到异常），SQLException（sql语句异常），finally与finalize的区别：通常放在try...catch...的后面构造总是执行代码块，这就意味着程序无论正常执行，还是发生异常，这里的代码只要JVM不关闭都能执行，可以将释放外部资源的代码写在finally块中，finalize:Object中定义的方法，java中允许使用finalize()方法在垃圾处理器中将对象从内存中清除出去之前，做必要的清理工作，这个方法是有垃圾回收器，在销毁对象时调用的，通过重写finalize()方法可以整理系统资源，或者执行其他的清理工作，
注意：try...catch...中异常（能使用父类型的地方一定能使用子类型），即catch中的异常能够抓住try中的异常。
Collection与Collections的区别：Collection是一个接口，它是set、List等容器的父接口，Collections是一个工具类，提供了一系列的静态方法来辅助容器操作，这些方法包含对容器的搜索、排序、线程安全化等操作。
@PathVariable 可以将URL中占位符参数绑定到控制器处理方法的入参中，Url中的{xxx}占位符可以通过@PathVariable("xxx")绑定到方法的入参中。@Param注解是给参数命名，参数命名后就能根据名字得到参数值，正确的将参数传入sql语句中，而@Param注解括号中的参数名称为sql语句中的数据，用括号中的参数名称代替sql语句中的参数。
将普通类加入Ioc容器中定义Bean的注解：@Component、@Service、@Controller、@Bean、@Repository，@Qualifier注解定义工厂名称的方法，@Scope注解定义该bean作用域的范围，@Configuration注解是将这个类作为创建各种bean的工厂。CGLIB简称动态代理：而在spring框架中@Component注解，在使用@Component注解的类中不会强制使用CGLIB代理去拦截方法和属性，而在@Configuration注解类中，则会使用CGLIB代理去调用@Bean标注的方法并返回对象的引用，在@Configuration注解中使用@Bean也可以防止同一个@Bean方法被意外调用多次时而产生细微的难以排除的错误。
@AutoWired注解可用于为类的属性和方法进行注值，默认情况下，其依赖的对象必须存在（bean可用），如果改变这种默认方式，可以设置其required属性为false,@Autowired注解默认按类型装配，如果容器中包含多个同一类型的bean,那么启动容器时会报找不到指定类型bean的异常，解决办法是通过@Qualifier注解进行限定，指定注入bean的名称.
throw、throws、throwabled的区别：throw是方法中抛出了一个异常，只能通过new throw来创建异常，throws是定义在方法处或者类定义处声明该类或方法可能产生的异常就像抛出的异常，需要调用处去处理。throwable是所有错误和异常的超类，所以当不知道要产生的异常是什么类型时，直接throws  throwable即可。
Collections是java.util下的类，它包含有各种有关集合操作的静态方法，而Collection是java.util下的接口，它是各种集合机构的父接口。
列出自己常用的jdk中的数据结构：线性表，链表，哈希表。
java反射机制的作用：1、在运行时判断任意一个对象的所属的类，2、在运行时构造任意一个类的对象，3、在运行时判断任意一个类的所具有的成员变量和方法，4、在运行时调用任意一个对象的方法。
什么是java序列化，而什么时候用到java序列化？
序列化是一种用来处理对象流的机制，所谓对象流就是将对象的内容进行流化，可以对流化的对象进行读写操作，也可将流化后的对象传输与网络之间，序列化是为了解决对对象流进行读写操作时所引发的问题。序列化的实现是通过类实现Serializable接口,而实现Serializable是为了标注该对象是可被序列化的，然后使用一个输出流（FileOutputStream）来构造一个ObjectOutPutStream(对象流）对象，使用ObjectOutPutStream对象的writeObject(Obj obj)方法就可以将参数为obj的对象写出（即保存其状态），要恢复的话则用输入流。
java数据库编程的基本流程是什么？
1、注册驱动，2、建立连接，3、创建Statement,4、执行sql语句，5、处理结果集，6、关闭连接。
Statement、PreparedStatement,CallableStatment的区别？
1、Statement是PreparedStatement/CallableStatement的父类，2、Statement是直接发送sql语句到数据库的，事先没有进行预编译，PreparedStatement会将sql进行预编译，当sql语句进行重复执行时，数据库会调用以前预编译好的sql语句，所以PreparedStatement在性能方面会更好，3、PreparedStatement在执行sql时,对传入的参数可以进行强制的类型转换，以保证数据格式与底层数据库格式一致。4、CallableStatmemt适用于存储过程的查询表达语句。
PreparedStatement与Statement的区别：1、PreparedStatement可以写参数化查询，比Statement能获得更好的性能，2、对于PreparedStatement来说，数据库可以使用已经编译过的及定义好的执行计划，这种预处理语句查询比普通查询速度更快，3、PreparedStatement可以阻止常见的sql注入式攻击，Preparedstatement可以写动态sql语句，Preparedstatement与java.sql.Connection对象是关联的，一旦关闭了connection,Preparedstatement是无法使用的，PreparedStatement不支持预编译的sql查询的JDBC的驱动，在调用connection.preparedStatement(sql)的时候，它不会把sql查询语句发送到数据库进行预处理，而是等待执行查询操作的时候（调用executeQuery()方法时）才把查询语句发送给数据库，这种情况和使用Statement是一样的。
说说连接池吧？连接池放了N个Connection对象，本质上是放在内存中，在内存中划出一块缓存内存，应用程序每次从池中获取Connection对象，而不是直接从数据里获得，这样不占用服务器的内存资源，如果不使用连接池会出现以下的情况：占用服务器的内存资源，导致服务器的运行速度变慢，应用连接池的三种方式，自定义连接池，使用第三方连接池，使用服务器自带的连接池，连接池比一般的直接连接更有优越性，因为它提高了性能的同时，还保存了宝贵的资源，在整个应用程序的使用过程中，当中重复的打开导致性能的下降，而池连接只在服务器启动时打开一次，从而消除了这种性能问题。连接池主要考虑的就是性能，每次获取连接和释放连接都有很大的工作量，会对性能有很大的影响，而对资源来说启的就是反作用了，而不是直接从数据里获得，这样不占服务器的内存资源，所以一般要建立连接池，而连接的数量要适当，不能太大，太大会过度消耗资源，所以这时候我们要考虑的就是内存与资源的平衡，连接池就是为了避免重复多次的打开数据库连接而造成的性能的下降和系统资源的浪费。
Tcp/ip是个协议组，可分为三个层次：网络层，传输层和应用层，在网络层有IP协议，ICMP协议，ARP协议，RARP协议，BOOTP协议，在传输层有TCP/ip协议，在应用层有FTP,HTTP,TELNET,SMTP,DNS等协议，因此，http本身就是一个协议，是从Web服务器传输超文本到本地浏览器的传输协议。Http协议是建立在请求/响应模型上的，首先由客户建立一条与服务器的tcp连接，并发送一个请求到服务器，请求中包含请求方法、URI、协议版本及相关的MIME样式的消息，服务器响应一个状态行，包含消息的协议版本，一个成功和失败码以及相关的MIME式样的消息。虽然HTTP本身就是一种协议，但是还是基于TCP的。
TCP与UDP的区别：TCP是面向连接的，UDP是无连接的，及发送数据之前不需要连接，TCP提供可靠的服务，安全，UDP是提供不安全的服务，无法保证数据安全，TCP面向字节流（实际上可以将TCP数据看成一连串无结构的字节流），而UDP面向报文的，UDP没有拥塞控制，因此网络出现拥塞现象时，不会使源主机的发送速率降低，（对实时应用也是很有用的，如IP电话，实时视屏会议等），每一条TCP连接只是点对点的，而UDP是支持一对一、一对多、多对一、多对多的交互通信，TCP首部开销是20字节，而UDP是8个字节，TCP的逻辑通信信道是全双工的可靠信道，而UDP是不可靠信道。
HTTP特性(与ajax、websocket做比较）:1、简单快速：客户向服务器请求服务时，只需传送方式和路径，请求方法常用的有GET、HEAD、POST,每种方法规定了客户与服务器联系的类型不同，由于HTTP协议比较简单，使得HTTP服务器的程序规模小，因而通信的速度非常快。2、灵活：HTTP允许传输任何类型的数据对象，正在传输的类型由Contente-Type加以标记，3、无连接：无连接的含义就是限制每次连接只处理一个请求，服务器处理完客户的请求，并收到客户的应答后，即断开连接，采用这种方式可以节省传输时间。4、无状态，HTTP协议是无状态协议，无状态是指协议对于事物处理没有记忆能力，缺少状态意味着如果后续需要前面的信息，则它必须重传，这样可能导致每次连接传输的数据量增大，另一方面，在服务器不需要先前消息时它的应答就较快。支持B/S以及C/S模式。
进程与线程的区别？1、进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动，进程是系统资源进行调配的一个独立单位，线程是进程的一个实体，是cpu调度和分派的基本单位，它比进程更小的能独立运行的基本单位，线程基本上不拥有任何系统资源，只拥有一点在运行时必不可少的资源（如程序计时器，一组寄存器和栈），但是它可以与同属一个进程的其他线程共享进程所拥有的的全部资源。线程与进程的关系就是一个线程可以创建和撤销另一个进程，同一进程中的多个线程之间可以并发执行，相对进程而言，线程是一个更加接近于执行实体的概念，它可以去同一进程中的其他线程共享数据，但拥有自己的栈空间，拥有独立的执行序列，进程与线程的区别：一个程序至少有一个进程，而一个进程至少有一个线程，线程的划分尺度小于进程，使得多线程程序的并发性高，进程在执行时拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率，每个独立的线程都是有一个独立的程序运行入口，顺序执行序列和程序的出口，但是线程不能独立的执行，而必须依存于应用程序中，由应用程序提供多个线程执行控制，线程的开销小，但不利于资源的管理和保护，而进程是恰恰相反的，线程适用于在smp机器上执行，而进程可以跨机器迁移。进程间相互独立，同一进程的各线程间共享，某进程内的线程其他进程不可见，进程间通信IPC,线程间可以直接读写进程数据段（如全局变量）来进行通信--需要进程同步和互斥手段的辅助，以保证数据的一致性，调度和切换：线程上下文切换比进程上下文切换要快的多。进程不是一个可执行的实体。
线程的生命周期：初始化状态，可运行状态，运行状态，阻塞状态，死亡/终止状态。新建、就绪、运行、阻塞、终止。新建（初始化状态）即是线程被创建了，但是在操作系统层面没有创建，只是在java中对象被new出来了，而线程调用了start方法之后，该线程处于就绪状态，java虚拟机会为其创建方法栈和程序计时器，一组寄存器，处于这种状态的线程并没有进行运行状态，只是表示这个线程已经可以运行了，但是什么时候运行取决于jvm里线程调度器的调度。运行状态：如果处于就绪状态的获得了cpu,开始执行run()方法的线程执行体就是处于运行状态，进程进入阻塞状态的方式：1、线程调用sleep方法主动放弃所占用的处理器资源，2、线程调用了阻塞式的IO方法，在该方法返回之前，该线程被阻塞，3、线程在等待某个通知（notify）,4、线程试图获得一个同步监视器，但该同步监视器正被其他线程锁持有，程序调用了线程的suspend方法将该线程挂起，不过这种方法容易造成死锁。让线程进入就绪状态的方式：1、调用sleep方法的线程经过了指定时间，2、线程调用阻塞式IO方法返回，3、线程成功的获得了同步监视器，4、线程正在等待某个通知时，其他线程发出一个通知，5、然后挂起的线程被调用了resume恢复方法。什么时候进入终止状态：run()方法执行完成，线程正常结束，线程抛出了一个未捕获了Exception和Error。直接调用该线程的stop()方法来结束该线程，该方法容易造成死锁，不推荐使用。
