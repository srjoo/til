# Git
- 깃은 2005년에 리누스 토르발스에 의해 개발된 '분산 버전 관리 시스템(Distributed Version Control System)'으로 컴퓨터 파일의 변경사항을 추적하고 여러명의 사용자들 간에 파일에 대한 작업을 조율하는데 사용된다

# 장점
- 분산 버전관리이기 때문에 인터넷 연결 없이도 개발 가능하며, 중앙 저장소가 삭제되어도 원상 복구 가능
- 각각의 개발자가 Branch에서 개발한 후 Merge를 통해 병렬 개발 가능하며

# SVN(Subversion SVN)과 비교
- 중앙서버 vs 로컬 저장소 저장 후 서버 업로드
- 동시 업로드시 충돌 가능성 vs Branch와 Merge를 통해 충돌 가능성 낮음
- 네트워크 필요 vs 업로드시만 네트워크
- 히스토리 관리 어려움 vs 히스토리 관리 쉬움

# 용어
- Repository : 저장소
- Working Tree : 작업자의 현재 시점
- Staging Area : 저장소에 커밋하기 전에 커밋을 준비하는 위치
- Commit : 변경된 작업 상태를 확정하고 저장소에 저장소
- Head : 현재 작업중인 Branch
- Merge : 다른 Branch를 현재 Branch로 가져와 합치는 작업