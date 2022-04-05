Maven本质上是一个执行插件的框架，所有的工作都是由插件完成的。

构建时使用的插件在构建期间执行，配置在POM的`<build/>`元素中。

报告时使用的插件在站点生成期间执行，配置在POM的`<reporting/>`元素中。

# clean

## 类型

编译时插件

## 描述

当想要删除项目目录中构建时生成的文件时，可以使用Clean插件。

## 目标

- clean:clean
  
  尝试清理在构建时工作目录中生成的的文件。默认情况下，它会查找并删除由以下属性配置的工作目录：
  
  1. project.build.directory
  
  2. project.build.outputDirectory
  
  3. project.build.testOutputDirectory
  
  4. project.reporting.outputDirectory

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
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.2.0</version>
        </plugin>
        ...
      </plugins>
    </pluginManagement>
    <!-- To use the plugin goals in your POM or parent POM -->
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-clean-plugin</artifactId>
        <version>3.2.0</version>
      </plugin>
      ...
    </plugins>
  </build>
  ...
</project>
  [...]
</project>
```

## 配置

- 删除额外的目录
  
  clean插件默认删除target目录，可以通过以下配置删除或保留额外的目录：
  
  ```xml
  <build>
    [...]
    <plugin>
      <artifactId>maven-clean-plugin</artifactId>
      <version>3.1.0</version>
      <configuration>
        <filesets>
          <fileset>
            <directory>some/relative/path</directory>
            <includes>
              <include>**/*.tmp</include>
              <include>**/*.log</include>
            </includes>
            <excludes>
              <exclude>**/important.log</exclude>
              <exclude>**/another-important.log</exclude>
            </excludes>
            <followSymlinks>false</followSymlinks>
          </fileset>
        </filesets>
      </configuration>
    </plugin>
    [...]
  </build>
  ```
  
  **directory参数中使用相对路径**

- 跳过测试：
  
  设置参数skip为true跳过测试。
  
  - 使用命令行：
    
    ```shell
    mvn clean -Dmaven.clean.skip=true
    ```
  
  - 在pom.xml中配置：
    
    ```xml
    <build>
      [...]
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
          <configuration>
            <skip>true</skip>
          </configuration>
        </plugin>
      [...]
    </build>
    ```

- 忽略清理时产生的错误信息：
  
  - 使用命令行：
    
    ```shell
    mvn clean -Dmaven.clean.failOnError=false
    ```
  
  - 在pom.xml中配置：
    
    ```xml
    <build>
      [...]
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
          <configuration>
            <failOnError>false</failOnError>
          </configuration>
        </plugin>
      [...]
    </build>
    ```

# compiler

## 类型

编译时插件

## 描述

用于编译项目的源代码。从3.0开始默认编译器是`javax.tools.JavaCompiler`。

## 目标

- compiler:compile
  
  Maven生命周期中的compile阶段，用于编译源文件。

- compiler:testCompile
  
  Maven生命周期中的test-compile阶段，用于编译测试文件。

## 依赖信息

```xml
<project>
  ...
  <build>
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.10.1</version>
          <configuration>
            <!-- put your configurations here -->
          </configuration>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
  ...
</project>
```

**从Maven3.0开始不指定插件版本号将发出警告。**

## 使用方法

```shell
mvn compile
mvn test-compile
```

## 配置

- compilerVersion：用于指定插件将使用的编译器版本。需要同时将fork参数设置为true。

- executable：指定javac的路径。

- source，target：将项目编译为与当前使用的版本不同的版本。source用于说明编写源代码时JDK的版本，target用于说明源代码编译后JDK的版本。
  
  **注意：仅设置target参数并不能保证代码运行在指定版本的JRE上**

- release：从JDK9开始，javac可以接受release参数来指定要使用哪个JDK版本构建项目。**该参数可以确保按照指定的JDK版本编译代码。**

- meminitial，maxmem：设置初始内存大小和最大内存使用量。

- compilerArgs：将参数传递给javac编译器。

- compilerId：指定其它的编译器。

# deploy

## 类型

编译时插件

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

## 类型

编译时插件

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

## 类型

编译时插件

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

## 类型



## 描述



## 目标



## 依赖信息



## 使用方法



## 配置
