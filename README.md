# smartstore_second-cert_killer
    ⚠️ 토이 프로젝트입니다.
    정상적으로 작동하지 않을 수 있습니다.

# 인증 과정
    특별한 경우가 아니면 Status Code를 기입하지 않았습니다.

## 1. 네이버 스마트스토어 로그인
* URL: https://sell.smartstore.naver.com/api/login
* Mathod: <span style="color: #000000; background-color:#ffdce0;">POST</span>
* Body
    ```json
    {
        "id":"", // 스마트스토어 아이디
        "pw":"", // 스마트스토어 패스워드
        "url":"https%3A%2F%2Fsell.smartstore.naver.com%2F%23",
        "captchaId":"" // 캡챠 없기 때문에 공란 (고정)
    }
    ```
* Status Code
    |Code|설명|
    |---|---|
    |200|로그인 성공 또는 실패|
    |400|존재하지 않는 회원|
* Response Header
    |Key|Value|
    |---|---|
    |X-NCP-LOGIN-INFO|URL 인코딩된 json포맷 데이터<br>내용 중 memberNo key의 value는 맴버 번호|


## 2. 인증 이메일 전송
* URL: https://sell.smartstore.naver.com/api/possession
* Mathod: <span style="color: #000000; background-color:#dcffe4;">GET</span>
* Params
    ```json
    {
        "_action":"emailAuthMemberMyself" // (고정)
    }
    ```
* Response (200)
    |Key|Value|
    |---|---|
    |iat|ㅁ?ㄹ|
    |token|인증 토큰(3단계에서 필요함)|

## 3. 인증번호 확인 (건너뛸 수 있음)
* URL: https://sell.smartstore.naver.com/api/possession
* Mathod: <span style="color: #000000; background-color:#dcffe4;">GET</span>
* Params
    ```json
    {
        "_action":"verify", // (고정)
        "checkCode":"",     // 이메일로 전송된 인증코드 6자리
        "token":"",         // 2단계의 인증 토큰 
        "type":"email"      // 인증 방법 (고정)
    }
    ```
* Response (200)
    |Key|Value|
    |---|---|
    |result|성공 시 ```true```, 실패 시 ```false```|

## 4. 인증
* URL: https://sell.smartstore.naver.com/api/login/second-cert
* Mathod: <span style="color: #000000; background-color:#ffdce0;">POST</span>
* Body
    ```json
    {
        "certificateInfo": {},
        "id": , // 1단계의 맴버 번호
        "certificateInfos": [
            {
                "type": "email",       // 인증 방법(고정)
                "checkCode": "900358", // 이메일로 전송된 인증코드 6자리
                "token": "",           // 2단계의 인증 토큰 
                "certificate": true    // (고정)
            }
        ]
    }
    ```
* Status Code
    |Code|설명|
    |---|---|
    |200|인증 성공|
    |400|인증 실패|
* Response (400)
    |Key|Value|
    |---|---|
    |code||
    |message||
    |timestamp||

