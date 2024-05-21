# 임계구역(Critical Section)
- 서로 다른 두 프로세스, 혹은 스레드 등의 처리 단위가 같이 접근해서는 안 되는 공유 영역을 뜻한다
> 동기화 도구에는 대표적으로 뮤텍스(Mutex)와 세마포어(Semaphore)가 있다

# 입장구역(Entry Section)
- 임계구역 진입 전 진입 허가를 요청하는 부분

# 퇴장구역(Exit Section)
- 임계구역 종료 후 끝낫음을 알리는 부분

# 나머지 구역(Remainder Section)
- 그 외의 모든 코드

# 코드 예시
```c++
do {
      wait(mutex);   // 입장 구역
      // 임계 구역
      signal(mutex); // 퇴장 구역
      // 나머지 구역
}
```

# 참고

|내용|URL|
|:---|:---|
|임계 구역|https://ko.wikipedia.org/wiki/%EC%9E%84%EA%B3%84_%EA%B5%AC%EC%97%AD|