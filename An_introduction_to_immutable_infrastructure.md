# An introduction to immutable infrastructure
왜 인프라스트럭처를 관리하지 않고, 프로그래밍해야하는가.

By Josh Stella June 9, 2015

- original article: https://www.oreilly.com/ideas/an-introduction-to-immutable-infrastructure

-----
변경불가 인프라스트럭처(Immutable infrastructure, II)는자동화와 프로그래밍을 통한 성공패턴을 활용해서 어플리케이션에 대한 안정성, 효율성 그리고 무중단에 대한 약속을 제공한다. 아직 변경불가 인프라스트럭처(II)에 대한 명확하고 표준화된 정의는 존재하지 않는다. 하지만 기본 사상은 사용자가 불변(immutability) 프로그래밍 컨셉을 사용해서 인프라스트럭처를 생성하고 운영하는 것이다. 무언가를 한번 생성하면, 다시는 바꾸지 않는다는 개념이다. 대신에 기존 인스턴스를 새로운 인스턴스로 대체하여 원하는 방식으로 작동케하는 개념이다.

채드 포울러(Chad Fowler)는 2013 블로그글에 "immutable infrastructure" 용어를 사용해서 "기존 서버는 쓰레기통에 버리고, 코드는 불태워버려라: 변경불가 인프라스트럭쳐와 폐기가능한 컴퍼넌트". 또한 다른 사람들도 유사한 개념을 말하고 있다. 마틴 포울러(Martin Fowler)는 2012년 불사조 서버라고 설명하고 있으며, 그렉 오즐(Greg Orzell), 제임스 카(James Carr), 키프 모리스(키프 모리스), 그리고 벤 버틀러-콜(Ben Butler-Cole) 등도 이에 대한 굉장한 사고와 작업을 내놓았다.

II는 운영환경의 완전 자동화를 필요로 한다. 또한컴퓨트 환경의 완전자동화를 구현하기 위해서는 환경 설정과 모니터링이 모두 API를 통해서 통신되도록 해야한다. 그러므로 II는 진정한 클라우드 환경에서면 실현가능하다. 물론 일부 요소를 구현해서 II의 일부 잇점을 얻을 수는 있다. 그러나 효율성과 장애극복이라는 II의 효과는 클라우드 환경를 통해서만 구현될 수 있다.

## 전문가가 관리하는 인프라스트럭쳐를 포기하라Give up on artisanal infrastructure
역사적으로, 시스템 관리자가 신경을 곤두세우는 요소는 시스템 업타입(살아있는 시간)과 유지보수였다. 왜냐면 전체 서비스와 어플리케이션이 그것들과 관계되어있다고 믿기 때문이다. 데이터센터에서 하드웨어는 매우 고가이기 때문에, 각 시스템들을 잘 관리해야 투자대비 효용성을 뽑아낼 수 있다. 반면 클라우드에서, 이런 사고는 시대착오적인 생각이고, 이런 생각을 버려야 좀 더 안정적이고, 더 단순하고 최종적으로는 안전한 서비스와 어플리케이션을 만들어낼 수 있다. 워너 보글(Wener Vogels, 아마존의CTO 이자 클라우드 시스템의 선구자)은 "서버를 끌어안고 있지 마라"라고 우리에게 얘기한다. 사실 우리가 서버를 안고있다해서 서버는 우리를 알아주지 않는다.

공들여서 관리하는 전통적인 방식의, 서버가 오래 살아있어야 하는 인프라스터럭처가 클라우드의 분산형 서비스를 처리하는데 비효율적이라는데에는 많은 이유가 있다.

* 운영 복잡성의 증가
분산형 서비스 아키텍처가 생겨나고, 동적 확장 기능이 사용되면서 모니터링해야할 요소들이 더욱 많아졌다. 그런데 전통적인 방식(죽지 않는 서버 운영 방식)으로는 수백, 수천대 서버들의 설정을 패치하고 업데이트하기가 어렵고 시간도 엄청 소모된다. 

