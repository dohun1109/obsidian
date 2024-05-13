
## HTTP 헤더 -용도 
- HTTP 필요한 모든 부가정보 
- 예) 메시지 바디의 내용, 메세지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버정보, 캐시 관리 정보 ... 
- 표준 헤더가 너무 많음 
- 필요시 임의의 헤더 추가  기능 
	- helloworld: hihi
 
## HTTP  헤더 분류 - RFC2616(과거)
- General 헤더 : 메시지 전체에 적용되는 정보 
- Request 헤더 : 요청 정보 
- Response 헤더 : 응답 정보 
- Entity 헤더 : 엔티티 바디 정보 
## HTTP BODY - message body - RFC2616(과거)
- 메시지 본문 (message body)은 엔티티 본문(entity body) 을 전달하는데 사용 
- 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터 
- **엔티티 헤더** 는 **엔티티 본문**의 데이터를 해석할 수 있는 정보 제공 
	- 데이터 유형 (html, json), 데이터 길이, 압축 정보 등등 

## RFC723X 변화 
- 엔티티(Entity) -> 표현(Representation)
- Representation = representation Metadata + Representation Data 
- 표현 = 표현 메타데이터 + 표현 데이터 
## HTTP BODY - message body - RFC7230(최신)
- 메시지 본문(message body)를 통해 표현 데이터 전달'
- 메시지 본문 = 페이로드 (payload)
- 표현은 요청이나 응답에서 전달할 실제 데이터 
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공 
	- 데이터 유형 ( html, json) , 데이터 길이 , 압축 정보 등등 
- 참고 : 표현 헤더는 표현 메타데이터와, 페이로드 메시지를 구분해야하지만,  여기서는 생략 

## 표현 
- Content - Type : 표현 데이터의 형식 
- Content - Encoding : 표현 데이터의 압축 방식 
- Content - Language : 표현 데이터의 자연 언어 
- Content - Length : 표현 데이터의 길이 

- 표현 헤더는 전송 , 응답 둘다 사용 

## 협상 (콘텐츠 네고시에이션) - 클라이언트가 선호하는 표현 요청
- Accept : 클라이언트가 선호하는 미디어 타입 전달
-  Accept - Charset : 클라이언트가 선호하는 문자 인코딩 
- Accept - Encoding : 클라이언트가 선호하는 압축 인코딩 
- Accept - Language : 클라이언트가 선호하는 자연언어 

- 협상 헤더는 요청시에만 사용

## 전송 방식 설명 
- 단순 전송 
- 압축 전송 
- 분할 전송 
- 범위 전송 

#### 단순 전송 - Content - Length 
- Content 에 대한 길이를 알수 있을 때 
#### 압축 전송 - Content-Encoding
#### 분할 전송 - Transfer - Encoding 
- 분할전송시 Content - Length X -> 처음에 길이가 예상이 안됨
#### 범위 전송 - Range, Content-Range





