# Effective Java



## 第四章 类和接口

### 第 17 条：使可变性最小化

- 不可变对象本质上是线程安全的，它们不要求同步。
- BigInteger 和 BitSet
- BigInteger 和 BigDecimal：BigInteger 实现了任意精度的整数运算，BigDecimal 实现了任意精度的浮点数运算。

### 第 18 条：复合优先于继承

- 只有当两者之间确实存在“ is-a ”关系的时候，类 B 才应该扩展类 A，否则 B 应该包含

  A 的一个私有实例，并且暴露一个较小的、较简单的 API。

  JDK 中如`Stack<E> extends Vector<E>`，`Properties extends Hashtable<Object,Object>`都违反该原则，采用复合更优。

