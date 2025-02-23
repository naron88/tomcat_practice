# **프로젝트 설정 및 Tomcat 서버 배포 방법**

이 문서는 IntelliJ에서 Jakarta EE 웹 애플리케이션을 생성하고, Tomcat 서버에 배포하는 방법을 설명합니다.

---

## **1. IntelliJ에서 프로젝트 생성**

### 1.1 **프로젝트 생성**
1. **File** → **New Project** 선택
2. **Generators** → **Jakarta EE** 선택
3. **Template** → **Web Application** 선택
4. **Application Server** 설정:
   - 오류 발생 시 (예: `Cannot find opt/homebrew/Cellar/tomcat/11.0.3/libexec/bin/`):
     1. (mac) `cd /opt/homebrew/Cellar`
     2. (mac) `sudo chmod -R 755 tomcat`
5. **Java 17** 선택 (`Project SDK`)
6. **Build System**: `Gradle` 선택
7. **Language**: `Java` 선택
8. **Additional Libraries and Frameworks**:
    - `Jakarta EE` 선택
    - `Servlet` 체크
9. **Finish** 클릭

---

## **2. Gradle 설정 (build.gradle)**

```css
plugins {
    id 'java'
    id 'war' // Servlet 실행을 위한 WAR 패키징
}

group = 'com.example'
version = '1.0-SNAPSHOT'

java {
    sourceCompatibility = JavaVersion.VERSION_17
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'jakarta.servlet:jakarta.servlet-api:5.0.0' // Servlet 5.0 지원
    providedRuntime 'org.apache.tomcat.embed:tomcat-embed-core:10.1.5'
}

tasks.withType(JavaCompile).configureEach {
    options.encoding = 'UTF-8'
}
```

이 설정은 Jakarta EE Servlet API 5.0과 Tomcat의 내장 서버를 사용하여 웹 애플리케이션을 배포할 수 있도록 도와줍니다.

---

## **3. Tomcat 서버 실행 및 배포**

### 3.1 **Tomcat 설정**

1. **Run** → **Edit Configurations...** 클릭
2. `+` 버튼 클릭 → **Tomcat Server** → **Local** 선택
3. **Tomcat 설치 경로** 설정:
   - 예: `C:\tomcat` 또는 `/usr/local/tomcat`
4. **Deployment 탭**에서 `+` 클릭 → **Artifact** 추가 → `war exploded` 선택
5. **Application context** 설정:
   - `/` 또는 원하는 경로로 설정 (`/myapp` 등)
6. **Apply** → **OK** 클릭

---
