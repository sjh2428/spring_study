## @NoArgsConstructor vs @AllArgsContructor vs @RequiredArgsConstructor

### @NoArgsConstructor

---

- parameters가 없는 생성자(기본생성자)를 생성하는 Annotation
- `final`이 붙어있는 fields가 존재해서 기본생성자를 만들 수 없다면 컴파일 에러
- 하지만 `(force = true)`라는 옵션을 추가하면 모든 final fields는 `0` / `false` / `null`로 초기화
  - ex) `@NoArgsConstructor(force = true)`
- `@NonNull`같이 필드에 제약조건이 있을 경우 생성자에서 null check로직이 발생하지 않음. 추후에 초기화를 진행하기 전까지 null check 로직이 발생하지 않는다는 것을 염두해두어야 함

### @RequiredArgsConstructor

---

- 초기화 되지 않은 모든 `final` 필드, `@NonNull`로 마크되어 있는( + 초기화 되지 않은) 필드들에 대한 생성자를 생성하는 Annotation
- `@NonNull`로 마크되어 있는 필드는 null check로직이 생성됨
- 생성자에서 `@NonNull`로 마크되어 있는 필드에 `null`이 들어온다면 `NullPointerException`이 발생
- params의 순서는 클래스에 있는 필드 순서와 맞춤

### @AllArgsContructor

---

- 클래스에 존재하는 모든 필드에 대한 생성자를 생성하는 Annotation
- `@NonNull`로 마크되어 있는 필드에는 null check로직 생성

### 생성자 Annotation을 사용할 때 참고&주의사항

---

- `staticName` 옵션으로 static factory method를 생성할 수 있음
  - ex) `@RequiredArgsConstructor(staticName = "of")`
- 생성자에 대하여 AccessLevel 설정 가능
  - 기본값은 `public`
  - 필요에 따라 접근제한자 설정
- `static` 필드들은 스킵됨
- 파라미터의 순서는 클래스에 있는 필드 순서에 맞춰서 생성하기 때문에 매우 주의해야 함
  - 클래스 필드의 순서를 바꾸었을 때 롬복이 자동적으로 생성자 파라미터 순서를 바꾸어주기 때문에 주의하지 않으면 버그 발생 가능

[롬복 공식문서](https://projectlombok.org/features/constructor)<br>
[참고 블로그](https://dingue.tistory.com/14)
