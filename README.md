## JWT(JSON Web Token)
JSON 객체로서 당사자 간에 안전하게 정보를 전송할 수 있는 작고 독립적인 방법을 정의하는 공개 표준 (RFC 7519)이다. 이 정보는 디지털로 서명되었기 때문에 신뢰할 수 있다. 암호(HMAC 알고리즘 사용) 또는 RSA를 사용하는 공용 / 개인 키 쌍을 사용하여 서명을 할 수 있다.

<h3>서버기반 인증의 문제점</h3>

세션의 경우 사용자의 인증 정보를 서버 메모리에 저장하게 된다. 이것은 서버의 자원을 사용하는 것이므로 사용자가 점차 증가하게 된다면, 서버의 자원들이 부족하게 될 것이다. 이 문제를 해결하기 위해서는 서버의 확장 및 업그레이드가 필요하며 각각 스케일 아웃, 스케일 업 이라고도 한다. 그리고 로드밸런싱을 위해 서버를 스케일 아웃 하는 경우 사용자는 한대의 서버로만 접속 되는 것이 아니라, 서로 다른 서버로 요청을 보내게 되는 경우가 있으므로 이때 사용자가 이전 상태를 유지하기 위해서는 서로 다른 서버 간의 세션 동기화 작업이 필요하다.

<h3>장점</h3>
<ul>
  <li>간편한 사용 및 인증 절차</li>
  <li>Horizontal Scalable</li>
  <li>REST API에 최적화된 키 인증 방식</li>
  <li>Expired Date 정보의 포함</li>
  <li>관리와 서버 부하에 있어 부담이 적음</li>
</ul>
<h3>단점</h3>
<ul>
  <li>인증 정보가 커질 수록 토큰의 크기가 커진다.</li>
  <li>모든 요청에 토큰 정보가 포함되므로 Token의 크기가 Message의 크기보다 커질 경우 비효율적인 통신 구조가 된다.</li>
</ul>

<h3>JWT 구조</h3>
<ul>
  <li>
    Header
<pre>
{
  "alg":"HS256",
  "typ":"JWT"
}
</pre>
  </li>
  <li>
    Payload
<pre>
{
  "loggedInAs":"admin",
  "iat":1422779638
}
</pre>
  </li>
  <li>
    Signature : Header와 Payload를 Base64로 인코딩 후 합쳐진다.
<pre>
key = 'secretkey' 
unsignedToken = encodeBase64Url(header) + '.' + encodeBase64Url(payload) 
signature = HMAC-SHA256(key, unsignedToken)
</pre>
  </li>
</ul>

<h3>JWT Claim</h3>

토큰의 정보를 클레임이라고 부르기 때문에 이 정보를 가지고 있는 바디 부분을 Claim Set이라 부르고 Claim Set은 키 부분인 Claim Name과 값 부분인 Claim Value의 여러 쌍으로 이루어져 있다. Claim Name으로 사용 가능한 값에는 3가지 분류가 있는데 등록된 클레임 이름(Registered), 공개 클레임 이름(Public), 비밀 클레임 이름(Private)이다. 등록된 클레임 이름은 IANA JSON Web Token Claims에 등록된 이름이고 필수 값은 아니지만 공통으로 사용하기 위한 기본 값이 정해져 있다. 아래 목록이 등록된 클레임 이름이며 모두 선택사항이다.

<ul>
  <li>iss : 토큰을 발급한 발급자(Issuer)</li>
  <li>sub : Claim의 주제(Subject)로 토큰이 갖는 문맥을 의미</li>
  <li>aud : 이 토큰을 사용할 수신자(Audience)</li>
  <li>exp : 만료시간(Expiration Time), 만료시간이 지난 토큰은 거절해야 함</li>
  <li>nbf : Not Before의 의미로 이 시간 이전에는 토큰을 처리하지 않아야 함</li>
  <li>iat : 토큰이 발급된 시간(Issued At)</li>
  <li>jti : JWT ID로 토큰에 대한 식별자이다.</li>
</ul>

<h3>JWT 사용 시 주의할 점</h3>
<ul>
  <li>Claim Set은 암호화 하지 않는다. 따라서 서명 없이도 누구나 확인이 가능하기에 보안이 중요한 데이터는 넣으면 안된다. Base64로 인코딩해서 사용하다 보면 이 부분을 간과하기 쉽기 때문에 필요한 최소한의 정보만 Claim Set에 담아야한다.</li>
  <li>토큰을 강제로 만료시킬 방법이 없다. 서버가 토큰의 상태를 가지고 있지 않고 토큰 발급 시 해당 토큰이 유효한 조건이 결정되므로 클라이언트가 로그아웃하더라도 해당 토큰을 클라이언트가 제거하는 것 뿐이지 토큰 자체가 만료되는 것은 아니다. 만약 이때 누군가 토큰을 탈취한다면 해당 토큰을 만료시간까지는 유효하게 사용할 수 있다. 무상태를 유지하면서 이 부분을 같이 해결할 방법은 존재하지 않으므로 서비스에서 이 부분이 문제가 된다면 선택을 해야한다.</li>
</ul>

## 레퍼런스
https://soye0n.tistory.com/3<br>
https://lazyhoneyant.tistory.com/7<br>
https://blog.outsider.ne.kr/1160
