# java并发 cp4
## 原子操作类
原子操作类是基于cas算法来改变值的
### 1.AtomicLong类
#### 递增方法（原数+1）
```java
// 返回值为修改后值
long incrementAndGet();
// 返回值为修改前值
long getAndIncrement();
```
#### 递减方法（原数-1）
```java
// 返回值为修改后值
long decrementAndGet();
// 返回值为修改前原值
long getAndDecrement();
```
#### compareAndSet方法
```java
// cas算法，先比较再更改
boolean compareAndSet(long expect,long update);
```
### 2.原子操作类的性能缺陷
原子类让多个线程竞争同一个资源，为竞争到资源的线程调用cpu资源大量自旋，消耗性能  
所以有了新的类```LongAdder```,使用一个cell数组记录更改的差值，最后将所有差值相加实现数值的更改。  
### 3.LongAdder类
#### LongAdder的使用
使用方法较为简单。
```java
increment();//cells中找一个cell+1
sum();//将所有cells值相加并返回
sumThenReset();//获得所有cells和并将所有cells置零
longValue();//等价于sum()
```
#### 实验
采用两个线程，每个线程循环递增100w次，LongAdder的时间约为AtomicLong的一半

### 4.LongAccumulator类
#### LongAccumulator的使用
```java
//构造时需要传入一个累积方法与初始值，LongAdder初始值只能是0
public LongAccumulator(LongBinaryOperator accumulatorFunction,long identity);
//实例
LongAccumulator accumulator=new LongAccumulator(new LongBinaryOperator() {
@Override
      public long applyAsLong(long left, long right) {
                return left+right;
            }
        },0);
//累积,left为base基值，right为x
accumulate(long x);
```

