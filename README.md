HTTP 헤더는 크게 응답 헤더(Response Headers), 요청 헤더(Request Headers),개체 헤더(Entity Header) 그리고 공통 헤더(General Headers)로 나눌 수 있다. 각 카테고리는 서로 다른 목적과 역할을 가지고 있으며, HTTP 통신 과정에서 중요한 정보를 전달하는 데 사용된다.

# 1.공통헤더
공통 헤더는 요청과 응답 모두에 사용될 수 있는 헤더로, 메시지 전체에 적용되는 정보를 담는다.


## [cache-control]
HTTP 캐시 제어(Cache-Control) 헤더는 웹 브라우저와 서버 간의 HTTP 통신에서 캐싱 메커니즘을 제어하기 위해 사용된다. 이 헤더는 자원의 캐싱 가능 여부, 캐시되는 기간, 캐시의 재검증 조건 등을 지정할 수 있다. 
적절한 캐시 전략을 사용함으로써 서버 부하를 줄이고, 사용자 경험을 향상시킬 수 있다. 각 옵션의 사용은 애플리케이션의 요구 사항과 콘텐츠의 특성에 따라 달라질 수 있으므로, 옵션들을 잘 이해하고 적절히 적용하는 것이 중요하다.


### 일반 지시어
* no-cache: 캐시된 복사본을 사용하기 전에 항상 웹서버에 재검증을 요청한다. 캐시된 데이터를 사용할 수는 있지만, 항상 최신 상태인지 서버에 확인해야 한다.
  * 재검증 과정은 주로 ETag 헤더와 Last-Modified 헤더를 이용해 HTML, CSS, JS 파일등이 캐시된 자원이 최신 상태인지 확인한다.(해당 헤더는 아래에서 설명)
  * 해당 옵션이 설정된 웹사이트는 배포의경우 파일이 변경되므로 서버로부터 자원을 받지만, 일반적인 상황에서는 브라우저에 캐시된 자원을 이용한다.
* no-store: 어떤 응답도 캐시에 저장을 하지 않는다. 즉, 매번 서버로부터 새로운 데이터를 받아와야 한다.
  * 해당옵션이 설정된 웹사이트는 배포뿐만 아니라 웹사이트를 방문할때마다 웹서버로부터 자원을 받아온다.
* public: 어떤 캐시(브라우저 캐시, 프록시 캐시 등)에서도 응답을 저장할 수 있음을 나타낸다.
  * 예를 들어, 공용 Wi-Fi 환경에서 여러 사용자가 같은 웹사이트에 접근할 때, public 캐시 옵션이 설정된 자원은 프록시 서버나 중간 캐시에 저장되어 이후 동일한 요청에 대해 빠르게 제공될 수 있다.
* private: 응답이 특정 사용자에게만 특화되었다는 것을 나타내며, 사용자의 브라우저에만 캐시될 수 있다.
* max-age=<seconds>: 자원이 최대 얼마나 오래(초 단위) 캐시될 수 있는지 지정
* s-maxage=<seconds>: 공유 캐시(예: 프록시 서버)에서의 최대 유효 기간을 지정(max-age보다 우선)

### 재검증 및 재로딩 지시어
* must-revalidate: 캐시된 자원이 만료된 경우, 사용되기 전에 반드시 서버로부터 재검증을 받아야 한다.
* proxy-revalidate: 공유 캐시에 대해 must-revalidate와 같은 역할을 한다.
* immutable: 응답이 절대 변하지 않음을 나타내어, 브라우저가 캐시된 자원을 재검증 없이 재사용하도록 한다.

### 기타 지시어
* no-transform: 중간 캐시(프록시 등)가 응답의 본문, Content-Type 헤더 등을 변경하는 것을 금지한다.
* only-if-cached: 네트워크를 사용하지 않고, 캐시에 있는 경우에만 자원을 가져오도록 요청한다.


## [Connection]
Connection 헤더는 현재의 네트워크 연결에 대한 옵션을 제어한다. 이 헤더는 특히 HTTP/1.1에서 중요한 역할을 하며, 클라이언트와 서버 간의 연결 관리에 사용된다. Connection 헤더의 주요 사용 예시는 다음과 같습니다:

* Connection: keep-alive: 이 옵션은 네트워크 연결을 지속적으로 유지하고, 여러 HTTP 요청/응답이 동일한 연결을 재사용할 수 있도록 합니다. 이는 연결 설정에 대한 오버헤드를 줄이고, 웹 페이지의 로딩 성능을 향상시키다. 클라이언트와 서버가 지속적인 연결을 지원하고 유지하기를 원할 때 사용된다.
* Connection: close: 이 옵션은 현재의 연결을 사용한 후에 닫아야 함을 나타낸다. 즉, 현재의 요청/응답 처리가 완료되면 연결이 종료된다. 이는 주로 HTTP/1.0과 같이 기본적으로 지속 연결을 지원하지 않는 환경에서 사용된다.


