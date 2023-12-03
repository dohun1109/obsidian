
# API URI 설계 
### URI (Uniform Resource Identifier)

- 회원 목록 조회 /read-member-list
- 회원 조회 /read-member-by-id
- 회원 등록 /create-member
- 회원 수정 /update-member
- 회원 삭제 /delete-member

## **설계 시 가장중요한 것은 리소스 식별!** 
# API URI 고민 
- 리소스의 의미는 뭘까?
	- 회원을 등록하고 수정하고 조회하는게 리소스가 아니다! 
	- 예) 미네랄을 캐라 -> 미네랄이 리소스
	- 회원이라는 개념 자체가 바로 리소스다. 
- 리소스를 어떻게 식별하는게 좋을까? 
	- 회원을 등록하고 수정하고 조회하는 것을 모두 배제
	- 회원이라느 리소스만 식별하면 된다. -> 회원 리소스를 URI에 맵핑 

# API URI 설계 
## 리소스 식별, URI 계층 구조 활용

- **회원**목록 조회 /members
- **회원**조회 /members/{id}
- **회원** 등록 /members/{id}
- 

