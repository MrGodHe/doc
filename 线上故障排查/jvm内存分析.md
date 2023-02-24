

切换到 JDK_HOME/bin/，执行以下命令：./jmap -dump:format=b,file=heap.hprof pid



内存分析工具：

eclipse Memory Analyzer (MAT)





在一个region中可能会引入到其他的region，为了避免不需要的全局扫描，在每个region中都对应一个Remembered Set（记忆集），使用CarTable记录每个region区相互引用的关系。
　　维护解决多个区间引用关系记录，避免扫描所有的区间。 

　　G1收集器回收过程分为三个环节：

　　1）年轻代GC（Young GC ）
　　　　当所有eden区满的情况下，采用并行多线程的方式，使用标记复制算法，基于stw暂定所有用户线程，将存活对象复制到S区，清空所有eden区；当S区对象的年龄达到阈值，使用标记复制算法，将对象移动到老年代。

　　2）混合回收（Mixed GC ）
　　　　越来越多的对象晋升到老年代，当堆内存使用达到一定值（默认45%）时，开始老年代并发标记过程，标记完成马上开始混合回收过程。为了避免堆内存耗尽，会触发混合收集器，即Mixed GC。

        young gc（新生代） 一> young gc + concurrent mark（新生代+并发标记） 一> Mixed GC（混合回收）顺序，进行垃圾回收
