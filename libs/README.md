# SAP Native Libraries — Required for Build and Runtime

This directory must contain the **proprietary SAP JARs** before the project can be compiled
or deployed. These files are **not included in source control** because they are licensed
SAP software.

## Files Required

| File | Description | Maven coordinate |
|------|-------------|-----------------|
| `sapjco3.jar` | SAP JCo Library (Java Connector) | `com.sap.conn.jco:sapjco3:3.1.9` |
| `sapidoc3.jar` | SAP IDoc Library | `com.sap.conn.idoc:sapidoc3:3.1.9` |
| `libsapjco3.dll` (Windows) / `libsapjco3.so` (Linux) | SAP JCo Native Library | n/a — must be on system PATH |

## How to Obtain the Libraries

1. Log in to **SAP ONE Support Launchpad**: https://launchpad.support.sap.com/
2. Search for SAP Note **1858741** (SAP Java Connector).
3. Download the SAP JCo package matching your OS and JDK version (64-bit, Java 11+).
4. Extract the archive — it contains `sapjco3.jar`, `sapidoc3.jar`, and the native `.dll`/`.so`.
5. Copy `sapjco3.jar` and `sapidoc3.jar` into **this directory** (`libs/`).
6. For the native library:
   - **Windows**: copy `sapjco3.dll` to `libs/` and add `libs/` to your system `PATH`, OR copy to `C:\Windows\System32`.
   - **Linux/macOS**: copy `libsapjco3.so` to `libs/` and set `LD_LIBRARY_PATH=<project>/libs`.

## Why These Errors Appear in Anypoint Studio

Anypoint Studio / Mule validates that the SAP Connector can locate:
- **JCo Library** (`sapjco3.jar`) → "JCo Library is missing"
- **IDoc Library** (`sapidoc3.jar`) → "IDoc Library is missing"
- **JCo Native Library** (`libsapjco3.dll`/`.so`) → "JCo Native Library is missing"

Once the three files above are placed in this `libs/` folder (and the native lib is on the PATH),
all SAP-related errors in `global.xml` and the Maven build error
`Failed to resolve com.sap.conn.jco:sapjco3:3.1.9 / com.sap.conn.idoc:sapidoc3:3.1.9`
will be resolved.

## pom.xml Reference

```xml
<sap.libs.dir>${project.basedir}/libs</sap.libs.dir>
```

The `system`-scope dependencies in `pom.xml` point to:
- `${project.basedir}/libs/sapjco3.jar`
- `${project.basedir}/libs/sapidoc3.jar`