## [Date]
Date 헤더는 HTTP 메시지가 생성된 정확한 날짜와 시간을 나타냅니다.(ex.Date: Thu, 15 Feb 2024 00:22:02 GMT)


# 2.엔티티 헤더
엔티티 헤더(Entity Headers)는 HTTP 메시지의 본문(body)을 기술하는 데 사용되며, 메시지의 콘텐츠에 대한 정보(예: 콘텐츠 길이, 타입 등)와 메시지 자체에 대한 정보(예: 수정된 날짜)를 포함한다.엔티티 헤더는 요청,응답헤더 둘다에서 사용된다.

## [Content-Type]
해당 헤더는 HTTP 메시지 본문의 미디어 타입을 나타낸다. 이는 브라우저나 클라이언트가 어떻게 콘텐츠를 해석해야 하는지를 알려주는데 예를 들어, Content-Type: text/html은 본문이 HTML 문서임을, Content-Type: application/json은 JSON 형식의 데이터임을 나타낸다.

## [Content-Length]
해당 헤더는 메시지 본문의 크기를 바이트 단위로 나타낸다. 이는 클라이언트가 서버로부터 얼마나 많은 데이터를 받을지 예상할 수 있게 해주며, 메시지의 끝을 결정하는 데 도움을 준다.

## [Content-Encoding]
해당 헤더는 메시지 본문이 어떠한 인코딩 알고리즘(예: gzip, deflate)을 사용하여 압축되었는지를 나타낸다. 이는 클라이언트가 콘텐츠를 올바르게 디코딩하기 위해 필요하다.

## [Content-Language]
해당 헤더는 응답 본문이 어떤 언어로 작성되었는지를 나타낸다. 이는 다국어 지원 웹 사이트에서 유용하게 사용된다.

## [Content-Disposition]
해당 헤더는 웹 서버가 브라우저에게 응답 본문을 어떻게 표시해야 하는지 알려즌다.(예를 들어, Content-Disposition: attachment; filename="filename.jpg"는 브라우저에게 해당 파일을 다운로드하도록 지시)

## [ETag]
ETag 헤더는 메시지 본문의 내용이 변경될 때마다 갱신되는 고유한 식별자이다. 이는 클라이언트가 캐시된 복사본의 유효성을 검증하는 데 사용된다.(위에서 설명한 no-cache 옵션의 경우 해당 헤더를 이용해 파일이 변경되었는지 파악한다)
  
## [Last-Modified]
Last-Modified 헤더는 자원이 마지막으로 변경된 날짜와 시간을 나타냅니다. 이는 If-Modified-Since 요청 헤더와 함께 사용되어 캐시된 복사본의 최신성을 검증하는 데 사용될 수 있다.(위에서 설명한 no-cache 옵션의 경우 해당 헤더를 이용해 파일이 변경되었는지 파악한다)

<br/>

요청,응답헤더에서 자주 사용하는 헤더는 각각 조금씩 다른데, 예를들면 다음과 같다. 


#### 요청에서의 엔티티 헤더 사용 예시:
* Content-Type: 클라이언트가 서버로 보내는 요청 본문의 미디어 타입을 명시한다.(예를 들어, 클라이언트가 JSON 형식의 데이터를 서버에 전송할 때 Content-Type: application/json을 사용할 수 있다.)
* Content-Length: 요청 본문의 길이를 바이트 단위로 지정한다. 이는 서버가 요청 본문의 끝을 정확히 알 수 있게 해 준다.

#### 응답에서의 엔티티 헤더 사용 예시:
* Content-Type: 서버가 클라이언트에게 보내는 응답 본문의 미디어 타입을 명시한다.(예를 들어, HTML 문서를 반환할 때 Content-Type: text/html을 사용할 수 있다.)
* Content-Encoding: 서버가 클라이언트에게 보내는 응답 본문이 어떤 인코딩 알고리즘(예: gzip)으로 압축되었는지 나타낸다.(이를 통해 클라이언트는 압축 해제 과정을 올바르게 처리할 수 있다.)
* ETag 및 Last-Modified: 이 헤더들은 서버가 응답에 포함시켜, 클라이언트가 캐싱과 관련된 결정을 내릴 수 있도록 한다. 이러한 헤더는 주로 응답에 사용되지만, 요청에서 조건부 요청을 할 때 관련 요청 헤더(If-None-Match, If-Modified-Since)와 함께 사용된다


# 3.요청헤더
클라이언트가 서버에 정보를 요청할 때 사용되며, 요청의 성격, 클라이언트의 정보, 서버에 대한 지시사항 등을 포함한다.

