[toc]

### File

#### 概述和构造方法

* File对象就表示一个路径，可以是文件的路径、也可以是文件夹的路径

* 这个路径可以是存在的，也允许是不存在的

| 方法名称                                 | 说明                                               |
| ---------------------------------------- | -------------------------------------------------- |
| public File(String pathName)             | 根据文件路径创建文件对象                           |
| public File(String parent, String child) | 根据父路径名字符串和子路径名字符串创建文件对象     |
| public File(File parent, String child)   | 根据父路径对应文件对象和子路径名字符串创建文件对象 |

1. 使用文件路径字符串：

```java
String filePath = "C:/path/to/file.txt";
File file1 = new File(filePath);
```

2. 使用父目录和子文件/目录的字符串：

```java
String parentDirectory = "C:/path/to";
String childFile = "file.txt";
File file2 = new File(parentDirectory, childFile);
```

3. 使用父目录的File对象和子文件/目录的字符串：

```java
File parentDirectory = new File("C:/path/to");
String childFile = "file.txt";
File file3 = new File(parentDirectory, childFile);
```

这些构造方法的使用允许我们创建表示文件或目录的File对象。我们可以使用File对象进行各种操作，如判断文件/目录是否存在、获取文件/目录的属性、创建/删除文件或目录等。

#### File的常见成员方法

##### 判断或获取的相关方法

| 方法名称                        | 说明                                 |
| ------------------------------- | ------------------------------------ |
| public boolean isDirectory()    | 判断此路径名表示的File是否为文件夹   |
| public boolean isFile()         | 判断此路径名表示的File是否为文件     |
| public boolean exists()         | 判断此路径名表示的File是否存在       |
| public long length()            | 返回文件的大小（字节数量）           |
| public String getAbsolutePath() | 返回文件的绝对路径                   |
| public String getpath()         | 返回定义文件时使用的路径             |
| public string getName()         | 返回文件的名称，带后缀               |
| public long lastModified()      | 返回文件的最后修改时间（时间毫秒值） |

```java
//1.对一个文件的路径进行判断
File f1 = new File(pathname:"D:\\aaa\\a.txt");
System.out.println(f1.isDirectory());//false
System.out.println(f1.isFile());//true
System.out.println(f1.exists());//true

//2.对一个文件夹的路径进行判断
File f2 = new File(pathname:"D:\\aaa\\bbb");
System.out.println(f2.isDirectory());//true
System.out.println(f2.isFile());//false
System.out.println(f2.exists());//true

//3.对一个不存在的路径进行判断
File f3 = new File(pathname:"D:\\aaa\\c.txt");
System.out.println(f3.isDirectory());//false
System.out.println(f3.isFile());//false
System.out.println(f3.exists());//false
```



```java
//1.length 返回文件的大小（字节数量）
//细节1：这个方法只能获取文件的大小，单位是字节
//如果单位我们要是M,G,可以不断的除以1024
//细节2：这个方法无法获取文件夹的大小
//如果我们要获取一个文件夹的大小，需要把这个文件夹里面所有的文件大小都累加在一起。
File f1 new File(pathname:"D:\\aaa\\a.txt");
long len f1.length();
System.out.println(len);

public String getAbsolutePath()	返回文件的绝对路径

public String getpath()	返回定义文件时使用的路径
    
public string getName()	返回文件的名称，带后缀
//细节：
//	如果目标是文件，则返回的是文件名.后缀名
//    如果是文件夹，则返回的是文件夹的名称

public long lastModified()	返回文件的最后修改时间（时间毫秒值）
```

##### 创建和删除的相关方法

| 方法名称                       | 说明                 |
| ------------------------------ | -------------------- |
| public boolean createNewFile() | 创建一个新的空的文件 |
| public boolean mkdir()         | 创建单级文件夹       |
| public boolean mkdirs()        | 创建多级文件夹       |
| public boolean delete()        | 删除文件、空文件夹   |

