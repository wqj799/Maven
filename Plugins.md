Maven本质上是一个执行插件的框架，所有的工作都是由插件完成的。

构建时使用的插件在构建期间执行，配置在POM的`<build/>`元素中。

报告时使用的插件在站点生成期间执行，配置在POM的`<reporting/>`元素中。

# clean

- clean:clean
  
  - 版本信息
    
    `org.apache.maven.plugins:maven-clean-plugin:3.2.0:clean`
  
  - 描述
    
    尝试清理在构建时工作目录中生成的的文件。默认情况下，它会查找并删除由以下属性配置的工作目录：
    
    1. project.build.directory
    
    2. project.build.outputDirectory
    
    3. project.build.testOutputDirectory
    
    4. project.reporting.outputDirectory
    
    通过`filesets`标签可以清理默认值以外的文件。
  
  - 参数
    
    | 参数名                       | 类型        | 描述                                                                                                                                             |
    | ------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
    | excludeDefaultDirectories | boolean   | 禁止清理默认的输出目录。如果设置为true，则只能清理使用`<filesets>`标签配置的目录。<br/>默认为false。                                                                                |
    | failOnError               | boolean   | 当清理时出现错误是否继续执行。<br/>默认为true。                                                                                                                   |
    | fast                      | boolean   | 是否启用快速清理。当为true时，则插件在执行时，将自动把要清理的目录移动到maven.clean.fastDir目录中，并启动一个线程在后台清理该目录。构建完成后，maven会等待此线程清理结束。如果在移动过程中出现错误，则插件将使用默认的传统清理机制。<br/>默认为false。 |
    | fastDir                   | File      | 当指定`<fast>`标签为true时，此属性将指定移动的位置。如果未指定，将使用`${maven.multiModuleProjectDirectory}/target/.clean`目录。如果`${build.directory}`已被修改，则必须明确调整此属性。         |
    | fastMode                  | String    | 使用快速清理时的模式：值为background时，立即开始清理操作并在会话结束时等待清理操作完成。值为at-end时，表示会话结束时应该同步执行清理操作。值为defer时，表示会话结束时应该在后台启动清理操作。<br/>默认为background。                   |
    | filesets                  | Fileset[] | 除默认目录外，需要被清理的文件列表。使用方法参考※注1                                                                                                                    |
    | followSymLinks            | boolean   | 设置插件从默认输出目录中删除文件使用应遵循符号链接。<br/>默认为false。                                                                                                       |
    | retryOnError              | boolean   | 删除文件失败后是否进行重试。<br/>默认为true。                                                                                                                    |
    | skip                      | boolean   | 是否跳过执行此插件。<br/>默认为false。                                                                                                                       |
    | verbose                   | boolean   | 设置是否已详细模式运行此插件。                                                                                                                                |
    
    **※注1**
    
    ```xml
    <filesets>
      <fileset>
        <directory>src/main/generated</directory>
        <followSymlinks>false</followSymlinks>
        <useDefaultExcludes>true</useDefaultExcludes>
        <includes>
          <include>*.java</include>
        </includes>
        <excludes>
          <exclude>Template*</exclude>
        </excludes>
      </fileset>
    </filesets>
    ```

# compiler

