# 对象池简介

## 1.什么是对象池
 单例模式：全局只有一个对象，供所有线程使用；
 
 原型模式：每次使用对象时都新建，用完之后释放；
 
 对象池模式：一个池子，里面维护多个对象，调用方从对象池中借取对象，使用后归还给对象池。
 
## 2.对象池利弊
缺点：
1.增加复杂度，正常对象的生命周期由使用者和JVM维护，即使用者创建（JVM分配内存） → 销毁 → GC，
  对象池对象的生命周期更复杂，需要引入多个状态。 IDLE、ALLOCATED、EVICTION、VALIDATION、INVALID、RETURNING；
  
2.线程竞争，多个线程共同调用对象池，可能会造成锁竞争，影响程序性能；

3.参数调整，池往往具有多个参数，需要不断调整，找到合适的配置

优点：

1.对调用方友好，只需要“借取”、“归还”；

2.实现对象复用，提高性能
## 3.使用场景：常见的线程池、连接池
创建对象的时间成本和空间成本很高，时间成本：创建对象需要网络通信，如数据库连接；空间成本：对象占用的内存比较大，如线程

## common-pool2简介

### 1.核心模块

ObjectPool:提供所有对象的存取管理，基本实现类BaseObjectPool，默认实现类包括GenericObjectPool、ProsiedObjectPool、SoftReferenceObjectPool等；

PooledObject：池化的对象，对象的一个包装，加上对象的状态、创建时间等，默认实现类DefaultPooledObject、PooledSoftReference(线程安全)；

PooledObjectFactory：工厂类，负责池化对象的创建、初始化、状态的销毁、状态的验证，默认实现类BasePooledObjectFactory

GenericObjectPoolConfig：参数配置类，负责配置基本参数lifo对象池存储空闲对象是使用的LinkedBlockingDeque，由common-pool2重写的双向队列，
  默认为true，表示FIFO获取对象；false，表示FILO获取对象、maxTotal、maxIdle、minIdle等
  
### 2.核心流程(GenericObjectPool示例)
1.BorrowObject

![Image_text](https://img-blog.csdn.net/20170812102738667?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYW1vbjE5OTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

2.ReturnObject(返回对象)

![Image_text](https://img-blog.csdn.net/20170812102756419?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYW1vbjE5OTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast）

 
  
