# Set up Maven to Automatically Pull Artifacts from GitHub

You can set up Maven to automatically pull IBM Streams Runner for Apache
Beam artifacts directly from this GitHub repository.

1. In the application `pom.xml`, add the repository:

    ```xml
    <repositories>
      <repository>
        <id>ibm-streams-runner-release</id>
        <name>IBM Streams Runner for Apache Beam</name>
        <url>https://raw.github.com/IBMStreams/streamsx.beam/mvn-release/</url>
      </repository>
    </repositories>
    ```

2. The application can then specify Streams Runner artifacts as dependencies,
and Maven will be able to automatically download them:

    ```xml
    <dependency>
      <groupId>com.ibm.streams.beam</groupId>
      <artifactId>beam-distribution</artifactId>
      <version>1.2.1</version>
      <type>tar.gz</type>
    </dependency>
    ```

3. Add a plugin to automatically unpack the runner archive:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>3.1.1</version>
          <executions>
            <execution>
              <phase>generate-resources</phase>
              <goals>
                <goal>unpack-dependencies</goal>
              </goals>
              <configuration>
                <includeTypes>tar.gz</includeTypes>
                <includeArtifactIds>beam-distribution</includeArtifactIds>
                <outputDirectory>target/product</outputDirectory>
              </configuration>
            </execution>
          </executions>
        </plugin>
      </plugins>
    </build>
    ```

  The runner distribution will be automatically unpacked to the following
  path when running `mvn compile` or `mvn package`:

  ```
  target/product/com.ibm.streams.beam-1.2.1
  ```

  See this directory for more information on using the runner with your
  Beam application.
