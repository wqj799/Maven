Maven本质上是一个执行插件的框架，所有的工作都是由插件完成的。

构建时使用的插件在构建期间执行，配置在POM的`<build/>`元素中。

报告时使用的插件在站点生成期间执行，配置在POM的`<reporting/>`元素中。

# clean

类型：编译时插件

描述：当想要删除项目目录中构建时生成的文件时，可以使用Clean插件。

目标：

- clean:clean
  
  尝试清理在构建时工作目录中生成的的文件。默认情况下，它会查找并删除由以下属性配置的工作目录：
  
  1. project.build.directory
  
  2. project.build.outputDirectory
  
  3. project.build.testOutputDirectory
  
  4. project.reporting.outputDirectory

依赖库信息：

```xml
<dependency>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-clean-plugin</artifactId>
    <version>3.1.0</version>
</dependency>
```

配置：

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

类型：编译时插件

描述：用于编译项目的源代码。从3.0开始默认编译器是`javax.tools.JavaCompiler`。

目标：

- compiler:compile
  
  Maven生命周期中的compile阶段，用于编译源文件。

- compiler:testCompile
  
  Maven生命周期中的test-compile阶段，用于编译测试文件。

依赖信息：

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

使用方法：

```shell
mvn compile
mvn test-compile
```

配置：

- compilerVersion：用于指定插件将使用的编译器版本。需要同时将fork参数设置为true。

- executable：指定javac的路径。

- source，target：将项目编译为与当前使用的版本不同的版本。source用于说明编写源代码时JDK的版本，target用于说明源代码编译后JDK的版本。
  
  **注意：仅设置target参数并不能保证代码运行在指定版本的JRE上**

- release：从JDK9开始，javac可以接受release参数来指定要使用哪个JDK版本构建项目。**该参数可以确保按照指定的JDK版本编译代码。**

- meminitial，maxmem：设置初始内存大小和最大内存使用量。

- compilerArgs：将参数传递给javac编译器。

- compilerId：指定其它的编译器。

# deploy

类型：部署时插件

描述：将工件部署到远端存储库。

目标：

- deploy:deploy
  
  Maven生命周期中的deploy阶段，将工件部署到远端仓库。

- deploy:deploy-file
  
  将工件安装到远端仓库。

依赖库信息：

```xml
<dependency>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-deploy-plugin</artifactId>
  <version>3.0.0-M2</version>
</dependency>
```

使用方法：

```shell
mvn deploy
```

配置：

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


