查看依赖的库中都引用了哪些
```gradle
gradlew -q app:dependencies
```

查看各model的依赖树
```gradle
gradlew :app:dependencies --configuration compile
```

把jar文件解压后  删除冲突的文件  然后通过以下命令 重新打包jar
```bash
jar cvf 新的文件.jar -C 原解压出来的文件夹/ .
```


<!-- ##{"timestamp":1544668980}## -->