# Actor系统

Actor是封装状态和行为的对象，他们唯一的通讯方式是交换消息——把消息存放在接收方的邮箱里。从某种意义上来说，actor是面向对象最严格的形式，不过最好把它们比作人：在使用actor来对解决方案建模时，把actor想象成一群人，把子任务分配给他们，将他们的功能整理成一个有组织的结构，考虑如何将失败逐级上传（好在我们并不需要真正跟人打交道，这样我们就不需要关心他们的情绪状态和道德问题）。这个结果就可以作为软件实现的思维框架。

.. note::

   An ActorSystem is a heavyweight structure that will allocate 1…N Threads,
   so create one per logical application.


注意
一个Actor系统是一个很重的结构，它会分配一到N个线程，所以对每一个逻辑应用创建一个就够了。

###树形结构

象一个经济组织一样，actor自然形成树形结构。程序中负责某一功能的actor，可能需要把它的任务分拆成更小的、更易管理的部分。为此它启动子Actor并监督它们。虽然[后面章节](http://doc.akka.io/docs/akka/2.3.6/general/supervision.html#supervision)会解释监督机制的细节，我们会集中在这一节里介绍其中的根本思想，唯一需要了解的前提是每个actor有且仅有一个监管者，就是创建它的那个actor。

Actor 系统的精髓在于任务被分拆开来并进行委托，直到任务小到可以被完整地进行处理。这样做不仅使任务本身被清晰地划分出结构，而且最终的actor也能按照它们“应该处理的消息类型”，“如何完成正常流程的处理”以及“失败流程应如何处理”来进行解析。如果一个actor对某种状况无法进行处理，它会发送相应的失败消息给它的监管者请求帮助。这样的递归结构使得失败能够在正确的层次进行处理。

可以将这与分层的设计方法进行比较。分层的设计方法最终很容易形成防护性编程，以防止任何失败被泄露出来。把问题交由正确的人处理会是比将所有的事情“藏在深处”更好的解决方案。












