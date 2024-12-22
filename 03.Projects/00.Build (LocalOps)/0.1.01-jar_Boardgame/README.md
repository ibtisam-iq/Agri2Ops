# BoardgameListingWebApp

## Description

**Board Game Database Full-Stack Web Application.**
This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/ edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/ delete the reviews on top of the authorities of users.  

## Technologies

- Java
- Spring Boot
- Thymeleaf
- Thymeleaf Fragments
- HTML5
- CSS
- JavaScript
- Spring MVC
- JDBC
- H2 Database Engine (In-memory)
- JUnit test framework
- Spring Security
- Twitter Bootstrap
- Maven

## Features

- Full-Stack Application
- UI components created with Thymeleaf and styled with Twitter Bootstrap
- Authentication and authorization using Spring Security
  - Authentication by allowing the users to authenticate with a username and password
  - Authorization by granting different permissions based on the roles (non-members, users, and managers)
- Different roles (non-members, users, and managers) with varying levels of permissions
  - Non-members only can see the boardgame lists and reviews
  - Users can add board games and write reviews
  - Managers can edit and delete the reviews
- JUnit test framework for unit testing
- Spring MVC best practices to segregate views, controllers, and database packages
- JDBC for database connectivity and interaction
- CRUD (Create, Read, Update, Delete) operations for managing data in the database
- Schema.sql file to customize the schema and input initial data
- Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

## How to Run

1. Clone the repository
2. Open the project in your IDE of choice
3. Run the application
4. To use initial user data, use the following credentials.
  - username: ibtisam      |     password: 123 (user role)
  - username: ibtisamops   |     password: 12345  (manager role)
5. You can also sign-up as a new user and customize your role to play with the application! 😊

## Project Structure

Root Level

1. .github/workflows/maven.yml (GitHub Actions workflow file)
This file is used to configure GitHub Actions for CI/CD.
maven.yml is a workflow definition file that automates Maven-based build tasks (e.g., build, test, deploy).

2. .gitignore
Specifies files and directories that Git should ignore.
Typically includes files like logs, IDE settings, and other transient files.

3. .mvn/wrapper
maven-wrapper.properties: Configuration for the Maven wrapper, including version and distribution URL.

4. mvnw / mvnw.cmd
Shell and batch scripts for running the Maven wrapper on Unix/Linux (mvnw) or Windows (mvnw.cmd).

5. pom.xml
Maven's Project Object Model file.
Defines dependencies, plugins, and build configurations for the project.

6. README.md
A Markdown file providing project documentation, including purpose, setup instructions, and usage.

src/main: Main Application Code

    java/com/javaproject

        beans:
            Contains POJOs (Plain Old Java Objects) like BoardGame, ErrorMessage, and Review.
            These represent the data models used in the application.

        controllers:
            Houses Spring MVC controllers like BoardGameController and HomeController.
            These handle HTTP requests and map them to corresponding services.

        database:
            Contains classes like DatabaseAccess for interacting with the database (e.g., querying, updating).

        DatabaseServiceProjectApplication.java:
            The main entry point of the Spring Boot application.
            Initializes and configures the application.

        security:
            LoggingAccessDeniedHandler: Custom handler for denied access attempts.
            SecurityConfig: Configures Spring Security, including authentication and authorization rules.

    resources
        application.properties: Configuration file for the Spring Boot application (e.g., database URLs, server port).
        schema.sql: SQL file for initializing the database schema.
        static:
            Contains static files like CSS (style.css), JavaScript (script.js), and images (favicon.ico, spring-boot-logo.png).
        templates:
            Houses Thymeleaf templates for rendering views.
            Includes templates for various pages (e.g., boardgame.html, index.html, login.html).
            secured: Templates for secured areas (e.g., adding games, reviews).
            Subdirectories like manager and user contain role-specific templates.

src/test: Test Code

java/com/javaproject
  DatabaseServiceProjectApplicationTests.java: Integration tests for the application.
  TestController.java: Unit tests for controller logic.
  TestDatabase.java: Unit tests for database interactions.

Summary

Root-Level Files: Configurations for build tools (Maven)
src/main: Contains the application code:
  java: Core logic split into beans, controllers, database, and security. 
  resources: Configuration, database schema, static assets, and view templates.
src/test: Houses test cases for validating the application.

You said:

