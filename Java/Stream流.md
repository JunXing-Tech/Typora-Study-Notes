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
list1.add("张无忌")；
list1.add("周芷若")；
list1.add("赵敏")；
list1.add("张强")；
list1.add("张三丰")；

list1.stream()
    .filter(name -> name.startswith("张"))
    .filter(name -> name.length() == 3)
    .forEach(name -> System.out.println(name));

```

#### Stream流的作用

* 结合了Lambda表达式，简化集合、数组的操作

#### Stream流的使用步骤

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
Arrays.stream(arr).forEach(s -> System.out.println(s));

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



2. #### 利用Stream流中的API进行各种操作

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
##### Stream流的中间方法

| 名称                                             | 说明                                   |
| ------------------------------------------------ | -------------------------------------- |
| Stream<T> filter(Predicate<? super T> predicate) | 过滤                                   |
| Stream<T> limit(long maxSize)                    | 获取前几个元素                         |
| Stream<T> skip(long n)                           | 跳过前几个元素                         |
| Stream<T> distinct()                             | 元素去重，依赖（hashCode和equals方法） |
| static <T> Stream<T> concat(Stream a, Stream b)  | 合并a和b两个流为一个流                 |
| Stream<R> map(Function<T, R> mapper)             | 转换流中的数据类型                     |

==注意1：==中间方法，返回新的Stream流，原来的Stream流只能使用一次，建议使用链式编程

==注意2：==修改Stream流中的数据，不会影响原来集合或者数组中的数据

```java
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "value1", "value2", "value3");

list.stream().filter(new Predicate<String>() { //filter()的匿名内部类的使用方法
    @Override
    public boolean test(String s) {
        //如果返回值为true，表示当前数据留下
        //如何返回值为false，表示当前数据舍弃不要
        return s.startsWith("过滤条件"); //Stream流的过滤条件
    }
}).forEach(s -> System.out.println(s));

list.stream() //Lambda表达式的使用方法
        .filter(s -> s.startsWith("过滤条件"))
        .filter(s -> s.length() == 3)
        .forEach(s -> System.out.println(s));

list.stream().limit(3) //limit(3) -> 获取前3个元素
        .forEach(s -> System.out.println(s));
list.stream().skip(3) //skip(3) -> 跳过前3个元素
        .forEach(s -> System.out.println());

//元素去重，无须指定
//注：该方法去重是依赖hashCode和equals方法，在String类中Java已重写好了hashCode和equals方法，如果是自定义对象，例如ArrayList<Student>，则需要自己重写hashCode和equals方法
list.stream().distinct()
    .forEach(s -> System.out.println(s));

//在使用Stream.concat()时，应尽可能的保证a流和b流数据类型相同，如不同，则合并后的流的数据类型为两种数据类型的父类，而导致无法使用子类的特有功能
Stream.concat(list1.stream(), list2.stream())
    .forEach(s -> System.out.println(s));

//map()的使用方法
ArrayList<String> list = new ArrayList<>();
Collections.addALL(list, "String1-int1", "String2-int2", "String3-int3");

//需求：只获取Integer类型的值并进行打印
//String -> int
//第一个类型：流种原本的数据类型
//第二个类型：要转成之后的数据
list.stream().map(new Function<String, Integer>() {
    //apply的形参s:依次表示流里面的每一个数据
	//返回值：表示转换之后的数据
    @Override
    public Integer apply(String s) {
        String[] arr = s.split("-");
        String ageString = arr[1];
		//Integer.parseInt()是将一个字符串转换为int类型的基本方法
        int age = Integer.parseInt(ageString);
        return age;
    }
//当map方法执行完毕之后，流上的数据就变成了整数
//所以在下面forEach当中，s依次表示流里面的每一个数据，这个数据现在就是整数了
}).forEach(s -> System.out.println(s));

//链式编程
list.stream()
    .map(s -> Integer.parseInt(s.split("-")[1]))
    .forEach(s -> System.out.println(s));
```

##### Stream流的终结方法

| 名称                          | 说明                       |
| ----------------------------- | -------------------------- |
| void forEach(Consumer action) | 遍历                       |
| long count()                  | 统计                       |
| toArray()                     | 收集流中的数据，放到数组里 |
| collect(Collector collector)  | 收集流中的数据，放到集合中 |

```java
//void forEach(Consumer action)		遍历
//Consumer的泛型：表示流中数据的类型
//accept方法的形参s：依次表示流里面的每一个数据
//方法体：对每一个数据的处理操作（打印）
list.stream().forEach(new Consumer<string>(){
    @Override
    public void accept(String s){
    	System.out.println(s);
    }
});

list.stream().forEach(s -> System.out.println(s));

//long count()		统计
//统计并打印流中元素个数
long count = list.stream().count();
System.out.println(count);

//toArray()		收集流中的数据，放到数组中
//Object[]arr1 = list.stream().toArray();
//System.out.println(Arrays.tostring(arr1));

