# Use Case

- UML에서 액터와 액터가 요구하여 시스템이 수행하는 행위를 의미한다.

# 객체 지향의 사실과 오해

### 객체지향의 이유?

    - 역할과 책임을 기반으로 안정적인 구조로 코드를 작성하기 위해서
        -> 재사용과 변경에 유연함을 가진다.

    >> 작은 단위로 나누는 방법은??

### 객체란?

책임과 역할을 바탕으로 타 객체와 메세지를 통해 협력한다

    - 캡슐화를 통해 자신이 맡은 부분은 자기만 책임진다. >> 변경을 외부로 노출시키면 안됨

객체는 상태, 행동, 식별자로 이뤄진다.
    - 행동을 기반으로 타 객체와 협력한다.

Layered Architecture
    

### Task

1. Google Code Style IntelliJ에 적용
    https://github.com/google/styleguide - Google Coding Style 참고
    file - settings - Editors -> Code Style -> Java -> 다운받은 xml Schema Import

---

2. [VO](https://tecoble.techcourse.co.kr/post/2020-06-11-value-object/)

값을 저장하는 객체로 동등성을 보장해야하며 불변객체이다.

- Equals / hashcode 를 구현하여 동등성을 보장
- setter 제공하지않고 불변객체(final)을 선언한다
    
    ```
    Entity는 DB와 1:1 매핑하는 객체다. PK를 기반으로 구별할 수 있다.
    DTO는 속성값이 가변적이며, 계층간 전송을 위한 객체이다.
    VO는 불변성을 강조한다.
        1. 동등성 비교된 VO는 이후 로직에서도 동일한 동등성 결과가 나온다
        2. 멀티 쓰레드 환경에서 안전하기 위해
        3. 정의 자체가 '값' 자체에 집중한다 -> 사용자의 집주소가 변경되면 => 새로운 집 주소지 기존집을 유지하지않는다.
    ```

---

3. [1급 컬렉션](https://f-lab.kr/insight/understanding-and-applying-first-class-collections?gad_source=1&gclid=CjwKCAjwjeuyBhBuEiwAJ3vuoYK-xFTSYwyHPJ9QW6hr6D2l3nr8HVRpkG8F6RrpE3oMBS4KeS1xUBoC0mIQAvD_BwE)

컬렉션을 래핑하여 단 하나의 멤버 변수만을 가지는 객체

- 객체의 상태와 행위를 한 곳에서 관리할 수 있게 해주며, 불변성을 보장 
- 컬렉션에 대한 조작을 제한하고, 관련 로직을 한 곳에서 관리

```java
    /**
     * 게임 결과 저장 일급 객체
     * 
     * 목적 : 컬렉션 조작 통제 / 로직 캡슐화 
    */ 
    public class GameResults {
        // 컬렉션 객체 하나를 가진다.
        private final List<GameResult> results = new ArrayList<>();

        // 로직 1 : 게임이 끝나면 결과를 저장한다.
        public void addResult(GameResult result) {
            results.add(result);
        }
        // 로직 2 : 결과 목록을 반환한다.
        public List<GameResult> getResults() {
            return new ArrayList<>(results);
        }
    }
```

---

4. [객체지향 생활 체조](https://jamie95.tistory.com/99)

- 한 메서드에 오직 하나의 들여쓰기만 한다.
- else 키워드를 쓰지 않는다.
- 모든 원시값과 문자열은 포장(wrap)한다.
- 한 줄에 점 하나만 찍는다.
- 줄여쓰지 않는다.
- 모든 Entity는 작게 유지한다.
- 2개 이상의 인스턴스 변수를 가진 클래스를 쓰지 않는다.
- 일급 컬렉션을 사용한다.
- getter/setter/property를 쓰지 않는다.(Entity)

**리팩토링시 필요한 개념이기 때문에 나중에 고려하자!


# 과제

### **유즈케이스 - 과외 수강생 관리 프로그램 구현 기능**

1. 일별 수업 정보 반환:
    - 요일별 수업 반환
        - 요일은 상수로 정의하며, 콘솔을 통해 입력받는다.
    - 학생(비활성화) => 반환 X

행동 기반 추출

    1. 필터 조건 입력
    2. 유효성 검증
    3. 데이터 조회
    4. 결과 반환

---
2. 수강생들 상태 변경 가능:
    - 학생(활성) => 일별 수업에서 포함
    - 학생(비활성화) => 일별 수업 포함 X
    - 동일 상태로 전환 금지

행동 기반 추출

    1. 상태 변경 정보 입력
    2. 데이터 조회
    3. 유효성 검증
    4. 데이터 변경
    5. 변경 결과 반환

---
3. 수강생들의 수강료 변경 가능:
    - 특정 학생 수강료 변경 시 해당 학생 수업 전체에 적용

        => 수강료는 시간당 수업료 변경으로 생각하자

    1. 수강료 변경 정보 입력
    2. 데이터 조회
    3. 학생 수강료 데이터 변경
    4. 학생 월간 수강료 조회
    5. 반환



# 실습 결과

VO를 Entity에 적용하면서 Fee 객체가 새로 생성되며 이를 Entity에 도입됐다. 하지만 이 과정이 서비스에서 요금 변경 시 Entity에서 Fee를 새로 갈아끼워주는데, 이게 맞는 형식인지 잘 모르겠다.