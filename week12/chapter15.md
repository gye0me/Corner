# 💾 15장 AWS와 GCP로 배포하기

## 1. RESTful 웹 서비스의 개요

REST(Representational State Transfer) 원리를 사용하여 HTTP와 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일임.

* **구성 요소**:
* **Resource (자원)**: URI로 명시되는 서버의 고유 자원임.
* **Method (행위)**: GET, POST, PUT, PATCH, DELETE 등 HTTP 메서드를 사용함.
* **Representation (표현)**: 클라이언트와 서버가 주고받는 데이터 형태이며, 주로 JSON을 사용함.



## 2. 주요 HTTP 메서드 및 어노테이션

| 기능 | HTTP 메서드 | 스프링 어노테이션 |
| --- | --- | --- |
| 조회 | GET | `@GetMapping` |
| 생성 | POST | `@PostMapping` |
| 수정 | PUT / PATCH | `@PutMapping` / `@PatchMapping` |
| 삭제 | DELETE | `@DeleteMapping` |

## 3. 주요 구현 코드 예시

### 3.1 REST Controller 구성

`@RestController`는 `@Controller`와 `@ResponseBody`를 합친 것으로, 데이터를 JSON 형태로 반환할 때 사용함.

```java
@RestController
@RequestMapping("/api/members")
public class MemberRestController {

    @Autowired
    private MemberService memberService;

    // 전체 회원 조회 (GET)
    @GetMapping
    public List<Member> list() {
        return memberService.findAll();
    }

    // 특정 회원 조회 (GET /path-variable)
    @GetMapping("/{id}")
    public ResponseEntity<Member> getMember(@PathVariable Long id) {
        Member member = memberService.findById(id);
        return ResponseEntity.ok(member);
    }

    // 회원 등록 (POST)
    @PostMapping
    public ResponseEntity<Member> create(@RequestBody Member member) {
        Member savedMember = memberService.save(member);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedMember);
    }
}

```

### 3.2 JSON 요청/응답 처리

* **`@RequestBody`**: HTTP 요청의 Body(JSON)를 자바 객체로 변환함.
* **`@ResponseBody`**: 자바 객체를 HTTP 응답의 Body(JSON)로 변환함.
* **`ResponseEntity`**: 응답 데이터와 함께 HTTP 상태 코드(200, 201, 404 등)를 직접 제어할 때 사용함.

## 4. RESTful API 설계 규칙

1. 슬래시(`/`)는 계층 관계를 나타내는 데 사용함.
2. URI 마지막 문자로 슬래시를 포함하지 않음.
3. 하이픈(`-`)은 가독성을 위해 사용하되, 밑줄(`_`)은 사용하지 않음.
4. 모든 URI는 소문자를 사용함.
5. 파일 확장자는 URI에 포함하지 않으며, Accept 헤더를 활용함.

---
