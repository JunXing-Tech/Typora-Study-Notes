[toc]

### Stream流

#### 使用案例

```java
/*
    创建集合添加元素，完成以下需求：
    1.把所有以“张”开头的元素存储到新集合中
    2.把“张”开头的，长度为3的元素再存储到新集合中
    3.遍历打印最终结果
*/
ArrayList<String> list1 = new ArrayList<>();
1ist1.add("张无忌")；
1ist1.add("周芷若")；
1ist1.add("赵敏")；
1ist1.add("张强")；
1ist1.add("张三丰")；

list1.stream().filter(name -> name.startswith("张")).filter(name -> name.length() == 3).forEach(name -> System.out.println(name));
```

Stream流的作用

* 结合了Lambda表达式，简化集合、数组的操作

Stream流的使用步骤

1. 先得到一条Stream流（流水线），并把数据放上去

| 获取方式     | 方法名                                        | 说明                     |
| ------------ | --------------------------------------------- | ------------------------ |
| 单列集合     | default Stream<E> stream()                    | Collection中的默认方法   |
| 双列集合     | 无                                            | 无法直接使用stream流     |
| 数组         | public static <T> Stream<T> stream(T[] array) | Arrays工具类中的静态方法 |
| 一堆零散数据 | public static<T> Stream<T> of(T...values)     | Stream接口中的静态方法   |

注：双列集合需要先使用`keySet()`或者 `entrySet()`先转成单列集合

```java
//1.单列集合获取Stream流
ArrayList<String> list = new ArrayLIst<>();
Collection.addAll(list, "a", "b", "c");

list.stream.forEach(s -> System.out.println(s));

//2.双列集合
HashMap<K, V> hm = new HashMap<>();
hm.put(K, V);
...
//keySet()
hm.keySet().stream.forEach(s -> System.out.println(s));
//entrySet()
hm.entrySet().stream.forEach(s -> System.out.println(s));

//3.数组
int[] arr = {1, 2, 3, 4, 5};
Arrays.stream.forEach(s -> System.out.println(s));

//4.同种数据类型的零散数据
Stream.of("a", "b", "c").forEach(s -> System.out.println(s));
```

```java
//注意：
//Stream接口中静态方法of的细节
//方法的形参是一个可变参数，可以传递一堆零散的数据，也可以传递数组
//但是数组必须是引用数据类型的，如果传递基本数据类型，是会把整个数组当做一个元素，放到Stream当中。
Stream.of(arr1).forEach(s->System.out.println(s));//[I@41629346
```



2. 利用Stream流中的API进行各种操作

<table>
    <tr>
        <td>过滤</td>
        <td>转换</td>
        <td>中间方法</td>
        <td>方法调用完毕之后，还可以调用其他</td>
    </tr>
    <tr>
        <td>统计</td>
        <td>打印</td>
        <td>终结方法</td>
        <td>最后一步，调用完毕之后，不能调用其他方法</td>
    </tr>
</table>



 



