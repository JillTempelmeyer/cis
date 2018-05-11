---
copyright:
  years: 2018
lastupdated: "2018-03-13"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 최적의 안정성을 위한 IBM CIS 배치 관리

IBM CIS 배치에 대한 최적의 안정성을 얻기 위해 유용한 DNS 구성을 설정하고 글로벌 로드 밸런서를 설정할 수 있습니다. 추가적인 안정성을 위해, 페이지 규칙을 사용하여 오리진 서버나 캐시에 문제점이 있어도 웹 컨텐츠가 고객에게 전달되도록 보장할 수 있습니다. 이 문서는 IBM CIS 배치를 최적으로 안정화하기 위한 몇 가지 우수 사례를 자세히 설명합니다. 

일반적으로는 다음의 우수 사례를 권장합니다. 

 * IBM CIS 프록시 서버 및 기타 기능을 활용하도록 DNS 설정
 * 하나 이상의 글로벌 로드 밸런서를 사용하여 사이트 트래픽을 균등하게 분배
 * 캐싱 및 기타 옵션을 관리하기 위한 적합한 페이지 규칙 설정

각각의 이러한 항목은 보다 안정적인 CIS 배치를 작성하는 데 사용할 수 있는 특정 기능을 제공합니다. 

참고로, CIS 인터페이스는 *보안*, *안정성* 및 *성능* 섹션으로 구성되어 있습니다. 기본 탐색 메뉴는 다음 그림에 표시되어 있으며, 글로벌 로드 밴런서와 DNS 메뉴 항목이 드러나 있습니다. 

![왼쪽 탐색 DNS ](images/cis-left-navigation.png)


