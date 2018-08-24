---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 연결 구성
{: #connection-configuration}

{{site.data.keyword.composeForElasticsearch_full}} 데이터베이스 연결은 2개의 HAProxy 포털에서 관리합니다. 각 포털에는 64MB의 메모리가 있습니다.

두 개의 포털이 있으므로 포털 중 하나에 연결할 수 없게 되는 경우에도 애플리케이션에서 계속 연결할 수 있습니다. 클라이언트 측의 장애 복구는 애플리케이션 디자이너의 책임입니다. 일부 Elasticsearch 드라이버에서는 여러 연결 문자열과 단계별 장애 복구를 자동으로 처리합니다.

장애 복구 오류를 완전히 처리하는 드라이버로 작업하지 않는 경우 다음 단계를 수행하여 오류를 직접 처리하십시오.

* 해당 연결 구성에서 다중 엔드포인트를 허용하는 드라이버로 작업합니다.
* 데이터베이스에서 읽거나 쓸 때 애플리케이션에서 오류에 적절하게 대응하는지 확인하십시오. 다음과 같은 여러 다른 방법으로 오류에 대응하도록 애플리케이션을 구성할 수 있습니다.
  + 오류를 로그하고 다시 연결하십시오.
  + 다시 연결하고 오류의 원인이 된 오퍼레이션을 재시도하십시오.
  + 실패한 오퍼레이션을 큐에 넣고 다시 연결하도록 스케줄링하십시오.

## 전송 중 암호화

모든 {{site.data.keyword.composeForElasticsearch}} HAProxy 포털에서는 TLS/SSL이 사용되며 TLS 버전 1.2를 지원합니다. 서비스 인증서는 Let's Encrypt 인증서입니다.

## 연결 한계

두 HAProxy 포털의 포털당 연결 한계는 2000개입니다. 연결 한계를 초과하면 서비스가 응답하지 않게 될 수 있습니다. 드라이버의 연결 풀링 기능을 통해 애플리케이션에서의 연결을 관리할 수 있습니다.

## 프록시 제한시간

HAProxy 포털에서 서버와 클라이언트 간 연결의 라이프사이클을 관리합니다. 제한시간 값은 드라이버 작성자 및 {{site.data.keyword.composeForElasticsearch}} 데이터베이스 연결의 하위 레벨 활동에 관심이 있는 사용자용 참조 안내서로 여기에 자세히 설명되어 있습니다.

설정 |값 | 적용
----------|-----------|-----------
client | 1m | 클라이언트에서 데이터를 수신확인하거나 전송할 것으로 예상되는 경우.
connect | 9s | 프록시와 서버 간에 연결 중인 경우.
server | 2m | 서버에서 데이터를 수신확인하거나 전송할 것으로 예상되는 경우.
check | 5s | 프록시와 서버를 서로 연결 중일 때 2차 제한시간 확인으로 사용하는 경우.
http-request | 5s | 완료된 HTTP 요청을 기다릴 수 있는 최대 시간.
{: caption="표 1. Elasticsearch HAProxy 제한시간" caption-side="top"}
