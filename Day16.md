# [2022/05/15 회의록]
---

## 05/18 까지 내가 해야할 부분

 1. 관리자 페이지 탈퇴관리
    - DTO 만들기
    - 클래스 윤곽 잡기
    - MVC 패턴을 활용하여 회원 탈퇴 구현하기!

---

### [ MVC 비즈니스 로직 처리과정 - 탈퇴회원]

--------------------------- 요청 방향 ------------------------->

서블릿 요청 -> FrontController -> Action 클래스 -> Service 클래스 -> DAO 클래스

<-------------------------- 응답 방향 --------------------------

#### < 동작 흐름 >
1. 서블릿 주소 "/MemberExitForm.me" 입력시 
   컨트롤러에 의해 "member/exit.jsp" 페이지로 이동 (Dispatcher 방식으로 포워딩)

2. exit.jsp 페이지(창) 에서 탈퇴 버튼 클릭시
   탈퇴 작업 요청을 위한 서블릿주소("MemberExit.me") 요청

3. 서블릿주소 "MemberExitPro.me" 입력시 비즈니스로직 처리를 위해
   컨트롤러에 의해 Aciton(MembertExitProAction) 클래스의  인스턴스 생성 후 execute() 메서드 호출

4. MemberExitProAction 클래스의 execute() 메서드에서 탈퇴  준비 작업 후,
   비즈니스 작업 요청을 위해 MemberExitProService 클래스의 인스턴스 생성 및 _____  메서드 호출

5. MemberExitProSErviec 클래스의 _____ 메서드에서 
   비즈니스 로직 처리를 수행하기 위한 준비작업
   및 MemberDAO 클래스의 ___ Delete() 메서드 호출하여 DELETE 작업 수행

6. MemberExitProService 클래스에서 DAO의 DELETE 작업이 완료된 후
   결과갑을 리턴받아 판별,
	=> 실패시 rollback, 성공시  commit  작업 및 결과값을 true 로 변경 후 결과값 리턴

7. MemberExitProAction 클래스에서 MemberExitProService 로부터 리턴된 결과값을 리턴
	=> 실패시 "탈퇴하기를 실패하였습니다." 출력 후 이전페이지로 이동
	=> 성공시 ActionForward 객체를  사용해 index 서블릿 주소를 Redirect 방식으로 설정

8. 컨트롤러에서 MemberExitProAction 클래스로부터 리턴받은 ActionForward 객체를 판별하여 포워딩