注意：delete方法默认只能删除文件和空文件夹，delete方法是直接删除，被删除的内容是不会经过回收站

````java
public boolean createNewFile()	创建一个新的空的文件
//细节1：如果当前路径表示的文件是不存在的，则创建成功，方法返回tue
//		如果当前路径表示的文件是存在的，则创建失败，方法返回fa1se
//细节2：如果父级路径是不存在的，那么方法会有异常 I0Exception
//细节3：createNewFile方法创建的一定是文件，如果路径中不包含后缀名，则创建一个没有后缀的文件
File file = new File("C:\\Users\\JunXing\\Desktop\\FileOfTest.txt");
boolean newFile = file.createNewFile();
System.out.println(newFile);
    
public boolean mkdir()	创建单级文件夹
//细节1：windows当中路径是唯一的，如果当前路径已经存在，则创建失败，返回false
//细节2：mkdir方法只能创建单级文件夹，无法创建多级文件夹。
    
public boolean mkdirs()	创建多级文件夹
//细节：既可以创建单级文件夹，也可以创建多级文件夹
    
public boolean delete()	删除文件、空文件夹
//细节：
//	如果删除的是文件，则直接删除，不走回收站。
//	如果刚除的是空文件夹，则直接删除，不走回收站
//	如果删除的是有内容的文件夹，则删除失败
````

##### 获取并遍历的相关方法

| 方法名称                  | 说明                     |
| ------------------------- | ------------------------ |
| public File[] listFiles() | 获取当前该路径下所有内容 |

```java
public File[] listFiles()	获取当前该路径下所有内容
//当调用者File表示的路径不存在时，返回null
//当调用者File表示的路径是文件时，返回null
//当调用者File表示的路径是一个空文件夹时，返回一个长度为0的数组
//当调用者File表示的路径是一个有内容的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回
//当调用者File表示的路径是一个有隐藏文件的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回，包括隐藏文件
//当调用者File表示的路径是需要权限才能访问的文件夹时，返回null
```

##### 所有获取并遍历的相关方法

| 方法名称                                       | 说明                                     |
| ---------------------------------------------- | ---------------------------------------- |
| public static File[] listRoots()               | 列出可用的文件系统根                     |
| public string[] list()                         | 获取当前该路径下所有内容                 |
| public String[] list(FilenameFilter filter)    | 利用文件名过滤器获取当前该路径下所有内容 |
| [掌握]public File[] listFiles()                | 获取当前该路径下所有内容                 |
| public File[] listFiles(FileFilter filter)     | 利用文件名过滤器获取当前该路径下所有内容 |
| public File[] listFiles(FilenameFilter filter) | 利用文件名过滤器获取当前该路径下所有内容 |

```java
public static File[] listRoots()	列出可用的文件系统根
//获取系统中的所有盘符，且该方法为静态方法  
    
public string[] list()	获取当前该路径下所有内容
//仅仅是获取内容的名称
     
public String[] list(FilenameFilter filter)	利用文件名过滤器获取当前该路径下所有内容

public File[] listFiles()	获取当前该路径下所有内容
    
public File[] listFiles(FileFilter filter)	利用文件名过滤器获取当前该路径下所有内容

public File[] listFiles(FilenameFilter filter)	利用文件名过滤器获取当前该路径下所有内容
```

