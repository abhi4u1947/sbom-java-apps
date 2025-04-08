# sbom-java-apps
CycloneDX SBOM for Java Applications

### Generating SBOM for Java Applications

This project demonstrates how to generate a Software Bill of Materials (SBOM) for Java applications using CycloneDX plugins for both Maven and Gradle. 
Below are the details on how these plugins are used and how they can be configured to include additional information in the SBOM.

---

#### **Maven-Based Projects**

For Maven-based projects, the [CycloneDX Maven Plugin](https://github.com/CycloneDX/cyclonedx-maven-plugin) is used to generate the SBOM.

##### **Configuration**
The plugin is configured in the `pom.xml` file as follows:

```xml
<plugin>
    <groupId>org.cyclonedx</groupId>
    <artifactId>cyclonedx-maven-plugin</artifactId>
    <version>2.9.1</version>
    <configuration>
        <projectType>application</projectType>
        <outputFormat>json</outputFormat>
        <outputName>sbom.cdx</outputName>
        <schemaVersion>1.6</schemaVersion>
    </configuration>
</plugin>
```

##### **How to Generate the SBOM**
Run the following Maven command to generate the SBOM:
```bash
mvn cyclonedx:makeAggregateBom
```

The SBOM will be generated in the `target` directory with the name `sbom.cdx.json`.

##### **Adding More Information to the SBOM**
To include additional metadata in the SBOM, you can configure the plugin with properties such as `organizationName`, `componentVersion`, or custom properties. Refer to the [CycloneDX Maven Plugin documentation](https://github.com/CycloneDX/cyclonedx-maven-plugin#configuration) for more details.

---

#### **Gradle-Based Projects**

For Gradle-based projects, the [CycloneDX Gradle Plugin](https://github.com/CycloneDX/cyclonedx-gradle-plugin) is used to generate the SBOM.

##### **Configuration**
The plugin is applied in the `build.gradle` file as follows:

```groovy
plugins {
    id 'org.cyclonedx.bom' version '1.7.4'
}

cyclonedxBom {
    outputFormat = 'json'
    outputName = 'sbom.cdx'
    schemaVersion = '1.6'
}
```

##### **How to Generate the SBOM**
Run the following Gradle command to generate the SBOM:
```bash
./gradlew cyclonedxBom
```

The SBOM will be generated in the `build/reports` directory with the name `sbom.cdx.json`.

##### **Adding More Information to the SBOM**
You can customize the SBOM by configuring additional properties such as `includeBomSerialNumber`, `includeLicenseText`, or `customProperties`. Refer to the [CycloneDX Gradle Plugin documentation](https://github.com/CycloneDX/cyclonedx-gradle-plugin#configuration) for more details.

---

By using these plugins, you can easily generate SBOMs for your Java applications and configure them to include detailed metadata about your project.