* **`.mvn/wrapper`** * **`maven-wrapper.jar`**: A wrapper for Maven, enabling the project to use a specific version of Maven without requiring it to be installed. * **`maven-wrapper.properties`**: Configuration for the Maven wrapper, including version and distribution URL. * **`mvnw` / `mvnw.cmd`** * Shell and batch scripts for running the Maven wrapper on Unix/Linux (`mvnw`) or Windows (`mvnw.cmd`).
please explain them in depth.

why are these? if let say, i remove them, will the project fail to run?

are these mandatory part of the java based projects like pom.xml?
ChatGPT said:
ChatGPT

Let’s delve into the components you’re referring to:
.mvn/wrapper

    maven-wrapper.properties

        Configuration file for the Maven Wrapper.

        Specifies:
            The Maven version to download.
            The distribution URL where Maven can be obtained.

        Impact of Removal:
            If removed, the Maven Wrapper cannot function because it doesn’t know which version of Maven to fetch.
            Again, this doesn’t break the project if global Maven is installed, but the version consistency across teams is lost.

mvnw / mvnw.cmd

    These are script files:
        mvnw: For Unix/Linux-based systems.
        mvnw.cmd: For Windows-based systems.

    Purpose:
        They allow you to run Maven commands (like mvn clean install) without relying on a globally installed Maven.
        Internally, these scripts use the Maven Wrapper (i.e., .mvn/wrapper/maven-wrapper.jar) to download and run Maven.

    Impact of Removal:
        If these are removed, developers must manually install Maven globally.
        The project itself won’t break as long as Maven is available, but convenience and uniformity are affected.

Are These Mandatory for Java Projects?

Not Mandatory, but strongly recommended for the following reasons:
- Consistency: Ensures all developers and CI/CD pipelines use the exact same Maven version.
- Ease of Use: Simplifies onboarding since developers don’t need to install Maven globally.
- Reproducibility: Helps avoid "it works on my machine" issues caused by Maven version differences.

Best Practice

Even though these files aren’t mandatory, it’s best to keep them in Java-based projects that use Maven. They offer:

- Version control for the build tool.
- Improved developer experience.
- Reliable builds across environments.

Would you like to explore how these files function in more depth or examples of their usage?
You said:
assume, i'm running this project as it on a jenkins server where maven tool is not configured in pipeline, nor it is installed locally on this machine?

then how's the flow going?
ChatGPT said:
ChatGPT

If Maven is not configured on the Jenkins server, either as a global installation or as part of the pipeline, the Maven Wrapper (mvnw, .mvn/wrapper/maven-wrapper.jar) becomes critical. Let me explain how the flow would work in this scenario:
Step-by-Step Flow

    Clone the Repository:
        Jenkins will clone your repository onto the Jenkins workspace.

    Execute the Build Command:
        If your Jenkinsfile contains something like:

        sh './mvnw clean install'

        This command will:
            Trigger the mvnw script located in your project directory.
            The mvnw script is a wrapper around Maven. It does the following:
                Checks if Maven is already installed locally in the workspace.
                If not, downloads the specific version of Maven defined in .mvn/wrapper/maven-wrapper.properties.

    Download Maven:
        Since the Jenkins server doesn’t have Maven installed, the Maven Wrapper will:
            Download the Maven binary to the local .mvn/wrapper directory in your project workspace.
            Use this binary for all Maven operations.

    Execute Maven Tasks:
        Once the required Maven version is downloaded, the build process continues:
            Read pom.xml: Maven reads this file to understand project dependencies, plugins, and build configuration.
            Resolve Dependencies: Maven downloads any missing dependencies specified in pom.xml.
            Run Build Steps: Tasks like compiling the code, running tests, packaging, etc., are executed as per the pom.xml.

    Produce Artifacts:
        If the build is successful, the output (e.g., JAR, WAR) is generated in the target/ directory.

Why Does This Work?

    Maven Wrapper:
        The mvnw script ensures Maven is automatically available, even if it isn’t pre-installed on the Jenkins server.
        The Maven version is controlled by .mvn/wrapper/maven-wrapper.properties.

    Self-Sufficient Project:
        With the Maven Wrapper, your project is self-sufficient. It doesn’t depend on external configurations or tools.

What Happens Without the Maven Wrapper?

If the Maven Wrapper (mvnw) is removed:

    Pipeline Will Fail: Jenkins will throw an error like mvn: command not found since Maven isn’t installed.

