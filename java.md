java

<img title="" src="file:///C:/Users/Administrator/AppData/Roaming/marktext/images/2023-07-01-08-50-08-image.png" alt="" width="749">

<img src="file:///C:/Users/Administrator/AppData/Roaming/marktext/images/2023-07-01-08-50-25-image.png" title="" alt="" width="712">

让idea读取到java目录下的mybatis xml文件

```
 <build>
        <resources>
            <resource>
                <directory>src/main/java</directory><!--所在的目录-->
                <includes><!--包括目录下的.properties,.xml 文件都会扫描到-->
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>

        <testResources>
            <testResource>
                <directory>src/test/resources</directory>
                <filtering>true</filtering>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </testResource>
        </testResources>

        <!-- ... 其他配置 ... -->
    </build>
```

ctrl+h查看继承体系