//IntFunction的泛型：具体类型的数组
//apply的形参：流中数据的个数，要跟数组的长度保持一致
//app1y的返回值：具体类型的数组
//方法体：就是创建数组

//toArray方法的参数的作用：负责创建一个指定类型的数组
//toArray方法的底层，会依次得到流里面的每一个数据，并把数据放到数组当中
//toArray方法的返回值：是一个装着流里面所有数据的数组
String[] arr = list.stream().toArray(new IntFunction<String[]>(){
    @Override
    public String[] apply(int value){
        return new String[value];
    }
});
System.out.println(Arrays.tostring(arr));

String[] arr = list.stream().toArray(value -> new String[value]);
System.out.println(Arrays.tostring(arr));
```

```java
//collect(Collector collector)		收集流中的数据，放到集合中（List、Set、Map）
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "String1-value1-int1", "String2-value2-int2");

//把中间值为“value1”的数据收集，放入List集合当中
List<String> newList = list.stream()
        .filter(new Predicate<String>() {
            @Override
            public boolean test(String s) {
                return "value1".equals(s.split("-")[1]);
            }
        }).collect(Collectors.toList());

//放入Set集合当中
Set<String> newList = list.stream().filter(s -> "value1".equal(s.split("-")[1]))
    .collection(Collectors.toSet());

//collect(Collector collector)		收集流中的数据，放到集合中（List、Set、Map）
//注意点：如果我们要收集到Map集合当中，键不能重复，否则会报错
/**
 * toMap：参数一表示键的生成规则
 *        参数二表示值的生成规则
 * 参数一：
 *       Function泛型-：表示流中每一个数据的类型
 *               泛型二：表示Map集合中键的数据类型
 *       方法apply形参：依次表示流里面的每一个数据
 *               方法体：生成键的代码
 *               返回值：已经生成的键
 * 参数二：
 *       Function泛型一：表示流中每一个数据的类理
 *               泛型二：表示Map集合中值的数据类型
 *       方法apply形参：依次表示流里面的每一个数据
 *               方法体：生成键的代码
 *               返回值：已经生成的键
 */
//匿名内部类
Map<String, Integer> map = list.stream()
        .filter(s -> "value1".equals(s.split("-")[1]))
        .collect(Collectors.toMap(new Function<String, String>() {
            @Override
            public String apply(String s) {
                return s.split("-")[0];
                }
            }, new Function<String, Integer>(){
                @Override
                public Integer apply(String s) {
                    return Integer.parseInt(s.split("-")[2]);
                }
        }));
//Lambda表达式
Map<String, Integer> map = list.stream()
        .filter(s -> "value1".equals(s.split("-")[1]))
        .collect(Collectors.toMap(
                s -> s.split("-")[0],
                s -> Integer.parseInt(s.split("-")[2])
        ));
```

#### 总结

1. Stream流的作用

结合了Lambda表达式，简化集合、数组的操作

2. Stream的使用步骤

   1. 获取Stream流对象

   2. 使用中间方法处理数据

   3. 使用终结方法处理数据

3. 如何获取Stream流对象

   * 单列集合：Collection中的默认方法stream

   * 双列集合：不能直接获取

   * 数组：Arrays.工具类型中的静态方法stream
     一堆零散的数据：Stream接口中的静态方法of

4. 常见方法

* 中间方法：
  * filter,limit,skip,distinct,concat,map

* 终结方法：
  * forEach,count,collect

#### 补充知识：List集合和Set集合的联系与区别

List和Set是两种常见的集合类型，它们在数据存储和使用方式上有一些联系和区别。

联系：

* 都是Java集合框架中的接口。
* 都可以存储多个对象。
* 都支持迭代操作，可以遍历集合中的元素。

区别：

* 数据重复性：
  * List允许存储重复的元素，而Set不允许存储重复的元素。Set通过使用元素对象的hashCode()和equals()方法来判断元素是否重复。

* 数据顺序性：
  * List是有序集合，它按照添加顺序保存元素，可以根据索引访问集合中的元素。可以通过调用get()方法来获取指定位置的元素。例如，ArrayList是List接口的实现。
  * Set是无序集合，它不保留元素的插入顺序，也没有索引。可以使用迭代器进行遍历访问。例如，HashSet是Set接口的实现。

* 数据结构：
  * List可以使用数组或链表等数据结构实现。常见的List实现有ArrayList、LinkedList和Vector。
  * Set一般使用哈希表数据结构实现，通过散列表来存储和访问元素。常见的Set实现有HashSet、LinkedHashSet和TreeSet。

* 性能：
  * List在插入和删除元素时，由于需要维护元素顺序，性能可能较差。在查询操作时，可以通过索引直接访问元素，性能较好。
  * Set在判断元素是否存在、插入和删除元素时，通过哈希表的运算，性能较好。但是在遍历元素时，由于无序性，性能较差。
    根据实际需求，在选择使用List还是Set时，需要考虑数据的重复性、顺序性和对性能的要求。
