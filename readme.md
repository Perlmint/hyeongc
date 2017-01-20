혀엉씨
===========

[난해한 혀엉...언어 v0.4.4](https://gist.github.com/xnuk/d9f883ede568d97caa158255e4b4d069/836a208b5d9fe729eb0d2fd9bf73aec7dc155ffb)의 C 구현체 혀엉씨 v0.4.4

모든 [해석 케이스](https://github.com/xnuk/hyeong-testcases) 구현 (example/parse-test 참조)

## 사용법
* hyeongc [option] [--] 파일명

#### 옵션
* -v, --version, -h, --help

    도움말 출력
* -d, --debug

    실행 시 디버그 메시지를 함께 출력
* --parse-only

    실행하지 않고, 파싱 결과만을 출력

#### 컴파일
gcc 와 make가 있는 Unix-like 시스템에서 컴파일 가능함

## 구현체 스펙
* 토큰 처리 O(n)
* 명령어 분석 최대 O(n^2) (대부분의 경우 O(n))
* 스택 찾기 O(1) (mod 10을 이용한 기초적인 해싱)
* 스택 개수 제한 없음 (허용 메모리 내)
* 하트 저장소 찾기 최대 O(n) (하트 모양 별 linked list)
* 하트 저장소 개수 제한 없음 (허용 메모리 내)
* 유리수 분모/분자 각각 부호 있는 최소한 64비트 이상의 정수 범위
* 디버그 모드
* 파서 결과만 출력 가능

## 미구현 언어 명세
* 2바이트 이상의 유니코드 출력

## 변경된, 또는 구체화된 언어 명세
* 하트 구역의 <code>?</code>와 <code>/</code>는 해당 토큰 전후에 하트 토큰이 없어도 정상 작동함
> 예시: <code>형...!</code>는 <code>3</code>를 현재 스택에 넣은 뒤, <code>!</code> 토큰으로 인해 이 값을 뽑는다. 선택할 하트 토큰이 없으므로 뽑은 값은 버려진다.

* 하트 구역에 하트 토큰이 여러 개 나타나는 경우 맨 처음 하트를 제외한 나머지 하트는 버려짐
> 예시: <code>혀엉...💗!💕</code> 와 <code>혀엉...💗💙💝!💕♡💜</code>는 동치이다.

* 공백 문자(스페이스, 탭, 캐리지, 뉴라인)는 명령어를 구분짓지 않는다.
> 예시: <code>혀엉... .</code>와 <code>혀엉....</code>은 완전히 동일한 명령어이다.

* <code>?</code>와 <code>!</code>에서 값을 비교할 때, 현재 스택에서 가져온 값이 유리수인 경우 자신보다 작은 정수 중 가장 큰 정수를 택하여 비교한다.
> 예시: 현재 스택 상태가 <code>-> (1/3) 5 7</code>인 경우, (1/3)을 뽑아 0으로 비교한다.  
> 예시: 현재 스택 상태가 <code>-></code>인 경우, (NaN)을 뽑아 비교하며, 항상 뒷부분 구역을 선택한다.

* 0번 스택에 넣어지는 값들은 무시된다.
