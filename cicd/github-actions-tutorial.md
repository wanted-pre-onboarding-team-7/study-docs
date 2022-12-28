[CI/CD 5분 개념 정리 (현업에서 쓰는 개발 프로세스) - 드림코딩](https://www.youtube.com/watch?v=0Emq5FypiMM)

# CI

1. 코드 변경을 주기적으로 빈번하게 머지해야한다.
2. 통합을 위한 단계(빌드, 테스트, 머지)의 자동화 - 팀에서 만든 ci script를 통해서 build, test 등 실행
   => 개발 생산성 향상, 문제점을 빠르게 발견, 코드 퀄리티 향상

# CD (Continuous Delivery)

> 준비된 Release가 명시적 승인을 얻은 후 배포가 가능하다.

# CD (Continuous Deployment)

> 준비된 Release가 명시적 승인 없이 자동으로 프로덕션이 일어난다.

## Continuous Delivery와 Continuous Deployment 차이점 설명

[AWS](https://aws.amazon.com/ko/devops/continuous-delivery/)

> 지속적 전달과 지속적 배포의 차이점은 프로덕션에 업데이트(릴리스)에 대한 수동 승인 존재 여부입니다.
> 지속적 배포(Continuous Deployment) 경우 명시적 승인 없이 자동으로 프로덕션이 일어납니다.

지속적 전달 - 수동 승인 존재
지속적 배포 - 자동 승인 존재

# TMI : 무중단배포

> 다운타임없이 서버를 운영할 수 있게 해주는 배포

1. Rolling
2. Canary
3. Blue-Green