Why is Maven Wrapper Preferred?

    Automation: Avoids manual setup of Maven in the CI/CD pipeline.
    Consistency: Ensures the same Maven version is used across all environments (local, staging, production).
    Ease of Use: Eliminates the need for global Maven configuration.

Final Note

For a Jenkins server without Maven, the Maven Wrapper (mvnw) ensures your project remains portable and functional. If your project has a Jenkinsfile configured to invoke mvnw, you don’t need to worry about global Maven installations.

Would you like me to show how to write such a Jenkinsfile or explore alternatives?
You said:
please help me understand mvnw.


ChatGPT said:
ChatGPT

Let’s break this portion of the mvnw script step by step to understand its purpose and functionality:

1. Checking for maven-wrapper.jar

if [ -r "$BASE_DIR/.mvn/wrapper/maven-wrapper.jar" ]; then
    if [ "$MVNW_VERBOSE" = true ]; then
      echo "Found .mvn/wrapper/maven-wrapper.jar"
    fi
else

What’s Happening?
- The script checks if the maven-wrapper.jar file exists (-r tests if the file is readable).
- This file is critical for the Maven Wrapper to function since it contains the logic to download and invoke Maven.

Actions Based on the Check:
- If the file is found:
  - Optionally, it prints a message (Found .mvn/wrapper/maven-wrapper.jar) if the environment variable MVNW_VERBOSE is set to true.
- If the file is missing:
  - It moves to the else block to download the JAR file.

2. Handling Missing maven-wrapper.jar

if [ "$MVNW_VERBOSE" = true ]; then
  echo "Couldn't find .mvn/wrapper/maven-wrapper.jar, downloading it ..."
fi

What’s Happening?
- If the file is missing, the script logs a message indicating it will download the JAR file. This message is displayed only if MVNW_VERBOSE is set to true for debugging purposes.

3. Determining the Download URL

if [ -n "$MVNW_REPOURL" ]; then
  jarUrl="$MVNW_REPOURL/org/apache/maven/wrapper/maven-wrapper/3.1.0/maven-wrapper-3.1.0.jar"
else
  jarUrl="https://repo.maven.apache.org/maven2/org/apache/maven/wrapper/maven-wrapper/3.1.0/maven-wrapper-3.1.0.jar"
fi

What’s Happening?
The script determines where to download the maven-wrapper.jar file:
- If MVNW_REPOURL is set, it uses this custom repository URL to construct the JAR's download path.
- If not, it defaults to Maven Central (https://repo.maven.apache.org).

Key File Path:
The JAR is located in Maven repositories under:
  - org/apache/maven/wrapper/maven-wrapper/3.1.0/maven-wrapper-3.1.0.jar
  - The 3.1.0 version here refers to the version of the Maven Wrapper.

4. Overriding URL via Properties

while IFS="=" read key value; do
  case "$key" in (wrapperUrl) jarUrl="$value"; break ;;
  esac
done < "$BASE_DIR/.mvn/wrapper/maven-wrapper.properties"

What’s Happening?
- The script reads the maven-wrapper.properties file to check for a custom wrapperUrl value.
- If wrapperUrl is defined, it overrides the jarUrl determined earlier.

Purpose:
- Allows projects to specify an alternative location for the Maven Wrapper JAR, overriding defaults like Maven Central.

5. Logging the Download URL

if [ "$MVNW_VERBOSE" = true ]; then
  echo "Downloading from: $jarUrl"
fi

What’s Happening?
- The script logs the download URL for the maven-wrapper.jar file, providing visibility into where it’s fetching the JAR from. Again, this only happens if MVNW_VERBOSE is enabled.

6. Setting the Path for maven-wrapper.jar

wrapperJarPath="$BASE_DIR/.mvn/wrapper/maven-wrapper.jar"
if $cygwin; then
  wrapperJarPath=`cygpath --path --windows "$wrapperJarPath"`
fi

What’s Happening?
- Defines the location to save the downloaded maven-wrapper.jar file.
- If the script is running on a Cygwin environment (Windows compatibility layer for Unix tools), it converts the path to a Windows-compatible format using the cygpath command.

Summary of This Block

This portion of mvnw ensures that:

- The Maven Wrapper JAR exists. If it doesn’t:
  - It determines the appropriate download URL.
  - Downloads the JAR to .mvn/wrapper/maven-wrapper.jar.
- Customization is supported via:
  - The MVNW_REPOURL environment variable.
  - The wrapperUrl property in maven-wrapper.properties.
