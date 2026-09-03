# Maven A to Z Commands Cheat Sheet

A complete Apache Maven command reference covering beginner, intermediate, and advanced commands used by Java Developers, DevOps Engineers, Build Engineers, and CI/CD professionals.

---

# A. About Maven

## Check Maven Version

```bash
mvn --version
```

or

```bash
mvn -v
```

---

# B. Build Project

## Compile Project

```bash
mvn compile
```

## Clean and Compile

```bash
mvn clean compile
```

## Package Application

```bash
mvn package
```

## Install Artifact

```bash
mvn install
```

---

# C. Clean Commands

## Clean Target Directory

```bash
mvn clean
```

## Clean and Install

```bash
mvn clean install
```

## Clean and Package

```bash
mvn clean package
```

---

# D. Dependency Commands

## Download Dependencies

```bash
mvn dependency:resolve
```

## View Dependency Tree

```bash
mvn dependency:tree
```

## Analyze Dependencies

```bash
mvn dependency:analyze
```

## Copy Dependencies

```bash
mvn dependency:copy-dependencies
```

---

# E. Execute Goals

## Run Single Goal

```bash
mvn compile
```

## Run Multiple Goals

```bash
mvn clean package
```

---

# F. Force Updates

## Force Dependency Updates

```bash
mvn clean install -U
```

## Update Snapshots

```bash
mvn package -U
```

---

# G. Generate Project

## Create New Maven Project

```bash
mvn archetype:generate
```

## Generate Quickstart Project

```bash
mvn archetype:generate \
-DgroupId=com.example \
-DartifactId=myapp \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DinteractiveMode=false
```

---

# H. Help Commands

## Display Maven Help

```bash
mvn help:help
```

## Effective POM

```bash
mvn help:effective-pom
```

## Effective Settings

```bash
mvn help:effective-settings
```

## Active Profiles

```bash
mvn help:active-profiles
```

---

# I. Install Commands

## Install to Local Repository

```bash
mvn install
```

## Install Specific JAR

```bash
mvn install:install-file \
-Dfile=my.jar \
-DgroupId=com.company \
-DartifactId=myjar \
-Dversion=1.0 \
-Dpackaging=jar
```

---

# J. Java Compilation

## Compile Java Sources

```bash
mvn compiler:compile
```

## Compile Test Classes

```bash
mvn compiler:testCompile
```

---

# K. Known Lifecycle Phases

```bash
validate
compile
test
package
verify
install
deploy
```

Run example:

```bash
mvn verify
```

---

# L. List Dependencies

## Dependency Tree

```bash
mvn dependency:tree
```

## Verbose Dependency Tree

```bash
mvn dependency:tree -Dverbose
```

---

# M. Multi-Module Projects

## Build Parent and Child Modules

```bash
mvn clean install
```

## Build Specific Module

```bash
mvn -pl module1 install
```

## Build Module With Dependencies

```bash
mvn -pl module1 -am install
```

---

# N. Network & Repository Commands

## Offline Mode

```bash
mvn -o package
```

## Show Repository Information

```bash
mvn help:effective-settings
```

---

# O. Output & Logging

## Debug Output

```bash
mvn -X clean install
```

## Error Details

```bash
mvn -e clean install
```

## Quiet Mode

```bash
mvn -q package
```

---

# P. Profiles

## List Active Profiles

```bash
mvn help:active-profiles
```

## Activate Profile

```bash
mvn package -Pdev
```

## Multiple Profiles

```bash
mvn package -Pdev,uat
```

---

# Q. Quality Checks

## Run Checkstyle

```bash
mvn checkstyle:check
```

## Run PMD

```bash
mvn pmd:check
```

## Run SpotBugs

```bash
mvn spotbugs:check
```

---

# R. Run Tests

## Execute Tests

```bash
mvn test
```

## Run Specific Test

```bash
mvn -Dtest=UserServiceTest test
```

## Skip Tests

```bash
mvn package -DskipTests
```

## Skip Test Compilation

```bash
mvn package -Dmaven.test.skip=true
```

---

# S. Settings Commands

## Show Effective Settings

```bash
mvn help:effective-settings
```

## Use Custom Settings File

```bash
mvn clean install -s settings.xml
```

---

# T. Test Reports

## Generate Surefire Report

```bash
mvn surefire-report:report
```

## Generate Site

```bash
mvn site
```

---

# U. Update Dependencies

## Update Snapshot Dependencies

```bash
mvn clean install -U
```

## Purge Local Repository

```bash
mvn dependency:purge-local-repository
```

---

# V. Validate Project

## Validate Project Structure

```bash
mvn validate
```

## Verify Project

```bash
mvn verify
```

---

# W. War Packaging

## Build WAR File

```bash
mvn package
```

## Build Specific WAR

```bash
mvn war:war
```

---

# X. XML & POM Operations

## View Effective POM

```bash
mvn help:effective-pom
```

## Flatten POM

```bash
mvn flatten:flatten
```

---

# Y. YAML and CI/CD Usage

## Jenkins Build

```bash
mvn clean install
```

## GitHub Actions Build

```bash
mvn clean verify
```

## Azure DevOps Build

```bash
mvn package
```

---

# Z. Advanced Production Commands

## Deploy Artifact

```bash
mvn deploy
```

## Release Build

```bash
mvn release:prepare
```

```bash
mvn release:perform
```

## Generate Project Site

```bash
mvn site
```

## Analyze Project

```bash
mvn dependency:analyze
```

---

# Maven Lifecycle Commands

```bash
mvn validate
mvn compile
mvn test
mvn package
mvn verify
mvn install
mvn deploy
```

---

# Top 20 Maven Commands Used Daily

```bash
mvn clean
mvn compile
mvn test
mvn package
mvn install
mvn deploy
mvn clean install
mvn clean package
mvn clean verify
mvn dependency:tree
mvn dependency:analyze
mvn help:effective-pom
mvn help:effective-settings
mvn package -DskipTests
mvn test
mvn -X clean install
mvn -U clean install
mvn validate
mvn site
mvn release:prepare
```

---

# Maven Troubleshooting Commands

```bash
mvn -version
mvn -X clean install
mvn -e clean install
mvn dependency:tree
mvn help:effective-pom
mvn help:effective-settings
mvn dependency:analyze
mvn clean install -U
mvn dependency:purge-local-repository
mvn validate
```

---

# Common Maven Workflow

```bash
# Create Project
mvn archetype:generate

# Build
mvn clean compile

# Run Tests
mvn test

# Package Application
mvn package

# Install Locally
mvn install

# Deploy to Repository
mvn deploy
```

---

# Common Maven Lifecycle

```text
validate
    ↓
compile
    ↓
test
    ↓
package
    ↓
verify
    ↓
install
    ↓
deploy
```

---

# Summary

This cheat sheet covers the most important Maven commands used for:

- Java Development
- Spring Boot Projects
- Dependency Management
- Build Automation
- CI/CD Pipelines
- Jenkins Builds
- GitHub Actions
- Azure DevOps
- Multi-Module Projects
- Production Deployments

⭐ Keep this README as a quick reference guide for daily Maven build and deployment activities.
