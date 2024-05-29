# Git Branch Strategy
- 개발자들이 협업을 진행할 때 동일한 소스코드를 함께 공유하고 다룬다. 여기서 어떤 사람은 버그를 수정하고, 어떤 사람을 기능을 개발하기도 한다. 동일한 코드를 여러 사람이 다른 작업을 할 때 서로 다른 버전의 코드가 만들어진다. 이 때 동시에 여러 작업을 할 수 있도록 Branch(브랜치)를 사용한다. 분리된 작업 영역에서 수정을 하고 나중에 원래 버전과 비교해서 하나의 새로운 버전을 만든다. 이러한 브랜치 전략은 몇가지가 있다

# Git Flow
- master : 기준이 되는 브랜치로 제품을 배포하는 브랜치
- develop : 개발 브랜치로 개발자들이 이 브랜치를 기준으로 각자 작업한 기능들을 Merge
- feature : 단위 기능을 개발하는 브랜치로 기능 개발이 완료되면 develop 브랜치에 Merge
- release : 배포를 위해 master 브랜치로 보내기 전에 먼저 QA(품질검사)를 하기위한 브랜치
- hotfix : master 브랜치로 배포를 했는데 버그가 생겼을 떄 긴급 수정하는 브랜치

> master와 develop가 중요한 브랜치이고 나머지는 필요에 의해서 운영하는 브랜치이다.
> branch를 merge할 때 항상 -no-ff 옵션을 붙여 branch에 대한 기록이 사라지는 것을 방지하는 것을 원칙으로 한다.

# GitHub Flow
- Git-Flow가 Github에서 사용하기에는 복잡하다고 나온 브랜치 전략이다.
- hotfix 브랜치나 feature 브랜치를 구분하지 않는다. 다만 우선순위가 다를 뿐
- 수시로 배포가 일어나며, CI와 배포가 자동화되어있는 프로젝트에 유용

1. master 브랜치는 어떤 때든 배포가 가능하다
> master 브랜치는 항상 최신 상태며, stable 상태로 product에 배포되는 브랜치
> 이 브랜치에 대해서는 엄격한 role과 함께 사용한다
> merge하기 전에 충분히 테스트를 해야한다. 테스트는 로컬에서 하는 것이 아니라 브랜치를 push 하고 Jenkins로 테스트 한다

2. master에서 새로운일을 시작하기 위해 브랜치를 만든다면, 이름을 명확히 작성하자
> 브랜치는 항상 master 브랜치에서 만든다
> Git-flow와는 다르게 feature 브랜치나 develop 브랜치가 존재하지 않음
> 새로운 기능을 추가하거나, 버그를 해결하기 위한 브랜치 이름은 자세하게 어떤 일을 하고 있는지에 대해서 작성해주도록 하자
> 커밋메시지를 명확하게 작성하자

3. 원격지 브랜치로 수시로 push 하자
> Git-flow와 상반되는 방식
> 항상 원격지에 자신이 하고 있는 일들을 올려 다른 사람들도 확인할 수 있도록 해준다
> 이는 하드웨어에 문제가 발생해 작업하던 부분이 없어지더라도, 원격지에 있는 소스를 받아서 작업할 수 있도록 해준다

4. 피드백이나 도움이 필요할 때, 그리고 merge 준비가 완료되었을 때는 pull request를 생성한다
> pull request는 코드 리뷰를 도와주는 시스템
> 이것을 이용해 자신의 코드를 공유하고, 리뷰받자
> merge 준비가 완료되었다면 master 브랜치로 반영을 요구하자

5. 기능에 대한 리뷰와 논의가 끝난 후 master로 merge한다
> product로 반영이 될 기능이므로, 이해관계가 연결된 사람들과 충분한 논의 이후 반영하도록 한다

6. master로 merge되고 push 되었을 때는, 즉시 배포되어야한다
> GitHub-flow의 핵심
> master로 merge가 일어나면 자동으로 배포가 되도록 설정해놓는다

# Gitlab Flow
- 복잡한 Git Flow와 너무 간단한 Github의 절충안
- production : master와 동일, 배포만을 담당한다
- pre-production : production과 합병하기 전에 테스트를 수행하는 역할
- production이 배포를 담당하고 있어, 배포 소스가 최신일 필요가 없다

1. master 브랜치에서 신규 브랜치를 추가
2. 신규 브랜치에서 소스를 commit 하고 push
3. merge request를 생성하여 master 브랜치로 merge 요청
4. master 권한 사용자는 developer 사용자와 함께 리뷰 진행 후 master 브랜치로 merge
5. 테스트가 필요하다면 master에서 procution 브랜치로 merge하기 전에 pre-production 브랜치에서 테스트

# 번외 : Fork와 Pull Request
- 큰 규모의 개발은 Fork와 Pull requests를 활용
- Fork는 브랜치와 비슷하지만 프로젝트를 통째로 외부로 복제하는 것
- 개발 후 Merge를 바로 하는 것이 아니라 Pull requests로 원 프로젝트 관리자에서 머지 요청을 보내면 원 프로젝트 관리자가 Pull requests된 코드를 보고 적절하다 싶으면 그때 그 기능을 붙히는 식으로 개발을 진행한다.