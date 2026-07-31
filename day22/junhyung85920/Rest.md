# Rest 란?

Date: 2026년 7월 31일
Status: Done

# 개념

<aside>
📜

**Rest (Representational State Transfer)**

자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것

</aside>

---

# REST의 모든 것

## 구성 요소

- **Resource : HTTP URI**
- **Verb : HTTP Method**
- **Representations : HTTP Message Pay Load**

## 특징

- **Client - Server**
- **Stateless**
- **Cacheable**
- **Layered System**
- **Uniform Interface**

## **장단점**

- **장점**
    - **HTTP 인프라 및 표준을 그대로 사용 → 별도의 인프라 구축 필요 X, qjadydtjd qhwkd**
    - **의도하는 바가 명확함**
    - **여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화 할 수 있음**
- **단점**
    - **HTTP Method 형태가 제한적**
    - **구형 브라우저 호환성**

# REST API

**REST의 원리를 따르는 API**

**아래 규칙들을 만족하는 서비스라면, RESTFUL 하다고 말할 수 있다.**

## 규칙

- **URI는 동사보다는 명사, 대문자보다는 소문자를 사용**
    
    ```
    Bad Example https://github.com/Omijapa/cs-study/Running
    Good Example  https://github.com/Omijapa/cs-study/run
    ```
    
- **마지막에 슬래시(/)를 포함하지 않음**
    
    ```
    Bad Example https://github.com/Omijapa/cs-study/
    Good Example  https://github.com/Omijapa/cs-study
    ```
    
- **언더바(_) 대신 하이픈(-) 사용**
    
    ```
    Bad Example https://github.com/Omijapa/cs-study/day_22
    Good Example  https://github.com/Omijapa/cs-study/day-22
    ```
    
- **파일 확장자는 URI에 포함하지 않음**
    
    ```
    Bad Example https://github.com/Omijapa/cs-study/photo.jpg
    Good Example  https://github.com/Omijapa/cs-study/photo
    ```
    
- **행위를 포함하지 않음**
    
    ```
    Bad Example https://github.com/Omijapa/cs-study/delete-post
    Good Example  https://github.com/Omijapa/cs-study/post
    ```