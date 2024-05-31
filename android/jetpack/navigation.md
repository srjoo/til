# Navigation
- Fragment와 Activity간 구현을 간단하고 안정적이게 이동할 수 있도록 해주는 컴포넌트

# 장점
- Fragment Transaction을 직접 하지 않아도 된다
- Back/Up 동작 기본 지원
- 화면 전환시 데이터 전달 가능
- 안드로이드의 res로 Animation/Transition 설정 가능
- 딥링크 지원
- Navigation Drawer, Bottom Navitation 연동 가능

# 기본 원칙
- 고정된 시작점(첫번째 화면)을 가져야 한다
- Navigation 상태는 Stack으로 표현되어야함
- Up 버튼으로 앱이 종료되면 안된다
- App Task 내에서 Up과 Back 버튼은 동일하게 동작해야함
- 딥링크와 탐색은 동일 Stack으로 처리