```java
public static void main(String[] args) {

    File file = new File("D:\\aaa");
    //accept方法的形参，依次表示aaa文件夹里面每一个文件或者文件夹的路径
    //参数一：父级路径
    //参数二：子级路径
    //返回值：如果返回值为true,就表示当前路径保留
    //如果返回值为fa1se,就表示当前路径舍弃不要
    file.list(new FilenameFilter() {
        @Override
        public boolean accept(File dir, String name) {
            File src = new File(dir, name);
            return src.isFile() && name.endsWith(".txt");
        }
    });

    File[] files1 = file.listFiles();
    for(File listFiles : files1){
        if(listFiles.isFile() && listFiles.getName().endsWith(".txt")){
            System.out.println(listFiles);
        }
    }

    File[] files2 = file.listFiles(new FileFilter() {
        @Override
        public boolean accept(File pathname) {
            return pathname.isFile() && pathname.getName().endsWith(".txt");
        }
    });

    File[] files3 = file.listFiles(new FilenameFilter() {
        @Override
        public boolean accept(File dir, String name) {
            File src = new File(dir, name);
            return src.isFile() && name.endsWith(".txt");
        }
    });
}
```

#### 案例

```java
//删除多级文件夹
private static void test4(File file) {
    //进入文件夹
    File[] files = file.listFiles();
    //遍历文件夹内的文件和文件夹
    for (File file1 : files) {
        //如果是文件
        if(file1.isFile()){
            file1.delete();
        } else { //如果是文件夹
            test4(file1);
        }
    }
    //如果文件夹遍历完成，且文件夹为空（文件全部删除或者本来就为空），就删除此文件夹
    file.delete();
}
```

```java
//遍历硬盘查找文件
private static void test3() {
    File[] arr = File.listRoots();
    for(File f : arr){
        test3(f);
    }
}

private static void test3(File src) {
    //1.进入文件夹
    File[] files = src.listFiles();
    //2.遍历数组，依次得到里面的每一个文件或文件夹
    if(files != null){ //如果文件夹内有需要权限才能访问的文件，则listFiles()方法会返回null
        for(File file : files){
            if(file.isFile()){
                //判断如果是文件，则执行业务逻辑
                String name = file.getName();
                if(name.endsWith(".txt")){
                    System.out.println(file);
                }
            } else {
                //如果是文件夹，则递归文件夹内的文件
                test3(file);
            }
        }
    }
}
```

```java
//统计文件夹大小    
private static long test5(File src) {
    long len = 0;
    File[] files = src.listFiles();
    //遍历文件数组，并作出判断
    // 如果是文件则累加文件大小
    // 如果是文件夹则递归，进入文件夹内再进行遍历
    for (File file : files) {
        if(file.isFile()){
            len = len + file.length();
        } else {
            len = len + test5(file);
        }
    }
    return len;
}
```

```java
//统计文件夹内各种文件的数量
public static void main(String[] args) {

    File file = new File("D:\\aDrive");

    TreeMap<String, Integer> treeMap = new TreeMap<>();

    NumberOfFile(file, treeMap);

    //遍历
    treeMap.forEach(new BiConsumer<String, Integer>() {
        int index = 0;
        @Override
        public void accept(String s, Integer integer) {
            index += integer;
            System.out.println(s + ":" + integer);
            System.out.println(index);
        }
    });
}

private static void NumberOfFile(File file, TreeMap<String, Integer> treeMap) {
    //1.进入文件夹
    File[] listFiles = file.listFiles();
    //2.遍历files数组，并作出判断
    for (File files : listFiles) {
        if(files.isFile()) {
            String[] split = files.getName().split("\\.");
            int endName = split.length - 1;
            //判断该文件是否有后缀名
            if (split.length >= 2) {
                //2.1如果是文件，则获取文件的后缀名
                //3.判断该文件后缀名是不是第一次添加
                //3.1如果是第一次添加，则添加相关后缀，并使其值为 1
                if (!treeMap.containsKey(split[endName]) || treeMap.isEmpty()) {
                    treeMap.put(split[endName], 1);
                    continue;
                }
                //3.2如果不是第一次添加，则值 + 1
                if (treeMap.containsKey(split[endName])) {
                    treeMap.put(split[endName], treeMap.get(split[endName]) + 1);
                }
            } 
        } else {
            //2.2如果是文件夹，则递归进入文件夹
            NumberOfFile(files, treeMap);
        }
    }
}
```

