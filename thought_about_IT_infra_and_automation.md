# IT인프라와 자동화에 대한 짧은 생각

약 15년 동안 리눅스, 유닉스 서버 관리 및 아키텍처 설계 업무를 해오고 있다. 2000년에 처음으로 리눅스로 서버를 관리할 때는 관리 대수가 많치는 않았지만, 두 명의 관리자가 샌스토리지, 라우터 및 스위치, 그리고 OS, WAS,  DB 설정까지 만능으로 구성해야 했다. 이렇게 많은 분야를 다루는만큼 전체 IT 구성요소를 어느 정도 자세하게 알 수 있는 환경은 되었지만 모든 설정을 외우고 있을 수는 없기 때문에 대부분의 설정 파일들을 텍스트파일로 저장해 놓고 필요할 때마다 복사하고 붙여넣는 방식으로 처리했었다. 또한 서버의 변경 작업이 별로 없었고, 필요한 작업은 스크립트를 만들어 놓았기 때문에 업무에 부담은 없었고, 서버의 uptime이 높을수록 관리를 잘하는 것이라는 인식을 가지게 되었다.

하지만 모든 산업에서 IT 서비스의 의존도가 점점 높아지고, 가상화가 도입되면서 상황이 급격하게 변화하게 되었다. 가상화를 처음 도입했을 때는 추운 서버실에서 서버가 부팅되길 기다리면서 벌벌 떨 필요도 없어지고, 네트워크 케이블 작업도 훨씬 줄어들게 되어 IDC를 방문할 일이 많이 줄어 너무 좋았다. 그리고 서버를 구축할 때도 가상머신의 템플릿을 찍어내기만 하면 되기 때문에 OS 설치시간과 어플리케이션 설치 및 설정 시간도 훨씬 단축되었다. 그런데 이렇게 쉽게 OS를 만들 수 있게 되니 관리해야할 서버대수가 기하급수적으로 늘어나게 되는 현상이 발생하였다. 처음에는 하나의 서버에 하나의 어플리케이션만 돌아가도록 분류하고 그룹화시킬 수 있어서 관리가 편해졌다고 느꼈었지만, 나중에는 너무 많은 서버들이 생겨서 표준화 적용도 힘들고 관리도 어려워지게 되었다. 패치 하나 적용하려면 수 십대를 각각 접속해서 동일한 작업을 해야하고 어떤 서버에 적용했는지 안했는지 파악하는 것도 힘들게 되었다. 그래서 결국은 파이썬으로 서버 정보를 관리하는 프로그램을 만들수 밖에 없었다.

초기 서버관리자들은 프로그래밍과 서버운영을 어느 정도 같이 하는 사람들이 많았으나, IT의 업군이 분업화되면서 스토리지 관리자는 스토리지만, 네트워크 관리자는 네트워크만, 그리고 서버 관리자는 OS 및 DNS, FTP와 같은 시스템 어플리케이션만 관리하도록 나눠지고 개발은 모두 전문 개발자가 하는 영역으로 분리시키는 경향이 생겼다. 그러면서 시스템 관리자는 점차 프로그래밍을 배우려 하지 않게 되고, 굳이 자신이 개발하지 않더라도 크게 불편하지 않게 되었다. 하지만 이제 “소프트웨어 정의” 인프라가 대두되고 시스템의 클러스터링과 API를 통한 시스템 간의 연계가 중시되면서 프로그래밍도 시스템 관리자가 가져야할 기술요소로 부각되게 되었다. 하지만 프로그래밍을 학습하는데는 어느 정도 시간이 필요하며,  배운다하더라도  시스템 관리자가 막상 써먹을데도 여의치 않다.

하지만 앤서블이 나타나면서 판도가 달라졌다. 앤서블은 일단 배우기 쉽고 적용할 범위가 넓다. 그리고 기존에 각 서버마다 접속해서 반복해서 스크립트를 실행해야하는 일도 앤서블의 플레이북 몇 줄로 해결이 가능해졌다. 앤서블은 프로그래머를 위한 언어가 아닌, 시스템 관리자를 위한 언어이며, OS 관리 명령을 알고 있으면 쉽게 관련 모듈을 찾아서 적용할 수 있다. 하지만 꼭 시스템 관리지만 사용해야한다는 것은 아니다. 왜냐면 앤서블은 현재 900여개 이상의 모듈을 통해서 900여개 이상의 기능을 제공하기 때문에 IT 엔지니어라면 가상화 또는 클라우드 관라자이던, 네트워크 담당자이던, 자신에게 필요한 모듈을 활용해서 자동화를 이룰수 있게 도와준다. 그리고 이렇게 자동화를 시켜놓으면 업무효율성이 높아져 기존 2-3시간 걸리는 일을 10분 안으로 줄일 수 있고, 에러 발생확률도 현격히 낮출 수 있다.

그럼 이렇게 모든 작업을 자동화시켜 버리면 관리자들은 무엇을 해야 할까? 그 대답은 표준화 정립이다. 지루하고 반복되는 작업은 자동화시켜버리고, 어플리케이션에 필요한 시스템 파라미터 표준화, 어플리케이션 설정 파라미터 표준화등 더 안정적 운영과 빠른 배포를 위한 표준화를 수립하기 위해서 시간을 할애해야 한다. 자동화의 기본 전제는 잘 만들어진 표준화 수립임을 명심해야 한다. 또 다른 중요한 업무로는 어플리케이션의 테스트 시나리오를 최신으로 완벽하게 준비하는 것이다. 어플리케이션의 기능이 정상적으로 작동하는지 판단하는 기능테스트와 다른 서버와의 연계테스트 시나리오가 완벽하게 준비되어 있어야 실제 업무환경으로 전환되었을 때도 큰 문제없이 또는 사소한 것에 시간을 빼앗기지 않고 어플리케이션 또는 시스템 변경 작업을 진행할 수 있다.  

지금껏 IT관리자들은 “서비스가 늘 살아있어야 한다”라는 강박감에 밤샘 작업을 밥먹듯이 하고, 오분대기조 상태로 신경을 곤두세워왔다. 이제는 설정 표준화와 테스트 시나리오를 완벽히 준비하고, 이를 기반으로 IT자동화를 구현하여 밤에는 발뻗고 잠을 잘 수 있는 인프라 환경을 만들었으면 좋겠다.