* 상대적으로 느린 배포와 더 많은 실패
인프라스트럭처가 전통적인 유지보수 방식(스트립트든 설정관리 도구를 사용하던) 으로 관리되면, 유리몸([snowflake server](https://martinfowler.com/bliki/SnowflakeServer.html))과 같이 깨지기 쉽다. 그리고 소스관리 프로세스에서 직접 현황을 보지 않고서는 현재 인프라스트럭처 상태를 정확히 파악할 수 없다. 인프라스트럭처가 예상치 못하게 작동하고, 설정 변경을 추적하며 시간을 소비하고, 어플리케이션을 실행하면서 디버깅을 하다보면 고객과의 약속은 깨질수 밖에 없다.

* 장애를 감소시키기 위한 에러와 위험요소 감지하기
오래살지만 죽을 수밖에없는(long-lived, mutable) 전통적인 시스템은 장애피해를 막기 위해 에러와 위험요소를 파악하는데 의존한다. 하지만 거의 매일 버그리포트가 나오고, 위험한   이제 우리는 이런 원인 파악이 부질없는 짓(Sisyphean undertaking)이라는 걸 알게되었다.   
* 드릴 발사

Increasing operational complexity. 
The rise of distributed service architectures, and the use of dynamic scaling results in vastly more stuff to keep track of. Using mutable maintenance methods for updates or patching configurations across fleets of hundreds or thousands of compute instances is difficult, error-prone, and a time sink.

Slower deployments, more failures. 
When infrastructure is comprised of snowflake components resulting from mutable maintenance methods (whether via scripts or configuration management tools), there’s a lot more that can go wrong. Deviating from a straight-from-source-control process means accurately knowing the state of your infrastructure is impossible. Fidelity is lost as infrastructure behaves in unpredictable ways and time is wasted chasing down configuration drift and debugging the runtime.

Identifying errors and threats in order to mitigate harm. 
Long-lived, mutable systems rely on identifying error or threat to prevent damage. We now know that this is a Sisyphean undertaking, as the near daily announcements of high profile and damaging enterprise exploits attest. And those are only the ones reported. With II and automated regeneration of compute resources, many errors and threats are mitigated whether they are detected or not.

Fire drills. 
Artisanal infrastructure allows us to take shortcuts on automation that come back to bite us in unexpected ways, such as when a cloud provider reboots underlying instances to perform their own updates or patches. If we build and maintain our infrastructure manually, and aren’t in the regular routine of II automation, these events become fire drills.


Immutable infrastructure provides hope

II shares much in common with how nature maintains advanced biological systems, like you and me. The primary mechanism of fidelity in humans is the constant destruction and replacement of subcomponents. It underlies the immune system, which destroys cells to maintain health. It underlies the growth system, which allows different subsystems to mature over time through destruction and replacement. The individual human being maintains a sense of self and intention, while the underlying components are constantly replaced. Systems managed using II patterns are no different.

The benefits of immutable infrastructure are manifold if applied appropriately to your application and have fully automated deployment and recovery methods for your infrastructure.

BOOK


MPLS in the SDN Era
By Krzysztof SzarkowiczAntonio Monge Shop now  
Simplifying operations. With fully-automated deployment methods, you can replace old components with new versions to ensure your systems are never far in time from their initial “known-good” state. Maintaining a fleet of instances becomes much simpler with II since there’s no need to track the changes that occur with mutable maintenance methods.
Continuous deployments, fewer failures. With II, you know what’s running and how it behaves, deploying updates can become routine and continuous, with fewer failures occurring in production. All change is tracked by your source control and Continuous Integration/Continuous Deployment processes.
Reduces errors and threats. Services are built atop a complex stack of hardware and software, and things do go wrong over time. By automating replacement instead of maintaining instances, we are, in effect, regenerating instances regularly and more often. This reduces configuration drift, vulnerability surface, and level of effort to keep Service Level Agreements. Many of the maintenance fire drills in mutable systems are taken care of naturally.
Cloud reboot? No problem! With II you know what you have running, and with fully automated recovery methods for your services in place, cloud reboots of your underlying instances should be handled gracefully and with minimal, if any, application downtime.
immutable_infrastructure

We have to work very hard to maintain things, and when those things were physical boxes in a rack, this was necessary work because we manually configured hardware. But with logically isolated compute instances that can be instantiated with an API call in an effectively infinite cloud, “maintaining boxes” is an intellectual ball and chain. It ties us to caring about and working on the wrong things. Giving up on them enables you to focus on what matters to the success of your application, rather than being constantly pulled down by high maintenance costs and the difficulty in adopting new patterns.

This is a collaboration between O’Reilly and Luminal. See our statement of editorial independence.