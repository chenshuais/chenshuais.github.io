类关系图：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/6b82af80bd927dd911fa4e17dcf189258562236b.png">`

由于InputStream、OutputStream是抽象类，所以要创建他们的子类对象。

一般读写取文件都使用FileInputStream、FileOutputStream.

1. 输入流 一般步骤

    a. 创建字节输入流对象

    ```java
    FileInputStream fis = new FileInputStream("text.txt");
    ```

    b. 调用read方法
    ```java
    //方式1 一次读取一个字节
    int by = 0;
    while((by=fis.read())!=-1) {
	    System.out.print((char)by);
    }
			
    //方式2 一次读取一个字节数组
    byte[] bys = new byte[1024];
    int len = 0;
    while((len=fis.read(bys))!=-1) {
	    System.out.print(new String(bys,0,len));
    }
    ```

    c .释放资源
    ```java
    fis.close();
    ```

2. 输出流 一般步骤

    a. 创建字节流输出对象
    ```java
    FileOutputStream fos = new FileOutputStream("text.txt");
    ```

    b. 调用write方法
    ```java
    fos.write("hello world".getBytes());
    ```

    c. 释放资源
    ```java
    fos.close();
    ```

> 参考文章：
> 
> https://www.jianshu.com/p/509c78602ed2
>
> https://www.ibm.com/developerworks/cn/java/j-lo-serial/index.html
>
> https://www.w3cschool.cn/java/java-io-inputstream.html


<!-- ##{"timestamp":1551058920}## -->