## [Host]
* 목적: 요청이 전송되는 서버의 도메인 이름과 포트 번호를 지정한다. HTTP/1.1 요청에서는 이 헤더가 필수이다.
* 예시: Host: www.example.com

## [Origin]
* 목적: 크로스 오리진 요청에서 요청이 시작된 출발점(도메인/스킴/포트)을 서버에 알린다. 주로 CORS(Cross-Origin Resource Sharing) 처리에 사용된다.
* 예시: Origin: http://www.example.com

## [If-None-Match]
* 목적: 서버의 자원이 클라이언트가 가진 ETag 값과 일치하지 않을 때만 자원을 요청합니다. 캐시된 복사본의 유효성을 검사하는 데 사용된다.
* 예시: If-None-Match: "e02aa9c4d6f4b60f847bc8b6d3429c9d"

## [If-Range]
* 목적: Range 요청이 이루어질 때, 서버의 자원이 변경되지 않았을 경우에만 부분적인 내용을 요청하는 데 사용된다. 자원이 변경된 경우, 전체 자원을 다시 요청하게 된다.
* 예시: If-Range: "e02aa9c4d6f4b60f847bc8b6d3429c9d"

## [Upgrade-Insecure-Requests]
* 목적: 클라이언트가 서버에게 안전하지 않은 연결(예: HTTP)을 안전한 연결(예: HTTPS)로 업그레이드 할 것을 요청
* 예시: Upgrade-Insecure-Requests: 1


# 4.응답헤더
응답 헤더(Response Headers)는 서버가 클라이언트의 요청에 대해 응답을 보낼 때 함께 전송되는 정보이다. 이 헤더들은 클라이언트에게 유용한 정보를 제공하며, 클라이언트의 다음 행동을 지시하는 역할을 한다.

## [WWW-Authenticate]
* 목적: 클라이언트가 요청한 자원에 대한 접근이 인증을 필요로 할 때, 이 헤더를 통해 사용 가능한 인증 방법을 클라이언트에 알린다. 주로 401 Unauthorized 응답과 함께 사용된다.
* 예시: WWW-Authenticate: Basic realm="example"

## [Access-Control-Allow-Origin]
* 목적: 서버가 어떤 출처에서의 자원 요청을 허용할지를 지정한다. 이 헤더는 CORS 정책을 구현할 때 핵심적인 역할을 한다.
* 예시: Access-Control-Allow-Origin: * 또는 Access-Control-Allow-Origin: http://example.com

## [Access-Control-Allow-Methods]
* 목적: 서버가 클라이언트의 사전 요청(Preflight Request)에 대해 허용하는 HTTP 메소드를 지정한다. 이는 CORS 정책의 일부로, 어떤 HTTP 메소드가 실제 요청에서 사용될 수 있는지를 나타낸다.
* 예시: Access-Control-Allow-Methods: GET, POST, PUT

## [Access-Control-Allow-Headers]
* 목적: CORS 요청에서 클라이언트가 사용할 수 있는 HTTP 헤더를 지정한다. 서버는 이 헤더를 통해 사전 요청에 대해 허용하는 요청 헤더를 클라이언트에 알린다.(해당 헤더는 실제 요청에서 사용가능)
* 예시: Access-Control-Allow-Headers: X-Custom-Header, Content-Type

## [Access-Control-Allow-Credentials]
* 목적: 크로스 오리진 요청에서 클라이언트가 인증 정보(예: 쿠키, HTTP 인증)와 함께 요청을 보낼 수 있는지 여부를 나타낸다. 이 헤더가 true로 설정되면, 응답을 요청한 페이지의 도메인이 Access-Control-Allow-Origin 헤더에 명시된 도메인과 정확히 일치해야 한다.
* 예시: Access-Control-Allow-Credentials: true

## [Access-Control-Max-Age]
* 목적: 사전 요청의 결과를 캐시할 수 있는 최대 시간(초 단위)을 나타낸다. 이는 클라이언트가 동일한 사전 요청을 반복해서 보내지 않도록 하여, CORS 요청의 효율을 높인다.
* 예시: Access-Control-Max-Age: 86400

## [Strict-Transport-Security]
* 목적: 이 헤더를 사용하여 웹 사이트가 HTTPS를 통해서만 접근되어야 함을 브라우저에 알린다.
* 예시: Strict-Transport-Security: max-age=31536000; includeSubDomains

## [Access-Control-Expose-Headers]
* 클라이언트에서 접근할 수 있도록 서버가 허용하는 헤더를 나타낸다.
* 예시: Access-Control-Expose-Headers: X-My-Custom-Header, X-Another-Custom-Header
