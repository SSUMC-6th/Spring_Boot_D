# Chapter 11. 무중단 CI/CD (Github Action+AWS Elastic Beanstalk)

## 1️⃣ CI / CD란?

### ✅ 개념
- 애플리케이션 개발 단계부터 배포 때까지의 모든 단계를 자동화를 통해 효율적이고 빠르게 사용자에게 배포할 수 있는 것

### ✅ CI(Continuos Integration)
- 애플리케이션의 버그 수정이나 새로운 코드 변경이 주기적으로 빌드 및 테스트 되면서 공유되는 리포지터리에 merge되는 것을 의미

- 개발자를 위한 자동화 프로세스, Code - Build - Test 단계
	- Code : 개발자가 코드를 원격 코드 저장소 (Ex. github repository)에 push하는 단계
	- Build : 원격 코드 저장소로부터 코드를 가져와 유닛 테스트 후 빌드하는 단계
	- Test : 코드 빌드의 결과물이 다른 컴포넌트와 잘 통합되는 지 확인하는 과정



### ✅ CD(Continuous Delivery / Deployment)
- CI에서 개발자들이 검증하고 난 뒤 배포를 수동적으로 진행
- 만약 배포할 준비가 되자마자 자동화를 통해 진행하는 것을 -> **Continuous Deployment**
- Release - Deploy - Operate 단계
	- Release : 배포 가능한 소프트웨어 패키지를 작성
	- Deploy : 프로비저닝(인프라 생성 및 프로세스)을 실행하고 서비스를 사용자에게 노출 => 실질적인 배포
	- Operate : 서비스 현황을 파악하고 생길 수 있는 문제를 감지