- compiler:compile
  
  - 版本信息：
    
    `org.apache.maven.plugins:maven-compiler-plugin:3.10.1:compile`
  
  - 描述
    
    编译源代码。
  
  - 参数
    
    | 参数名                           | 类型       | 最低版本   | 描述                                                                                                                                                                                                                                                                         |
    | ----------------------------- | -------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | annotationProcessorPaths      | List     | 3.5    | 指定注解处理器的类路径。如果不指定，则使用默认的注解处理器类路径。参考※注2                                                                                                                                                                                                                                     |
    | annotationProcessors          | String[] | 2.2    | 要运行的注解处理器的名称。                                                                                                                                                                                                                                                              |
    | compilerArgs                  | List     | 3.1    | 设置要传递给编译器的参数。**注意仅当 fork 设置为 true 时才会传递 -J 选项。** 参考※注3                                                                                                                                                                                                                     |
    | compilerArgument              | String   | 2.0    | 设置要传递给编译器的未格式化的单个字符串参数。要传递多个参数，如-Xmaxerrs 1000（这里实际是两个参数），必须使用compilerArgs。**注意仅当 fork 设置为 true 时才会传递 -J 选项。**                                                                                                                                                             |
    | compilerId                    | String   | 2.0    | 要使用的编译器的ID。<br/>默认为javac                                                                                                                                                                                                                                                   |
    | compilerReuseStrategy         | String   | 2.5    | 复用javac类的策略：<br/>reuseCreate（默认）：多线程构建的情况下，每个线程将有单独的实例。<br/>reuseSame：多线程构建下，每次编译都将使用相同的javac类。<br/>alwaysNew：每次编译都会创建一个新的javac类。                                                                                                                                          |
    | compilerVersion               | String   | 2.0    | 指定编译器版本。注意fork需要设置为true。                                                                                                                                                                                                                                                   |
    | createMissingPackageInfoClass | boolean  | 3.10   | 包信息源文件只包含 javadoc 而包上没有注解可能导致编译器不生成类文件。 这会导致下一次编译时文件丢失并强制进行不必要的重新编译。 默认值 true 会导致生成一个空的类文件。 可以通过将此参数设置为 false 来更改此行为。                                                                                                                                                      |
    | debug                         | boolean  | 2.0    | 设置为 true 以在编译的类文件中包含调试信息。<br/>默认为true。                                                                                                                                                                                                                                     |
    | debugFileName                 | String   | 3.10.0 | 当forking和debug为true时，命令行将转储到此文件。                                                                                                                                                                                                                                           |
    | debuglevel                    | String   | 2.1    | 要附加到 -g 命令行开关的关键字列表。 合法值是 none 或以下关键字的逗号分隔列表：lines、vars 和 source。 如果未指定调试级别，默认情况下，-g 不会附加任何内容。 如果未打开调试，则该属性将被忽略。                                                                                                                                                           |
    | enablePreview                 | boolean  | 3.10.1 | 设置为 true 以启用 java 编译器的预览语言功能。<br/>默认为false。                                                                                                                                                                                                                                |
    | encoding                      | String   | 2.1    | Java 编译器的 -encoding 参数。                                                                                                                                                                                                                                                    |
    | excludes                      | Set      | 2.0    | 编译器的排除过滤器列表。                                                                                                                                                                                                                                                               |
    | executable                    | String   | 2.0    | 设置Fork为True时使用编译器的可执行文件。                                                                                                                                                                                                                                                   |
    | failOnError                   | boolean  | 2.0.2  | 指示即使出现编译错误，构建是否会继续。<br/>默认为true。                                                                                                                                                                                                                                           |
    | failOnWarning                 | boolean  | 3.6    | 指示即使出现编译警告，构建是否会继续。<br/>默认为false。                                                                                                                                                                                                                                          |
    | fileExtensions                | List     | 3.1    | 用于检查增量构建时间戳的文件扩展名。 默认只包含class和jar。                                                                                                                                                                                                                                         |
    | forceJavacCompilerUse         | boolean  | 3.0    | 编译器现在可以使用 javax.tools （如果在您当前的 jdk 中可用）。<br/>默认为false。                                                                                                                                                                                                                     |
    | fork                          | boolean  | 2.0    | 允许在单独的进程中运行编译器。 如果为假，则使用内置编译器，如果为真，则使用可执行文件。<br/>默认为false。                                                                                                                                                                                                                 |
    | generatedSourcesDirectory     | File     | 2.2    | 指定由注解创建的生成源文件的位置。 仅适用于 JDK 1.6+                                                                                                                                                                                                                                            |
    | includes                      | Set      | 2.0    | 编译器的包含过滤器列表。                                                                                                                                                                                                                                                               |
    | jdkToolchain                  | Map      | 3.6    | 指定此 jdk 工具链的要求，以使用与 Maven 使用的 JRE 不同的 javac。参考※注4                                                                                                                                                                                                                          |
    | maxmem                        | String   | 2.0.1  | 设置内存分配池的最大大小（以 MB 为单位）。                                                                                                                                                                                                                                                    |
    | meminitial                    | String   | 2.0.1  | 内存分配池的初始大小（以 MB 为单位）。                                                                                                                                                                                                                                                      |
    | multiReleaseOutput            | boolean  | 3.7.1  | 当设置为 true 时，类会放在 META-INF/versions/${release} 中，必须设置 release 值，否则插件会失败。                                                                                                                                                                                                    |
    | outputFileName                | String   | 2.0    | 将一组源编译为单个文件时设置输出文件的名称。 表达式="${project.build.finalName}"                                                                                                                                                                                                                    |
    | parameters                    | boolean  | 2.0    | 设置为 true 以生成元数据以反映方法参数。<br/>默认为false。                                                                                                                                                                                                                                      |
    | proc                          | String   | 2.2    | 设置是否执行注释处理。 仅适用于 JDK 1.6+ 如果不设置，编译和注解处理同时进行。<br/>允许的值为：