## DNS 설정
 
 DNS 구성 설정을 시작하려면 이전에 표시된 대로 탐색 메뉴에서 **DNS**를 선택하십시오. 
 
 안정성을 위한 DNS 설정과 관리에 대한 세부 정보는 [이 문서를 참조](dns.html#setting-up-your-domain-name-system-dns-for-ibm-cis)하십시오. 


## 글로벌 로드 밸런서 설정


글로벌 로드 밸랜서 설정을 시작하려면 탐색 메뉴에서 **글로벌 로드 밸랜서**를 선택하십시오. 

글로벌 로드 밸런서 설정과 관리에 대한 세부 정보는 [이 문서를 참조](glb.html#global-load-balancer-glb-concepts)하십시오. 

## 페이지 규칙을 사용하여 안정성 증가

사이트 최대 안정성을 제공하는 일부 권장되는 페이지 규칙 설정은 다음과 같습니다. 

 * 항상 온라인
 * 오리진 캐시 제어
 * URL 전달

 ## 항상 온라인

**항상 온라인** 페이지 규칙 설정을 사용하여 서버가 작동 중지될 때 제한된 사이트 버전을 온라인 상태로 유지할 수 있습니다. 

**항상 온라인**에서는 서버가 작동 중지될 때 IBM CIS가 캐시에서 페이지를 서비스하므로 방문자는 자신이 방문을 시도하는 일부 페이지를 계속해서 봅니다. 방문자는 자신이 오프라인 브라우징 모드에 있음을 알려주는 페이지 맨 위의 메시지를 봅니다. "항상 온라인"은 HTTP 상태 503을 리턴하지만 503은 기타 많은 웹 애플리케이션에 의해서도 사용됩니다. 서버가 다시 온라인 상태로 돌아오면 IBM CIS는 일반 브라우징으로 유연하게 사용자를 다시 범핑합니다. 

IBM CIS의 캐시에 요청된 페이지가 없는 경우, 방문자는 요청 중인 웹 사이트 페이지가 오프라인 상태임을 알려주는 오류 페이지를 봅니다. 

### "항상 온라인" 설정 방법

**항상 온라인**을 사용하려면 다음 단계를 따르십시오. 

 * 탐색 메뉴를 사용하여 성능 아래의 페이지 규칙을 선택합니다. 
 * 사용자 도메인의 URL 패턴으로 페이지 규칙을 작성합니다. 
 * 토글을 켜서 **항상 온라인** 설정을 추가합니다. 
 * 리소스 프로비저닝을 선택합니다. 

 ### "항상 온라인"의 제한사항

 * **항상 온라인**은 루트 HTML에서 최초 10개의 링크를 캐시한 후에 각각의 해당 페이지에서 단지 최초 링크만 캐시합니다. 그리고 마지막으로 각각의 해당 후속 페이지에서 최초 링크를 캐시합니다. 이는 오리진 서버가 작동 중지될 때 사이트의 일부 페이지만 볼 수 있음을 의미합니다. 

 * 최근에 추가된 사이트는 해당 사이트의 대형 캐시를 사용할 수 없으며, 이는 단지 몇일 전에 해당 사이트를 추가한 경우에 **항상 온라인**이 작동하지 않을 수 있음을 의미합니다. 

 * 서버가 작동 중지된 경우에 CIS는 개인용 컨텐츠를 표시하거나 양식 제출(POST)을 처리할 수 없습니다. 방문자에게는 보기 위해 로그인을 요구하는 항목 또는 체크아웃 페이지의 오류가 표시됩니다. 

 * **항상 온라인**을 트리거하려면 웹 서버가 502 또는 504 제한시간 초과의 표준 HTTP 오류 코드를 리턴해야 합니다. "항상 온라인"은 오리진 접속 문제(오류 521 & 523), 제한시간 초과(522 & 524), SSL 오류(525 & 526) 또는 알 수 없는 오류(520)가 발생할 때도 작동됩니다. **항상 온라인**은 기타 HTTP 응답 코드에 대해서는 트리거되지 않습니다(예: 404s, 500, 503, 데이터베이스 연결 오류, 내부 서버 오류 또는 서버의 빈 응답). 

 * "에지 캐시 만기 TTL"로 인해 **항상 온라인** 캐시가 해당 간격에 영구 제거되므로, 캐싱 빈도(무료 고객: 7일, 프로 고객: 3일, 비즈니스 및 엔터프라이즈 고객: 1일) 미만인 "에지 캐시 만기 TTL"로 "모두 캐시" 페이지 규칙이 사용되는 경우에는 **항상 온라인**이 작동되지 않습니다. 

## 오리진 캐시 제어

**오리진 캐시 제어** 페이지 규칙 설정을 사용하여 오리진에서 캐시되는 컨텐츠와 해당 컨텐츠의 업데이트 빈도를 판별할 수 있습니다(이는 안정성과 성능에 영향을 줌). 기본적으로, 설정이 변경되지 않았으며 캐싱을 방지하는 헤더가 오리진 서버에서 전송되지 않은 경우에는 IBM CIS가 특정 확장자의 모든 정적 컨텐츠를 캐시합니다. 이러한 유형의 컨텐츠에는 이미지, CSS 및 Javascript가 포함됩니다. 이 캐싱은 주로 성능상의 이유로 수행됩니다. 

**오리진 캐시 제어**를 설정하려면 컨텐츠의 각 리소스와 관련하여 원하는 작동을 제공하는 특정 헤더를 켜는 페이지 규칙을 사용합니다. **오리진 캐시 제어**를 사용하는 방법을 이해하려면 CIS의 전체 캐싱 작동 및 페이지 규칙의 일부 추가적인 일반 설명이 컨텍스트를 제공하기 위해 필요하며, 다음의 여러 섹션에서 이를 볼 수 있습니다. 일반적으로 캐싱을 제어하기 위해 사용할 수 있는 세 가지 방법이 존재하며 **오리진 캐시 제어**는 두 번째 방법입니다. 

**오리진 캐시 제어** 설정은 인터넷 우수 사례 및 RFC에 가장 가까이 접근하고자 하는 캐싱 규칙을 호출합니다(주로 재인증과 관련하여). 예를 들어, `max-age=0`의 CIS 기본 작동은 전혀 캐시하지 않는 반면 **오리진 캐시 제어** 캐시 설정은 캐시합니다. 그러나 항상 재검증하지는 않습니다. 

### "오리진 캐시 제어" 설정 방법

 * 탐색 메뉴를 사용하여 성능 아래의 페이지 규칙을 선택합니다. 
 * 사용자 도메인을 참조하는 URL 패턴으로 페이지 규칙을 작성합니다. 
 * 토글을 켜서 **오리진 캐시 제어** 설정을 추가합니다. 
 * 리소스 프로비저닝을 선택합니다. 

### 페이지 규칙 우선순위

두 개의 특정 페이지 규칙이 전체적으로 캐싱에 우선합니다. 

 * 페이지 규칙의 **캐시 레벨**이 `Bypass`로 설정된 경우에는 해당 페이지 규칙과 일치하는 리소스가 캐시되지 않습니다. IBM CIS는 여전히 프록시로서의 역할을 하며 기타 성능 특성은 활성 상태를 유지합니다. 그러나 컨텐츠가 캐시에서 서비스되지 않으며 이는 오리진 서버에서 직접 페치됩니다. 

 * 페이지 규칙의 **캐시 레벨**이 `Cache everything`으로 설정된 경우에는 해당 페이지 규칙과 일치하는 리소스가 캐시됩니다. **이 페이지 규칙 설정의 사용은 HTML을 포함하여 정적으로 간주되는 것을 넘어서는 리소스를 캐시함을 사용자에게 알려주는 유일한 방법입니다. **

페이지 규칙이 설정되지 않은 경우에는 리소스의 확장자를 기반으로 하는 `Standard` 캐싱 모드를 사용합니다. 정적 리소스만 캐시됩니다(이전에 언급된 대로). 

### 오리진 Cache-Control 헤더

IBM CIS가 캐시하는 내용을 변경하는 두 번째 방법은 오리진에서 전송된 캐시 헤더를 통해서입니다. CIS는 이러한 설정을 준수하지만, 사용자는 **에지 캐시 TTL** 페이지 규칙 설정을 지정하여 이를 대체할 수 있습니다. 오리진에서 캐시할 리소스를 결정할 때 고려할 헤더는 다음과 같습니다. 

 * **Cache-Control** 헤더가 `private`, `no-store`, `no-cache` 또는 `max-age=0`으로 설정되거나 응답에 쿠키가 있는 경우, IBM CIS는 리소스를 캐시하지 않습니다. 참고로, 민감한 자료는 캐시되지 않아야 하므로 해당 경우에는 이러한 헤더 중 하나의 사용을 고려할 수 있습니다. 

 * **Cache-Control** 헤더가 `public`으로 설정되어 있으며 `max-age`가 0보다 큰 경우 또는 `Expires` 헤더가 향후 임의의 시점에 설정된 경우에는 리소스가 캐시됩니다. 

**참고:** RFC 규칙에 따라 `Cache-Control: max-age`는 `Expires` 헤더에 우선합니다. 둘 모두가 표시되고 서로 충돌하면 `max-age`가 우세합니다. 

### 's-maxage' 헤더 사용

캐싱 작동 및 브라우저 캐싱 작동을 함께 제어하는 세 번째 방법은 `s-maxage` Cache-Control 헤더를 사용하는 것입니다. 

일반적으로는 `max-age` 지시문을 준수합니다. 

`Cache-Control: max-age=1000`

그러나 사용자가 브라우저와 다른 캐시 제한시간을 지정하고자 하는 경우에는 `s-maxage`가 사용될 수 있습니다. 200초 동안 오브젝트를 캐시하도록 IBM CIS에 알리고 60초 동안 오브젝트를 캐시하도록 브라우저에 알리는 예제는 다음과 같습니다. 

`Cache-Control: s-maxage=200, max-age=60`

기본적으로 `s-maxage` 뒤에는 리버스 프록시만 나와야 합니다(브라우저가 이를 무시할 수 있도록). 그러나 이와는 반대로 당사(IBM CIS)는 `s-maxage`(존재하는 경우)에 우선순위를 부여합니다. 브라우저 캐시 설정 또는 `max-age` 헤더 중에서 더 높은 값을 준수합니다. 

### 안정성을 위한 페이지 규칙 및 캐시 제어 헤더의 요약

요약하자면 캐싱과 관련하여 안정성을 위해 고려할 일부 기본 영역은 다음과 같습니다. 

 * 오리진의 캐싱 헤더를 확인하여 캐시 가능한 리소스에 대한 대체 헤더가 없는지 확인하십시오(`Cache-Control` 및 `Expires`). 

 * CIS는 기본적으로 리턴 코드에 따라 다음 TTL로 항상 정적 컨텐츠를 캐시합니다. 

```
200 301    120m;
302 303    20m;
403        5m; for reliability
404        5m;
any        0s;
```

 * 추가로 캐시하려면 원하는 URL에서 "모두 캐시"로 설정된 **캐시 레벨**로 페이지 규칙을 작성하십시오(이 URL을 요청할 때 웹 서버가 404를 리턴하면 5m에 대해서만 이 결과가 캐시됨). 

 * URL에서 캐싱을 피하려면 **캐시 레벨**을 "무시"로 설정하여 페이지 규칙을 작성하십시오. 


 ## URL 전달

컨텐츠가 항상 사용 가능함(HA)을 보장하려면 사이트가 사용 불가능할 때 사용되는 **URL 전달** 설정으로 페이지 규칙을 작성할 수 있습니다. 

 **참고:** **URL 전달**을 사용하는 경우에는 모든 트래픽을 다른 URL로 전송하므로 기타 모든 설정이 사용되지 않습니다. 

### URL 전달을 설정하는 방법

 * 탐색 메뉴를 사용하여 성능 아래의 페이지 규칙을 선택합니다. 
 * 사용자 도메인을 참조하는 URL 패턴으로 페이지 규칙을 작성합니다. 
 * **URL 전달** 설정을 추가합니다. 
 * 전달 유형을 선택하고 대상 URL을 입력합니다. 
 * 리소스 프로비저닝을 선택합니다. 

### 전달 예제:

누구나 다음의 URL에 쉽게 접근할 수 있도록 하고자 한다고 가정합니다. 

    *www.example.com/+

    *example.com/+

다음 패턴은 일치합니다. 

    http://example.com/+
    http://www.example.com/+
    https://www.example.com/+
    https://blog.example.com/+
    https://www.blog.example.com/+
    Etc...

다음은 일치하지 않습니다. 

    http://www.example.com/blog/+  [extra directory before the +]
    http://www.example.com+  [no trailing slash]


일단 원하는 내용과 일치하는 패턴이 작성되면 **URL 전달** 설정을 추가하고 전달 유형을 선택한 후에 대상 URL을 입력하십시오. 예를 들어 다음과 같습니다. 

    https://plus.google.com/yourid

리소스 프로비저닝을 선택합니다. 수 초 내에 패턴과 일치하는 요청은 지정된 경로 재지정으로 새 URL로 전달됩니다. 

### 고급 전달 옵션:

기본 경로 재지정을 사용하는 경우(예: 루트 도메인을 www.yourdomain.com으로 전달)에는 URL의 다른 부분이 유실됩니다. 예를 들어 패턴을 설정할 수 있습니다. 

    example.com

그리고 이를 다음으로 전달합니다. 

    http://www.example.com

그러나 누군가가 다음을 입력합니다. 

    example.com/some-particular-page.html

그러면 이는 다음으로 경로 재지정됩니다. 

    www.example.com

다음이 아닙니다. 

    www.example.com/some-particular-page.html

솔루션은 변수를 사용하는 것입니다. 각각의 와일드카드는 전달 주소에서 참조될 수 있는 변수에 대응됩니다. 변수는 `$`와 그 이후의 숫자로 표시됩니다. 첫 번째 와일드카드를 참조하려면 `$1`을 사용하고, 두 번째 와일드카드를 참조하려면 `$2`를 사용합니다. 나머지도 마찬가지입니다. 이전 예제에서 루트에서 `www`로의 전달을 지정하기 위해 동일한 패턴을 사용할 수 있습니다. 

    example.com/*

그리고 전달할 트래픽에 대해 다음 URL을 설정할 수 있습니다. 

    http://www.example.com/$1

이 경우에 누군가가 다음으로 이동합니다. 

    example.com/some-particular-page.html

이들은 다음으로 경로 재지정됩니다. 

    http://www.example.com/some-particular-page.html