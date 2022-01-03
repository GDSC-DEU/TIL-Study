## **2022/01/03 Today I Learn**


#### 스프링부트 시큐리티에서 가장중요한 개념은  '**`인증`**'과 '**`권한부여`**'
- 인증: 식별가능한 정보를 이용하여 사용자가 서비스를 사용할 수 있는 사용자인지 확인
    - 서비스에 등록된 사용자에게만 서비스를 제공

- 인가: 데이터나 프로그램 등의 특정 자원이나 서비스에 접근할 수 있는 권한을 허용하는 것

### **`oauth2`** 는 소셜인증, 회원가입을 생략할 수 있고 이미 가입된 소셜의 정보를 제공받아서 사용자 등록

<br></br>

### **Oauth2 전체 시퀀스 다이어그램**
<br/>
<p align="middle" >
  <img width="400px;" src="..\image\oauth2.jpg"/>
</p>
<br/>

<hr>

1. 사용자가 프론트엔드를 통해서 소셜로그인 요청

2. 프론트엔드에서 백엔드로 소셜로그인할 수 있는 URI를 이용하여 Oauth인가 요청

3. 백엔드는 소셜별 로그인화면의 URI을 리다이렉트해줌
4. 리다이렉트화면에서 로그인

5. 프론트엔드에서 Authorization Server로 백엔드의 리다이렉트 URI와 함께 Authorization Code를 요청한다.
    - Authorization Code요청하는 이유 사용자 정보에 접근할 수 있는 자격을 부여받기 위한 Access token을 받기 위해서
6. Authorization Server는 백엔드에게 Authorization Code를 준다.

7. 백엔드에서는 Authorization Server로 Authorization Code(권한 부여 코드)를 이용하여 Access token을 요청한다.
    - Access token은 로그인 세션에 대한 보안 자격을 증명하는 식별 코드

8. Authorization Server는 백엔드에게 access token을 준다.(발행한다)

9. 백엔드에서 access token를 이용하여 Resource Server에 사용자의 데이터를 요청

10. Resource Server에게 받은 데이터를 DB에 저장하고 JWT와 Refresh Token을 생성한다.
    - Resource server: kakap,facebook,google과 같은 사용자의 정보를 보관하고있는 서버
11. Refresh token을 db에 저장
    - Refresh Token은 Access Token과 똑같은 형태의 JWT, 처음에 로그인을 완료했을 때 Access Token과 동시에 발급되는 Refresh Token은 긴 유효기간을 가지면서, Access Token이 만료됐을 때 새로 발급해주는 열쇠가 됨

12. Refresh token을 쿠키에저장

13. 백엔드에서 프론트엔드 리다이렉트 URI에 access token을 담아 리다이텍트해준다.

14. 받은 토큰을 브라우저쿠키나 로컬 저장소에 저장한다.

15. 프론트엔드에서 토큰을 가지고 백엔드로 API 요청한다.

16. 백엔드에서는 토큰을 확인하고 사용자의 데이터를 전송

17. 사용자의 데이터를 프론트엔드에서 저장하여 사용자에게 보여준다.

**-토큰이 만료되었을때**
1. 프론트엔드에서 토큰을 가지고 백엔드로 API 요청한다.

2. 백엔드에서는 토큰을 확인하고 만료된 토큰이면 401을 프론트엔드에 전송

3. 401을 받은 프론트엔드는 토큰이 만료되었음을 알고 refreshToken을 api에 담아 백엔드에 전송

4. 백엔드에서는 AccessToken과 RefreshToken을 재발급해주고 새로 발급받은 RefreshToken을 DB에 저장한다

5. 프론트엔드에서는 새로 받은 RefreshToken을 cookie에 저장한다.



