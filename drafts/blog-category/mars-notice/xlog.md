常规写文件的问题：

写文件的时候是写入系统的缓存页的，然后系统自己有策略（定时、内存比例不足、内存直接不足）将缓存页的数据写入磁盘。

这样有两次拷贝，而且时机不太可控，就有性能问题  



拷贝和时机的问题，可以先在内存缓存足够的压缩日志，减少写入的次数。  

然后Android gc 的问题，可以考虑c++ 实现代替  



但是内存缓存在进程异常退出（崩溃、手动杀死）时常常不能写入，会造成丢日志  



如此有mmap 这个现有 内存映射的方案可以解决上面的问题。特性：

- 内存不足、进程崩溃会回写
- 主动调用 同步方法  



然后是压缩加密的问题：

- 里面用的比较高级，先把LZ77（历史窗口和滑动窗口）进行短语压缩，再讲字符用assci 编码，然后编码后的数字用huffman 编码。