<br/>none：不执行注释处理。
<br/>only：只进行注解处理，不进行编译                                                                                                                                                                         |
    | release                       | String   | 3.6    | Java 编译器的 -release 参数，从 Java9 开始支持                                                                                                                                                                                                                                         |
    | showDeprecation               | boolean  | 2.0    | 设置是否显示使用已弃用 API 的源位置。                                                                                                                                                                                                                                                      |
    | showWarnings                  | boolean  | 2.0    | 设置为 true 以显示编译警告。                                                                                                                                                                                                                                                          |
    | skipMain                      | boolean  | 2.0    | 将此设置为“真”以绕过main资源的编译。                                                                                                                                                                                                                                                      |
    | source                        | String   | 2.0    | Java 编译器的 -source 参数。                                                                                                                                                                                                                                                      |
    | staleMillis                   | int      | 2.0    | 设置最后修改日期的粒度（以毫秒为单位），以测试源是否需要重新编译。                                                                                                                                                                                                                                          |
    | target                        | String   | 2.0    | Java 编译器的 -target 参数。                                                                                                                                                                                                                                                      |
    | useIncrementalCompilation     | boolean  | 3.1    | 启用/禁用增量编译功能。<br/>这会导致两种不同的模式，具体取决于底层编译器。 默认的 javac 编译器执行以下操作：
<br/>true （默认）在此模式下，编译器插件确定当前模块所依赖的任何 JAR 文件在当前构建运行中是否已更改； 或自上次编译以来添加、删除或更改的任何源文件。 如果是这种情况，编译器插件会重新编译所有源代码。
<br/>false （不推荐）这只编译比其相应类文件更新的源文件，即自上次编译以来已更改的源文件。 这不会重新编译使用更改后的类的其他类，可能会使它们引用不再存在的方法，从而导致运行时出错。 |
    | verbose                       | boolean  | 2.0    | 设置为 true 以显示有关编译器正在做什么的消息。<br/>默认为false。                                                                                                                                                                                                                                   |
    
    **※注2**
    
    ```xml
    <configuration>
      <annotationProcessorPaths>
        <path>
          <groupId>org.sample</groupId>
          <artifactId>sample-annotation-processor</artifactId>
          <version>1.2.3</version>
        </path>
        <!-- ... more ... -->
      </annotationProcessorPaths>
    </configuration>
    ```
    
    **※注3**
    
    ```xml
    <compilerArgs>
      <arg>-Xmaxerrs</arg>
      <arg>1000</arg>
      <arg>-Xlint</arg>
      <arg>-J-Duser.language=en_us</arg>
    </compilerArgs>
    ```
    
    **※注4**
    
    ```xml
    <configuration>
      <jdkToolchain>
        <version>11</version>
      </jdkToolchain>
      ...
    </configuration>
    
    <configuration>
      <jdkToolchain>
        <version>1.8</version>
        <vendor>zulu</vendor>
      </jdkToolchain>
      ...
    </configuration>
    ```

