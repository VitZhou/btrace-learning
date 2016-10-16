BTrace Maven支持
======
##组件
####BTrace Maven插件(btrace-maven-plugin)
允许BTrace脚本编译作为Maven项目生命周期的一部分。
```xml
<plugins>
    <plugin>
    <groupId>io.btrace</groupId>
    <artifactId>btrace-maven-plugin</artifactId>
    <version>1.3.8.5</version>
    <executions>
        <execution>
        <goals>
            <goal>btracec</goal>
        </goals>
        </execution>
    </executions>
    <configuration>
        <classpathElements>
            <classpathElement>/path/to/jar_or_folder</classpathElement>
            ...
        </classpathElements>
    </configuration>
    </plugin>
    ...
</plugins>
```
该插件的配置接受以下参数:
- sourceDirectory 编译源码的位置(默认是:${project.build.sourceDirectory})
- classPathElements 用于编译的附加类的路径
- outputDirectory　编译好的二进制文件的存放位置(默认是:${project.build.outputDirectory})

####BTrace项目原型(btrace-project-archetype)
一个简单的原型，用于为包含BTrace脚本的项目生成功能性脚手架。
要引导一个新的项目中使用:
```shell
mvn archetype:generate
  -DgroupId=[你的项目 group id]
  -DartifactId=[你的项目的 artifact id]
  -DarchetypeArtifactId=btrace-project-archetype
```

##用法
加载依赖
```xml
<dependency>
  <groupId>io.btrace</groupId>
  <artifactId>btrace-maven</artifactId>
  <version>1.3.8.6</version>
  <type>pom</type>
</dependency>
```
maven的托管在[这里](https://bintray.com/btraceio/maven/btrace-maven)

修改你的Maven的settings.xml:
```xml
...
<profiles>
    <profile>
        <repositories>
            <repository>
                <snapshots>
                    <enabled>false</enabled>
                </snapshots>
                <id>central</id>
                <name>bintray-central</name>
                <url>https://jcenter.bintray.com</url>
            </repository>
            </repositories>
        <pluginRepositories>
            <pluginRepository>
                <snapshots>
                    <enabled>false</enabled>
                </snapshots>
                <id>central</id>
                <name>bintray-central</name>
                <url>https://jcenter.bintray.com</url>
            </pluginRepository>
        </pluginRepositories>
        <id>bintray</id>
    </profile>
</profiles>
<activeProfiles>
    <activeProfile>bintray</activeProfile>
</activeProfiles>
</settings>
...
```