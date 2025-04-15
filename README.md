# Generating Software Bill of Materials (SBOM) for Java Applications

## Table of Contents
- [Overview](#overview)
- [Maven-Based Projects](#maven-based-projects)
  - [Configuration](#maven-configuration)
  - [Key Configuration Options](#maven-key-configuration-options)
  - [Generating the SBOM](#maven-generating-the-sbom)
- [Gradle-Based Projects](#gradle-based-projects)
  - [Configuration](#gradle-configuration)
  - [Key Configuration Options](#gradle-key-configuration-options)
  - [Generating the SBOM](#gradle-generating-the-sbom)
- [Advanced Configuration](#advanced-configuration)
  - [Adding Metadata](#adding-metadata)
  - [Adding License Information](#adding-license-information)
  - [Version Compatibility](#version-compatibility)
- [CI/CD Integration](#cicd-integration)
- [References](#references)

## Overview

This guide explains how to generate Software Bill of Materials (SBOM) for Java applications using CycloneDX plugins for both Maven and Gradle build systems. An SBOM provides a detailed inventory of all components, libraries, and dependencies used in your software project, which is essential for security and compliance.

## Maven-Based Projects

### Maven Configuration

Add the CycloneDX Maven plugin to your `pom.xml`:

```xml
<plugin>
    <groupId>org.cyclonedx</groupId>
    <artifactId>cyclonedx-maven-plugin</artifactId>
    <version>2.9.1</version>
    <configuration>
        <projectType>application</projectType>
        <outputFormat>json</outputFormat>
        <outputName>application.cdx</outputName>
        <schemaVersion>1.6</schemaVersion>
    </configuration>
</plugin>
```

### Maven Key Configuration Options

| Option | Description | Default |
|--------|-------------|---------|
| `projectType` | Specifies the type of project (`application` or `library`) | `application` |
| `outputFormat` | Format of the SBOM (`json` or `xml`) | `json` |
| `outputName` | Name of the output SBOM file | `application.cdx` |
| `schemaVersion` | CycloneDX schema version | `1.6` |

### Maven Generating the SBOM

To generate the SBOM, run:

```bash
mvn cyclonedx:makeAggregateBom
```

The SBOM will be generated in the `target` directory with the specified output name.

## Gradle-Based Projects

### Gradle Configuration

Add the CycloneDX Gradle plugin to your `build.gradle` or `build.gradle.kts`:

```groovy
plugins {
    id 'org.cyclonedx.bom' version '2.2.0'
}

tasks.named('cyclonedxBom') {
    schemaVersion = "1.6"
    includeConfigs = ["runtimeClasspath", "compileClasspath"]
    skipProjects = [rootProject.name]
    projectType = "application"
    includeBomSerialNumber = true
    includeLicenseText = false
    destination = file("build/reports")
    outputName = "application.cdx"
    outputFormat = "json"
}
```

### Gradle Key Configuration Options

| Option | Description | Default |
|--------|-------------|---------|
| `schemaVersion` | CycloneDX schema version | `1.6` |
| `includeConfigs` | List of configurations to include | `["runtimeClasspath"]` |
| `skipProjects` | Projects to exclude | `[]` |
| `projectType` | Type of project | `application` |
| `includeBomSerialNumber` | Include unique identifier | `true` |
| `includeLicenseText` | Include full license text | `false` |
| `destination` | Output directory | `build/reports` |
| `outputName` | Output file name | `application.cdx` |
| `outputFormat` | Output format | `json` |

### Gradle Generating the SBOM

To generate the SBOM, run:

```bash
./gradlew cyclonedxBom
```

The SBOM will be generated in the specified destination directory (default: `build/reports`).

## Advanced Configuration

### Adding Metadata

Both plugins support adding additional metadata to the SBOM:

#### Maven Metadata Configuration
```xml
<configuration>
    <organizationalEntity>
        <name>Your Organization</name>
        <url>https://your-org.com</url>
        <contact>
            <name>Contact Name</name>
            <email>contact@your-org.com</email>
        </contact>
    </organizationalEntity>
</configuration>
```

#### Gradle Metadata Configuration
```groovy
cyclonedxBom {
    organizationalEntity { oe ->
        oe.name = 'Your Organization'
        oe.url = ['https://your-org.com']
        oe.addContact(organizationalContact)
    }
}
```

### Adding License Information

You can specify license information for your project:

#### Maven License Configuration
```xml
<configuration>
    <licenseChoice>
        <license>
            <name>Apache License 2.0</name>
            <url>https://www.apache.org/licenses/LICENSE-2.0</url>
        </license>
    </licenseChoice>
</configuration>
```

#### Gradle License Configuration
```groovy
cyclonedxBom {
    licenseChoice { lc ->
        def license = new License()
        license.setName("Apache License 2.0")
        license.setUrl("https://www.apache.org/licenses/LICENSE-2.0")
        lc.addLicense(license)
    }
}
```

### Version Compatibility

The following table shows the compatibility between plugin versions and CycloneDX schema versions:

| Plugin Version | Schema Version | Supported Formats |
|---------------|----------------|------------------|
| 2.x.x         | 1.6           | XML, JSON        |
| 1.10.x        | 1.6           | XML, JSON        |
| 1.9.x         | 1.6           | XML, JSON        |
| 1.8.x         | 1.5           | XML, JSON        |
| 1.7.x         | 1.4           | XML, JSON        |

## CI/CD Integration

Both plugins can be easily integrated into CI/CD pipelines. For example, in GitHub Actions:

```yaml
- name: Generate SBOM (Maven)
  run: mvn cyclonedx:makeAggregateBom

- name: Generate SBOM (Gradle)
  run: ./gradlew cyclonedxBom
```

The generated SBOMs can be found in:
- Maven: `target/application.cdx.json`
- Gradle: `build/reports/application.cdx.json`

## References

- [CycloneDX Gradle Plugin Documentation](https://github.com/CycloneDX/cyclonedx-gradle-plugin)
- [CycloneDX Maven Plugin Documentation](https://github.com/CycloneDX/cyclonedx-maven-plugin)
- [CycloneDX Specification](https://cyclonedx.org/specification/)