- compiler:testCompile
  
  Maven生命周期中的test-compile阶段，用于编译测试文件。

# deploy

## 描述

将工件部署到远端存储库。

## 目标

- deploy:deploy
  
  Maven生命周期中的deploy阶段，将工件部署到远端仓库。

- deploy:deploy-file
  
  将工件安装到远端仓库。

## 依赖库信息

```xml
<project>
  ...
  <build>
    <!-- To define the plugin version in your parent POM -->
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>3.0.0-M2</version>
        </plugin>
        ...
      </plugins>
    </pluginManagement>
    <!-- To use the plugin goals in your POM or parent POM -->
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-deploy-plugin</artifactId>
        <version>3.0.0-M2</version>
      </plugin>
      ...
    </plugins>
  </build>
  ...
</project>
```

## 使用方法

```shell
mvn deploy
```

## 配置

- 部署工件
  
  要将工件部署到远端仓库需要提供`<distributionManagement/>`参数，该参数提供了一个`<repository/>`参数定义了工件的远端存储库的位置。如下所示：
  
  ```xml
  [...]
    <distributionManagement>
      <repository>
        <id>internal.repo</id>
        <name>MyCo Internal Repository</name>
        <url>Host to Company Repository</url>
      </repository>
    </distributionManagement>
  [...]
  ```
  
  若要将快照工件和发布工件分开，可以指定`<snapshotRepository/>`单独定义快照工件的远程存储库位置。要部署项目网站，可以指定`<site/>`参数。
  
  如果远端存储库需要身份验证，则需要在`settings.xml`文件中使用`<server/>`参数提供身份验证信息。该参数中的`<id/>`元素匹配`<repository/>`的`<id/>`元素。`settings.xml`文件的配置如下所示：
  
  ```xml
  [...]
      <server>
        <id>internal.repo</id>
        <username>maven</username>
        <password>foobar</password>
      </server>
  [...]
  ```
  
  要发布的工件不是由Maven构建的情况下可以使用以下方式部署到远端仓库：
  
  ```shell
  mvn deploy:deploy-file -Durl=file://C:\m2-repo \
                         -DrepositoryId=some.id \
                         -Dfile=your-artifact-1.0.jar \
                         [-DpomFile=your-pom.xml] \
                         [-DgroupId=org.some.group] \
                         [-DartifactId=your-artifact] \
                         [-Dversion=1.0] \
                         [-Dpackaging=jar] \
                         [-Dclassifier=test] \
                         [-DgeneratePom=true] \
                         [-DgeneratePom.description="My Project Description"] \
                         [-DrepositoryLayout=legacy]
  ```

- 使用FTP部署构建
  
  在pom.xml中配置使用FTP服务器。
  
  ```xml
  <project>
    ...
    <distributionManagement>
      <repository>
        <id>ftp-repository</id>
        <url>ftp://repository.mycompany.com/repository</url>
      </repository>
    </distributionManagement>
  
    <build>
      <extensions>
        <!-- Enabling the use of FTP -->
        <extension>
          <groupId>org.apache.maven.wagon</groupId>
           <artifactId>wagon-ftp</artifactId>
           <version>1.0-beta-6</version>
        </extension>
      </extensions>
    </build>
    ...
  </project>
  ```
  
  在settings.xml中配置FTP需要的身份验证信息：
  
  ```xml
  <settings>
    ...
    <servers>
      <server>
        <id>ftp-repository</id>
        <username>user</username>
        <password>pass</password>
      </server>
    </servers>
    ...
  </settings>
  ```

