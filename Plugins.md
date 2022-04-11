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
    
    | 参数名                       | 类型        | 最低版本  | 描述                                                                                                                                             |
    | ------------------------- | --------- | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
    | excludeDefaultDirectories | boolean   | 2.3   | 禁止清理默认的输出目录。如果设置为true，则只能清理使用`<filesets>`标签配置的目录。<br/>默认为false。                                                                                |
    | failOnError               | boolean   | 2.2   | 当清理时出现错误是否继续执行。<br/>默认为true。                                                                                                                   |
    | fast                      | boolean   | 3.2   | 是否启用快速清理。当为true时，则插件在执行时，将自动把要清理的目录移动到maven.clean.fastDir目录中，并启动一个线程在后台清理该目录。构建完成后，maven会等待此线程清理结束。如果在移动过程中出现错误，则插件将使用默认的传统清理机制。<br/>默认为false。 |
    | fastDir                   | File      | 3.2   | 当指定`<fast>`标签为true时，此属性将指定移动的位置。如果未指定，将使用`${maven.multiModuleProjectDirectory}/target/.clean`目录。如果`${build.directory}`已被修改，则必须明确调整此属性。         |
    | fastMode                  | String    | 3.2   | 使用快速清理时的模式：值为background时，立即开始清理操作并在会话结束时等待清理操作完成。值为at-end时，表示会话结束时应该同步执行清理操作。值为defer时，表示会话结束时应该在后台启动清理操作。<br/>默认为background。                   |
    | filesets                  | Fileset[] | 2.1   | 除默认目录外，需要被清理的文件列表。使用方法参考※注1                                                                                                                    |
    | followSymLinks            | boolean   | 2.1   | 设置插件从默认输出目录中删除文件使用应遵循符号链接。<br/>默认为false。                                                                                                       |
    | retryOnError              | boolean   | 2.4.2 | 删除文件失败后是否进行重试。<br/>默认为true。                                                                                                                    |
    | skip                      | boolean   | 2.2   | 是否跳过执行此插件。<br/>默认为false。                                                                                                                       |
    | verbose                   | boolean   | 2.1   | 设置是否已详细模式运行此插件。                                                                                                                                |
    
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
    
    | 参数名                           | 类型       | 最低版本   | 描述                                                                                                                                                                                                                                                                       |
    | ----------------------------- | -------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | annotationProcessorPaths      | List     | 3.5    | 指定注解处理器的类路径。如果不指定，则使用默认的注解处理器类路径。参考※注2                                                                                                                                                                                                                                   |
    | annotationProcessors          | String[] | 2.2    | 要运行的注解处理器的名称。                                                                                                                                                                                                                                                            |
    | compilerArgs                  | List     | 3.1    | 设置要传递给编译器的参数。**注意仅当 fork 设置为 true 时才会传递 -J 选项。** 参考※注3                                                                                                                                                                                                                   |
    | compilerArgument              | String   | 2.0    | 设置要传递给编译器的未格式化的单个字符串参数。要传递多个参数，如-Xmaxerrs 1000（这里实际是两个参数），必须使用compilerArgs。**注意仅当 fork 设置为 true 时才会传递 -J 选项。**                                                                                                                                                           |
    | compilerId                    | String   | 2.0    | 要使用的编译器的ID。<br/>默认为javac                                                                                                                                                                                                                                                 |
    | compilerReuseStrategy         | String   | 2.5    | 复用javac类的策略：<br/>reuseCreate（默认）：多线程构建的情况下，每个线程将有单独的实例。<br/>reuseSame：多线程构建下，每次编译都将使用相同的javac类。<br/>alwaysNew：每次编译都会创建一个新的javac类。                                                                                                                                        |
    | compilerVersion               | String   | 2.0    | 指定编译器版本。注意fork需要设置为true。                                                                                                                                                                                                                                                 |
    | createMissingPackageInfoClass | boolean  | 3.10   | 包信息源文件只包含 javadoc 而包上没有注解可能导致编译器不生成类文件。 这会导致下一次编译时文件丢失并强制进行不必要的重新编译。 默认值 true 会导致生成一个空的类文件。 可以通过将此参数设置为 false 来更改此行为。                                                                                                                                                    |
    | debug                         | boolean  | 2.0    | 设置为 true 以在编译的类文件中包含调试信息。<br/>默认为true。                                                                                                                                                                                                                                   |
    | debugFileName                 | String   | 3.10.0 | 当forking和debug为true时，命令行将转储到此文件。                                                                                                                                                                                                                                         |
    | debuglevel                    | String   | 2.1    | 要附加到 -g 命令行开关的关键字列表。 合法值是 none 或以下关键字的逗号分隔列表：lines、vars 和 source。 如果未指定调试级别，默认情况下，-g 不会附加任何内容。 如果未打开调试，则该属性将被忽略。                                                                                                                                                         |
    | enablePreview                 | boolean  | 3.10.1 | 设置为 true 以启用 java 编译器的预览语言功能。<br/>默认为false。                                                                                                                                                                                                                              |
    | encoding                      | String   | 2.1    | Java 编译器的 -encoding 参数。                                                                                                                                                                                                                                                  |
    | excludes                      | Set      | 2.0    | 编译器的排除过滤器列表。                                                                                                                                                                                                                                                             |
    | executable                    | String   | 2.0    | 设置Fork为True时使用编译器的可执行文件。                                                                                                                                                                                                                                                 |
    | failOnError                   | boolean  | 2.0.2  | 指示即使出现编译错误，构建是否会继续。<br/>默认为true。                                                                                                                                                                                                                                         |
    | failOnWarning                 | boolean  | 3.6    | 指示即使出现编译警告，构建是否会继续。<br/>默认为false。                                                                                                                                                                                                                                        |
    | fileExtensions                | List     | 3.1    | 用于检查增量构建时间戳的文件扩展名。 默认只包含class和jar。                                                                                                                                                                                                                                       |
    | forceJavacCompilerUse         | boolean  | 3.0    | 编译器现在可以使用 javax.tools （如果在您当前的 jdk 中可用）。<br/>默认为false。                                                                                                                                                                                                                   |
    | fork                          | boolean  | 2.0    | 允许在单独的进程中运行编译器。 如果为假，则使用内置编译器，如果为真，则使用可执行文件。<br/>默认为false。                                                                                                                                                                                                               |
    | generatedSourcesDirectory     | File     | 2.2    | 指定由注解创建的生成源文件的位置。 仅适用于 JDK 1.6+                                                                                                                                                                                                                                          |
    | includes                      | Set      | 2.0    | 编译器的包含过滤器列表。                                                                                                                                                                                                                                                             |
    | jdkToolchain                  | Map      | 3.6    | 指定此 jdk 工具链的要求，以使用与 Maven 使用的 JRE 不同的 javac。参考※注4                                                                                                                                                                                                                        |
    | maxmem                        | String   | 2.0.1  | 设置内存分配池的最大大小（以 MB 为单位）。                                                                                                                                                                                                                                                  |
    | meminitial                    | String   | 2.0.1  | 内存分配池的初始大小（以 MB 为单位）。                                                                                                                                                                                                                                                    |
    | multiReleaseOutput            | boolean  | 3.7.1  | 当设置为 true 时，类会放在 META-INF/versions/${release} 中，必须设置 release 值，否则插件会失败。                                                                                                                                                                                                  |
    | outputFileName                | String   | 2.0    | 将一组源编译为单个文件时设置输出文件的名称。 表达式="${project.build.finalName}"                                                                                                                                                                                                                  |
    | parameters                    | boolean  | 2.0    | 设置为 true 以生成元数据以反映方法参数。<br/>默认为false。                                                                                                                                                                                                                                    |
    | proc                          | String   | 2.2    | 设置是否执行注释处理。 仅适用于 JDK 1.6+ 如果不设置，编译和注解处理同时进行。<br/>允许的值为<br/>none：不执行注释处理。<br/>only：只进行注解处理，不进行编译。                                                                                                                                                                         |
    | release                       | String   | 3.6    | Java 编译器的 -release 参数，从 Java9 开始支持                                                                                                                                                                                                                                       |
    | showDeprecation               | boolean  | 2.0    | 设置是否显示使用已弃用 API 的源位置。                                                                                                                                                                                                                                                    |
    | showWarnings                  | boolean  | 2.0    | 设置为 true 以显示编译警告。                                                                                                                                                                                                                                                        |
    | skipMain                      | boolean  | 2.0    | 将此设置为“真”以绕过main资源的编译。                                                                                                                                                                                                                                                    |
    | source                        | String   | 2.0    | Java 编译器的 -source 参数。                                                                                                                                                                                                                                                    |
    | staleMillis                   | int      | 2.0    | 设置最后修改日期的粒度（以毫秒为单位），以测试源是否需要重新编译。                                                                                                                                                                                                                                        |
    | target                        | String   | 2.0    | Java 编译器的 -target 参数。                                                                                                                                                                                                                                                    |
    | useIncrementalCompilation     | boolean  | 3.1    | 启用/禁用增量编译功能。<br/>这会导致两种不同的模式，具体取决于底层编译器。 默认的 javac 编译器执行以下操作：<br/>true （默认）在此模式下，编译器插件确定当前模块所依赖的任何 JAR 文件在当前构建运行中是否已更改； 或自上次编译以来添加、删除或更改的任何源文件。 如果是这种情况，编译器插件会重新编译所有源代码。<br/>false （不推荐）这只编译比其相应类文件更新的源文件，即自上次编译以来已更改的源文件。 这不会重新编译使用更改后的类的其他类，可能会使它们引用不再存在的方法，从而导致运行时出错。 |
    | verbose                       | boolean  | 2.0    | 设置为 true 以显示有关编译器正在做什么的消息。<br/>默认为false。                                                                                                                                                                                                                                 |
    
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
  
  [Apache Maven Compiler Plugin – compiler:testCompile](https://maven.apache.org/plugins/maven-compiler-plugin/testCompile-mojo.html)

# deploy

- deploy:deploy
  
  - 版本信息
    
    `org.apache.maven.plugins:maven-deploy-plugin:3.0.0-M2:deploy`
  
  - 描述
    
    将工件部署到远程存储库。
  
  - 参数
    
    | 参数名                             | 类型      | 最低版本 | 描述                                                                                                                                   |
    | ------------------------------- | ------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------ |
    | altDeploymentRepository         | String  | -    | 指定项目工件应部署到的替代存储库（除了在 distributionManagement 中指定的存储库）。<br/>格式：`id::url`<br/>id<br/>该 ID 可用于从 settings.xml 中获取正确的凭据<br/>url<br/>存储库的位置 |
    | altReleaseDeploymentRepository  | String  | 2.8  | 当项目有最终版本时使用的替代存储库。                                                                                                                   |
    | altSnapshotDeploymentRepository | String  | 2.8  | 当项目具有快照版本时使用的替代存储库。                                                                                                                  |
    | deployAtEnd                     | boolean | 2.8  | 每个项目是否应该在其自己的部署阶段或多模块构建结束时进行部署。 如果设置为 true 并且构建失败，则不会部署任何项目。                                                                         |
    | retryFailedDeploymentCount      | int     | 2.7  | 用于控制失败部署在放弃和失败之前重试多少次的参数。                                                                                                            |
    | skip                            | String  | 2.4  | 将此设置为 'true' 以绕过工件部署。自 3.0.0-M2 以来，它不再是一个布尔值，因为它可以有两个以上的值：                                                                           |
    | <br/>true：会像往常一样跳过              |         |      |                                                                                                                                      |
    | <br/>release：如果项目的当前版本是发布，则将跳过  |         |      |                                                                                                                                      |
    | <br/>snapshots：如果项目的当前版本是快照，将跳过 |         |      |                                                                                                                                      |
    | <br/>任何其他值将被视为false。            |         |      |                                                                                                                                      |

- deploy:deploy-file
  
  - 版本信息
    
    `org.apache.maven.plugins:maven-deploy-plugin:3.0.0-M2:deploy-file`
  
  - 描述
    
    在远程存储库中部署工件。
  
  - 必须参数
    
    | 参数名          | 类型     | 最低版本 | 描述                                                                 |
    | ------------ | ------ | ---- | ------------------------------------------------------------------ |
    | file         | File   | -    | 将要部署的文件。                                                           |
    | repositoryId | String | -    | server ID 映射到 settings.xml 的 <server> 部分下的 <id> 。                  |
    | url          | String | -    | 将部署工件的 URL。<br/>如：file:///C:/m2-repo 或 scp://host.com/path/to/repo |
  
  - 可选参数
    
    | 参数名                        | 类型      | 最低版本 | 描述                                                                             |
    | -------------------------- | ------- | ---- | ------------------------------------------------------------------------------ |
    | artifactId                 | String  | -    | 要部署的工件的 artifactId。                                                            |
    | classifier                 | String  | -    | 将classifier添加到工件。                                                              |
    | classifiers                | String  | -    | 一个逗号分隔的分类器列表，用于部署每个额外的工件。                                                      |
    | description                | String  | -    | 传递给生成的POM文件的描述。                                                                |
    | files                      | String  | -    | 每个要部署的额外辅助工件的逗号分隔文件列表。 如果类型或分类器中的条目数不匹配，则会引发错误。                                |
    | generatePom                | boolean | -    | 上传此工件的 POM。 如果没有提供 pom File 参数，将生成默认 POM。                                      |
    | groupId                    | String  | -    | 要部署的工件的 GroupId。 如果指定，则从 POM 文件中检索。                                            |
    | javadoc                    | File    | 2.6  | 工件的 API 文档。                                                                    |
    | packaging                  | String  | -    | 要部署的工件的类型。 如果指定了 POM 文件，则从 POM 文件的 packaging元素中检索。 如果未通过命令行或 POM 指定，则默认为文件扩展名。 |
    | pomFile                    | File    | -    | 与主要工件一起部署的现有 POM 文件的位置，由 ${file} 参数给出。                                         |
    | retryFailedDeploymentCount | int     | 2.7  | 用于控制部署失败在放弃和失败之前重试多少次的参数。                                                      |
    | sources                    | File    | 2.6  | 工件的源代码。                                                                        |
    | types                      | String  | -    | 每个要部署的额外辅助工件的类型的逗号分隔列表。 如果文件或分类器中的条目数不匹配，则会引发错误。                               |
    | version                    | String  | -    | 要部署的工件的版本。 如果指定，则从 POM 文件中检索。                                                  |

# failsafe

- failsafe:verity
  
  - 版本信息
    
    `org.apache.maven.plugins:maven-failsafe-plugin:3.0.0-M6:verify`
  
  - 描述
    
    验证使用 Surefire 运行的集成测试。
  
  - 必须参数
    
    | 参数名         | 类型   | 最低版本 | 描述                                                                                           |
    | ----------- | ---- | ---- | -------------------------------------------------------------------------------------------- |
    | summaryFile | File | -    | 从中读取集成测试结果的摘要文件。<br/>默认值为：`${project.build.directory}/failsafe-reports/failsafe-summary.xml` |
    
    
  
  - 可选参数
    
    | 参数名                  | 类型      | 最低版本          | 描述                                                    |
    | -------------------- | ------- | ------------- | ----------------------------------------------------- |
    | basedir              | File    | -             | 正在测试的项目的基本目录。                                         |
    | failIfNoTests        | boolean | 2.4           | 将此参数设置为true，如果没有要运行的测试，将导致失败。 <br/>默认值为：false         |
    | failOnFlakeCount     | int     | 3.0.0-M6      | 请将其设置为大于 0 的值，如果flake的累积数量达到此阈值，以使整个测试集失败。<br/>默认值为：0 |
    | reportsDirectory     | File    | -             | 写入所有报告的基本目录。                                          |
    | skip                 | boolean | -             | 将此设置为true以完全绕过单元测试。                                   |
    | skipITs              | boolean | 2.4.3-alpha-2 | 将此设置为true以跳过正在运行的集成测试，但仍会编译它们。                        |
    | skipTests            | boolean | 2.4           | 将此设置为true以跳过正在运行的测试，但仍编译它们。                           |
    | summaryFiles         | File[]  | 2.6           | 用于读取集成测试结果的附加摘要文件。                                    |
    | testClassesDirectory | File    | -             | 包含正在测试的项目的生成测试类的目录。 这将包含在测试类路径的开头。                    |
    | testFailureIgnore    | boolean | -             | 将此设置为 true 以在测试期间忽略失败。                                |

# install

- install:install
  
  - 版本信息
    
    `org.apache.maven.plugins:maven-install-plugin:3.0.0-M1:install`
  
  - 描述
    
    将项目的主要工件以及生命周期中其他插件附加的任何其他工件安装到本地存储库。
  
  - 参数
    
    | 参数名          | 类型      | 最低版本 | 描述                                                             |
    | ------------ | ------- | ---- | -------------------------------------------------------------- |
    | installAtEnd | boolean | 2.5  | 每个项目是否应该在其自己的安装阶段或在多模块构建结束时安装。 如果设置为 true 并且构建失败，则不会安装任何反应器项目。 |
    | skip         | boolean | 2.4  | 将此设置为 true 以绕过工件安装。 将此用于不需要安装在本地存储库中的工件。                       |

## resources

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
