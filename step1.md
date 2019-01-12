# 还没想好标题

## 读取 XML 文件

1. Resource 接口
   该接口继承 InputStreamSource 接口，InputStreamSource 提供 `InputStream getInputStream() throws IOException` 方法。
   当通过如下方式读取 xml 文件时：

   ```java
       BeanFactory bf = new XmlBeanFactory(new ClassPathResource("hello.xml"));
   ```

   ClassPathResource 类提供了`InputStream getInputStream()`方法的实现：

   ```java
      @Override
      public InputStream getInputStream() throws IOException {
        InputStream is;
        if (this.clazz != null) {
          is = this.clazz.getResourceAsStream(this.path);
        }
        else if (this.classLoader != null) {
          is = this.classLoader.getResourceAsStream(this.path);
        }
        else {
          is = ClassLoader.getSystemResourceAsStream(this.path);
        }
        if (is == null) {
          throw new FileNotFoundException(getDescription() + " cannot be opened because it does not exist");
        }
        return is;
      }
   ```

   采用如上方法读取文件时，文件不存在时返回 null，不会抛出异常。