- 使用SSH部署构建
  
  在pom.xml中配置使用SSH服务器。
  
  ```xml
  <project>
    ...
    <distributionManagement>
      <repository>
        <id>ssh-repository</id>
        <url>scpexe://repository.mycompany.com/repository</url>
      </repository>
    </distributionManagement>
  
    <build>
      <extensions>
        <!-- Enabling the use of SSH -->
        <extension>
          <groupId>org.apache.maven.wagon</groupId>
           <artifactId>wagon-ssh-external</artifactId>
           <version>1.0-beta-6</version>
        </extension>
      </extensions>
    </build>
    ..
  </project>
  ```
  
  如果在Unix中部署或是安装了Cygwin，则不需要在settings.xml中进行配置，所有的内容都将从环境变量中获取。

- 配置失败后多次部署
  
  通过使用`<retryFailedDeploymentCount/>`参数配置部署失败后重试次数。
  
  ```xml
  <project>
    [...]
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-deploy-plugin</artifactId>
          <configuration>
            <retryFailedDeploymentCount>3</retryFailedDeploymentCount>
          <configuration>
        </plugin>
      </plugins>
    </build>
    [...]
  </project>
  ```

# failsafe

## 描述

用于Maven生命周期的集成测试和验证阶段。在集成测试阶段不会使构建失败，从而后续阶段能够执行。

集成测试包含四个阶段：

- pre-integration-test：设置集成测试环境

- integration-test：运行集成测试

- post-integration-test：拆除集成测试环境

- verify：检查集成测试结果

## 目标

- failsafe:integration-test
  
  运行集成测试。

- failsafe:verify
  
  验证集成测试是否通过。

## 依赖信息

```xml
<dependency>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-failsafe-plugin</artifactId>
  <version>3.0.0-M6</version>
</dependency>
```

## 使用方法

```shell
mvn verify
```

## 配置

- 要使用此插件需要在pom.xml中如下配置：
  
  ```xml
  <project>
    [...]
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-failsafe-plugin</artifactId>
          <version>3.0.0-M6</version>
          <executions>
            <execution>
              <goals>
                <goal>integration-test</goal>
                <goal>verify</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
      </plugins>
    </build>
    [...]
  </project>
  ```

- 将jetty和failsafe插件集成：
  
  需要将 jetty:start、jetty:run、jetty:run-exploded 或 jetty:run-war 之一绑定到 pre-integration-test 阶段，并将守护进程设置为 true，
  
  将 failsafe:integration-test 绑定到 integration-test 阶段，
  
  将 jetty:stop 绑定到 post-integration-test 阶段，
  
  最后将 failsafe:verify 绑定到 verify 阶段。
  
  ```xml
  <project>
    [...]
    <build>
      [...]
      <plugins>
        [...]
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-failsafe-plugin</artifactId>
          <version>3.0.0-M6</version>
          <executions>
            <execution>
              <id>integration-test</id>
              <goals>
                <goal>integration-test</goal>
              </goals>
            </execution>
            <execution>
              <id>verify</id>
              <goals>
                <goal>verify</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
        <plugin>
          <groupId>org.eclipse.jetty</groupId>
          <artifactId>jetty-maven-plugin</artifactId>
          <version>9.2.2.v20140723</version>
          [...]
          <configuration>
            [...]
            <scanIntervalSeconds>10</scanIntervalSeconds>
            <stopPort>8005</stopPort>
            <stopKey>STOP</stopKey>
            [...]
          </configuration>
          [...]
          <executions>
            [...]
            <execution>
              <id>start-jetty</id>
              <phase>pre-integration-test</phase>
              <goals>
                <goal>start</goal>
              </goals>
              <configuration>
                <scanIntervalSeconds>0</scanIntervalSeconds>
                <daemon>true</daemon>
              </configuration>
            </execution>
            <execution>
              <id>stop-jetty</id>
              <phase>post-integration-test</phase>
              <goals>
                <goal>stop</goal>
              </goals>
            </execution>
            [...]
          </executions>
          [...]
        </plugin>
        [...]
      </plugins>
      [...]
    </build>
    [...]
  </project>
  ```

