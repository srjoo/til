# 캐시 교체 정책(Cache Replacement Policies)
- 싱은 일반 메모리 저장소보다 액세스 속도가 더 빠르거나 계산 비용이 저렴한 메모리 위치에 최근 또는 자주 사용되는 데이터 항목을 보관하여 성능을 향상시킵니다. 캐시가 가득 차면 알고리즘은 새 데이터를 위한 공간을 확보하기 위해 삭제할 항목을 선택하는 방법

# 무작위 교체(RR)
- 무작위로 선택하여 필요한 경우 폐기한다

# 대기열 기반 정책
- 선입선출(FIFO) - 가장 오래된 캐시를 먼저 제거함
- 후입선출(LIFO) - 가장 최근에 추가된 캐시를 먼저 제거함 

# 단순 최근성 기반 정책
- LRU(Least recently used) - 최근에 사용하지 항목을 먼저 삭제한다
- TLRU(Time-aware, Least-Recently-Used) - LRU 변형, 사용 시간을 추가 계산한다
- MRU(Most-recently-used) - 가장 최근에 사용한 항목을 삭제한다

# 단순 사용 빈도 기반 정책
- LFU(Least frequently used) - 가장 덜 사용된 항목을 먼저 삭제한다
- LFRU(Least frequent recently used) - LRU + LFU

# 참고
|내용|URL|
|:---|:---|
|Cache replacement policies|https://en.wikipedia.org/wiki/Cache_replacement_policies|

