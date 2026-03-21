## 목차
### **1.YAML이 무엇인가?**
### 2.**YAML의 기본 문법과 사용법**
### 3.**SpringBoot에서 YAML사용법**


### 1.YAML이 무엇인가?

YAML은 YAML Ain't Markup Language 의 약자이고 공식 홈페이지에서 기재 되어있는 YAML의 정의는 아래와 같습니다.  

```
YAML (YAML Ain't Markup Language) is a human-readable data serialization language designed for data exchange between languages with different data structures.

It's often used for configuration files, data exchange, and representing complex data structures in a readable format.
```

위의 내용을 번역을 하자면 아래와 같습니다. 

```
YAML(YAML Ain't Markup Language)은 **서로 다른 데이터 구조를 가진 언어** 간의 데이터 교환을 위해 설계된 사람이 읽을 수 있는 데이터 직렬화 언어입니다.

이는 주로 설정 파일, 데이터 교환, 그리고 복잡한 데이터 구조를 읽기 쉬운 형식으로 표현하는 데 사용됩니다. 
```

여기에서 서로 다른 데이터 구조를 가진 언어는 프로그래밍 언어마다 데이터를 표현하는 방식은 서로 다르다는 것을 말합니다. 예를 들어 Java의 객체, Python의 딕셔너리는 구조는 비슷하지만 표현 방법과 문법이 다릅니다.

YAML은 이러한 차이를 넘어서 데이터를 **공통된 텍스트 형식으로 표현**할 수 있도록 도와주는 포맷이며, 이 때문에 설정 파일이나 환경 구성과 같은 영역에서 널리 사용되고 있습니다.
### 2.YAML의 기본 문법과 사용법

YAML의 대표적인 특징은 다음과 같습니다. 

##### 1. 사람이 읽기 쉬운 형식 (Human-readable)

YAML은 사람이 직접 읽고 수정하기 쉽도록 설계된 데이터 표현 형식이다. 중괄호나 복잡한 문법 대신 들여쓰기를 사용하여 구조를 표현하기 때문에 설정 파일을 한눈에 이해하기 쉽습니다.

이러한 특징 때문에 애플리케이션 설정 파일에서 많이 사용됩니다.

##### 2. 언어 독립적 (Language Independent)

YAML은 특정 프로그래밍 언어에 종속되지 않는다. Java, Python, JavaScript 등 다양한 언어에서 YAML을 읽고 쓸 수 있는 라이브러리를 제공합니다.

즉, 서로 다른 언어로 작성된 시스템 간에도 동일한 형식으로 데이터를 교환할 수 있다는 것입니다.

##### 3. 계층 구조 지원 (Hierarchical Structure)

YAML은 들여쓰기를 이용하여 계층 구조를 표현할 수 있습니다.

예를 들어 다음과 같은 구조가 가능합니다.

`spring:   
	datasource:     
		url: jdbc:mysql://localhost:3306/test`

이처럼 복잡한 설정 구조도 비교적 읽기 쉽게 표현할 수 있습니다.

##### 4. 확장 가능 (Extensible)

YAML은 기본적인 데이터 타입뿐 아니라 사용자 정의 타입도 표현할 수 있도록 설계되어 있습니다. 일반적인 애플리케이션 설정에서는 자주 사용되는 기능은 아니지만, 복잡한 데이터 구조를 표현해야 하는 환경에서는 유용하게 활용될 수 있습니다.

##### 5. 보안 고려 (Security Considerations)

YAML은 단순한 텍스트 기반 포맷이기 때문에 실행 코드가 포함되지 않으며, 설정 파일 형태로 사용하기에 비교적 안전한 구조를 가집니다.

다만, YAML 파서를 사용할 때 신뢰할 수 없는 입력을 그대로 로딩하는 경우 보안 문제가 발생할 수 있으므로, 라이브러리 사용 시 안전한 로딩 옵션을 사용하는 것이 권장된다.

### 3. Spring Boot에서 YAML사용법

#### **3-1) 계층적 구조를 통한 가독성 향상**

`properties` 파일은 키 값을 매번 반복해서 적어야 하지만, YAML은 들여쓰기를 통해 중복을 제거하고 구조를 명확히 합니다.

- **Properties 방식**

```
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=admin
spring.datasource.password=1234
```
 
- **YAML 방식**

```
spring:
      datasource:
        url: jdbc:mysql://localhost:3306/test
        username: admin
        password: 1234
```

### **3-2) 프로필(Profile) 분리 및 통합 관리**

하나의 YAML 파일 내에서 `---` 구분자를 사용하여 로컬, 테스트, 운영 환경 설정을 분리하여 관리할 수 있습니다.

```
spring: 
	profiles: 
		active: dev # 현재 활성화할 프로필 
--- 
spring: 
	config: 
		activate: 
			on-profile: dev 
server: port: 8080 
--- 
spring: 
	config: 
		activate: 
			on-profile: prod 
server: port: 80
```


### 3-3) **리스트 및 객체 매핑 (@ConfigurationProperties)**

YAML은 리스트(`-`) 구조를 직관적으로 지원하며, 이를 자바 객체로 바인딩하여 안전하게 사용할 수 있습니다.

```
myapp: 
	kafka: 
	bootstrap-servers: 
		- localhost:9092 
		- localhost:9093 
    retry-topics:
	    - delay-1m 
	    - delay-5m
```

### 3-4) 환경 변수 및 기본값 활용

시스템 환경 변수를 주입받거나, 값이 없을 경우 기본값을 설정하는 유연함을 제공합니다.

```
spring: 
	datasource: password: ${DB_PASSWORD:default_password} # 환경 변수 DB_PASSWORD가 없으면 default_password 사용
```




참고 사이트

YAML 공식 사이트 (https://yaml.org/about/)