# install

## 描述

在安装阶段使用该插件将工件添加到本地存储库。

## 目标

- install:install
  
  将工件安装到本地存储库。

- install:install-file
  
  将不是用Maven创建的工件及其POM安装到本地存储库中。

## 依赖信息

```xml
<project>
  ...
  <build>
    <!-- To define the plugin version in your parent POM -->
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-install-plugin</artifactId>
          <version>3.0.0-M1</version>
        </plugin>
        ...
      </plugins>
    </pluginManagement>
    <!-- To use the plugin goals in your POM or parent POM -->
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-install-plugin</artifactId>
        <version>3.0.0-M1</version>
      </plugin>
      ...
    </plugins>
  </build>
  ...
</project>
```

## 使用方法

```shell
mvn install
```

## 配置

将不是由Maven构建的工件安装到本地：

```shell
mvn install:install-file -Dfile=your-artifact-1.0.jar \
                         [-DpomFile=your-pom.xml] \
                         [-Dsources=src.jar] \
                         [-Djavadoc=apidocs.jar] \
                         [-DgroupId=org.some.group] \
                         [-DartifactId=your-artifact] \
                         [-Dversion=1.0] \
                         [-Dpackaging=jar] \
                         [-Dclassifier=sources] \
                         [-DgeneratePom=true] \
                         [-DcreateChecksum=true]
```

# resources

## 描述

将项目资源复制到输出目录。分为main资源和test资源。

## 目标

- resources:resources
  
  将`<resources>`元素指定的main源代码的资源复制到输出目录，将不会影响测试代码的资源。

- resources:copy-resources
  
  将不在maven标准资源目录中或未在buid/resources元素中声明的资源输出到目录。

- resources:testResources
  
  将`<testResources>`元素指定的测试资源复制到输出目录。

## 依赖信息

```xml
<project>
  ...
  <build>
    <!-- To define the plugin version in your parent POM -->
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.2.0</version>
        </plugin>
        ...
      </plugins>
    </pluginManagement>
    <!-- To use the plugin goals in your POM or parent POM -->
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
        <version>3.2.0</version>
      </plugin>
      ...
    </plugins>
  </build>
  ...
</project>
```

## 使用方法

```shell
mvn resources:resources
```

## 配置

# site

## 描述

用于为项目生成站点。

## 目标

- site:attach-descriptor
  
  - 全名：
    
    `org.apache.maven.plugins:maven-site-plugin:3.11.0:attach-descriptor`
  
  - 描述：
    
    将站点描述符文件（site.xml）添加到要安装/部署的文件列表中。**该操作已经从Maven 3.x的默认生命周期中删除了。**
  
  - 可选参数：
    
    | 参数名                           | 类型      | 描述                                         |
    | ----------------------------- | ------- | ------------------------------------------ |
    | `<locales>`                   | String  | 使用逗号分隔的语言列表。默认值为：en                        |
    | `<pomPackagingOnly>`          | boolean | 仅打包为pom时才附加站点描述符文件（site.xml）。默认为true       |
    | `<relativizeDecorationLinks>` | boolean | 在站点描述符中创建相对于项目URL的链接。默认为true               |
    | `<siteDirectory>`             | File    | 包含site.xml文件和源代码的目录。默认是${basedir}/src/site |
    | `<skip>`                      | boolean | 设置为true将跳过站点生成。默认为false                    |

- site:deploy
  
  - 全名：
    
    `org.apache.maven.plugins:maven-site-plugin:3.11.0:deploy`
