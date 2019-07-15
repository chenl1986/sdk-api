[toc]
# Java开发规范

## <font color="green">目录</font>
 

- 中文版: *[Java中文开发手册下载](https://github.com/alibaba/p3c/blob/master/%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C%EF%BC%88%E5%8D%8E%E5%B1%B1%E7%89%88%EF%BC%89.pdf)*
- 中文版: *[Java中文开发手册在线查看](https://github.com/alibaba/p3c/blob/master/p3c-gitbook/SUMMARY.md)*
- English Version: *[Java Coding Guidelines](https://alibaba.github.io/Alibaba-Java-Coding-Guidelines)*

## <font color="green">IDE集成</font>
IntelliJ IDEA/Eclipse integration 
- [IntelliJ IDEA plugin](https://github.com/alibaba/p3c/tree/master/idea-plugin)  
- [Eclipse plugin](https://github.com/alibaba/p3c/tree/master/eclipse-plugin)   

## <font color="green">阿里巴巴Java规约p3c-pmd与maven集成</font>
      <!-- 属性配置 -->
      <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>
      </properties>
      <build>
        <plugins>
    
          <!-- PMD插件 -->
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-pmd-plugin</artifactId>
            <version>3.8</version>
            <configuration>
              <sourceEncoding>${project.build.sourceEncoding}</sourceEncoding>
              <targetJdk>${java.version}</targetJdk>
              <printFailingErrors>true</printFailingErrors>
              <!-- 代码检查规则 -->
              <rulesets>
                <ruleset>rulesets/java/ali-comment.xml</ruleset>
                <ruleset>rulesets/java/ali-concurrent.xml</ruleset>
                <ruleset>rulesets/java/ali-constant.xml</ruleset>
                <ruleset>rulesets/java/ali-exception.xml</ruleset>
                <ruleset>rulesets/java/ali-flowcontrol.xml</ruleset>
                <ruleset>rulesets/java/ali-naming.xml</ruleset>
                <ruleset>rulesets/java/ali-oop.xml</ruleset>
                <ruleset>rulesets/java/ali-orm.xml</ruleset>
                <ruleset>rulesets/java/ali-other.xml</ruleset>
                <ruleset>rulesets/java/ali-set.xml</ruleset>
              </rulesets>
            </configuration>
            <executions>
              <!-- 绑定pmd:check到verify生命周期 -->
              <execution>
                <id>pmd-check-verify</id>
                <phase>verify</phase>
                <goals>
                  <goal>check</goal>
                </goals>
              </execution>
              <!-- 绑定pmd:pmd到site生命周期 -->
              <execution>
                <id>pmd-pmd-site</id>
                <phase>site</phase>
                <goals>
                  <goal>pmd</goal>
                </goals>
              </execution>
            </executions>
            <!-- p3c依赖 -->
            <dependencies>
              <dependency>
                <groupId>com.alibaba.p3c</groupId>
                <artifactId>p3c-pmd</artifactId>
                <version>1.3.5</version>
              </dependency>
            </dependencies>
          </plugin>
          <!-- 编译插件 Spring-Boot 项目不需要设置，因为parent中已设置 -->
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.7.0</version>
            <configuration>
              <source>${java.version}</source>
              <target>${java.version}</target>
            </configuration>
          </plugin>
        </plugins>
      </build>
    <!-- 用于生成错误到代码内容的链接 -->
      <reporting>
        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jxr-plugin</artifactId>
            <version>2.3</version>
          </plugin>
        </plugins>
      </reporting>

## <font color="green">单元测试</font>
- [单元测试规范](https://github.com/alibaba/p3c/blob/master/p3c-gitbook/%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95.md)
- [代码覆盖率测试工具Jcoco]()