# 关于HashCode 和 equals() 面试题

## HashCode和Equals()功能一样，在Java中都是用来比较两个对象是否相等一致

## HashCode和equals()区别
#### equals()
1. 绝对可靠，但是带来的问题是效率
2. equals()相等，那么Hash一定相等
### HashCode
1. 并不完全可靠，有时候HashCode会冲突，不同的对象，一样的HashCode，所以需要再使用equals()

## 使用事项
1. 在使用的过程中，如果HashCode不同，那就一定不同，如果HashCode一样，那么需要再使用equals()对比，提高效率。
2.  HashCode和Equals()都是Object中的方法，HashCode返回的是对象地址，同一个类，不同的地址，显然不是我们想要的，所以要重写 HashCode和Equals()

## 阿里巴巴开发规范
1. 只要重写 equals，就必须重写 hashCode；

2. 因为 Set 存储的是不重复的对象，依据 hashCode 和 equals 进行判断，所以 Set 存储的对象必须重写这两个方法；

3. 如果自定义对象做为 Map 的键，那么必须重写 hashCode 和 equals；

4. String 重写了 hashCode 和 equals 方法，所以我们可以非常愉快地使用 String 对象作为 key 来使用；


## 如何生成HashCode()
```java

    public static int hashCode(Object a[]) {
        if (a == null)
            return 0;

        int result = 1;

        for (Object element : a)
            result = 31 * result + (element == null ? 0 : element.hashCode());

        return result;
    }
```

