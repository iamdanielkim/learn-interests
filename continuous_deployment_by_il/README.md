# Continuous Deployment

**Ultimately, every large feature is split into a sequence of incremental changes deployed immediately.**

    Continuous Deployment is deploying early and often
    to avoid painful and expensive deploys.

* 배포의 안전성을 보장한다.
    * 모든 코드는 자동화 테스트가 있어야함
    * 모든 코드는 배포되기전 테스트를 통과해야함
* 배포 과정을 자동화 한다.
    * Deploys are automated and fool-proof.
    * 배포과정은 자동화되고 오류가 있거나 잘못사용되지 않음
    * Deploys have ZERO-Downtime.
    * 배포는 제로 다운타임을 가질것
* 작은 변경을 자주 배포한다.
    * Provides lower chance of failure
    * 실패할 확률을 줄인다.
    * Yields fewer problems in the event of failure
    * 오류상황에서 문제를 일으키지 않음.

## Continuous Deploy의 가치

* An automated process is safer than a non-automated process
* A frequently used process is safer than an infrequently used process
* Designing for failures is safer than assuming you won't make mistakes
    * (!) 실수는 항상일어난다. DRY가 중요.
* Keeping code deployable is safer than having broken builds between releases
* Responding to issues by deploying changes is safer than being forced to wait until the next release cycle

## Benefit

* Automation removes errors.
* Eliminates stressful release crunch.
* Users continuously see improvements.
* Reducing batch size helps drive out waste.
* Faster responses to customer issues.
* A/B testing is easier due to less release overhead.
* Encourages experimenting with new ideas.
* Allows for fine-grained monitoring of changes.
* Delivers always on expectation of modern software.
* Provides satisfaction from seeing constant progress.



# Deploy Pipeline

    Deployment pipeline은 코드들이
    production환경으로 배포되기위해 거쳐야하는단계들을 의미한다.

* Develop > Assure > Monitor 로 크게 나눠 볼 수 있다.

## Measuring Speed, Uptime, Happiness

* Speed - 커밋 부터 베포까지 얼마나 걸리는가?
* Uptime - 얼마나 자주 빌드가 깨지는가 / 파이프라인이 죽는가?


    이를 위한 minimum value가 필요하다.
    * 5minute commit-to-live
    * 99% uptime

* Developer Happiness - 빠르게 pain point를 찾는다.
    * 개발자들이 잦은 commit을 하지 않으면 문제가 잠복해버린다.
    * Ex) 간단한 monthly thumbup survey등


    Your developers will be happier knowing
    they have a voice and are being heard.


# Exercise 1.

* 실습 환경
    * 가상서버 발급해줌
        * svn 깔려있음
        * 소스 코드 svn에 등록되어 있음.
        * wep app 실행되어 있음
        * jenkins에 설정 되어 있음
    * 개발환경 구성
        * 코드 checkout / 환경 구성 / 실행

* 실습 수행
    * 자동화 인수 테스트
    * exercise 시작
        * 테스트 변경 -> fix
        * jenkins에서 test - deploy 수행



# Continuous Integration

* Rigorous Continuous Integration


    Rigorous continuous integration means committing
    several times a day and never waiting
    more than a day to commit to **trunk**.

* Big Changes


    There are no changes "too big" for continuous integration

```모든 변경은 작은 변경들의 모음으로 쪼겔수 있다. ```

* (?) web framework을 upgrade하는 걸 하루 안에 할 수 있어?
    * 인스턴스를 두개 뛰우고 old -> new proxy해서 항상 돌아가게 한다.
* (?) Changing the serialization format for every class in our application.
* (?) Spiking a design change to test potential usability improvements.
* (?) Rewriting a component of the application in a new language.

## Safe or UnSafe

* (x) All developers are required to go through a three-week boot-camp before being given commit privileges.
* (o) I deployed code with a bug that broke sign-up. In response, we added significant test coverage and monitoring to sign-up.
* (x) I don't like to commit code on Friday because everyone else is rushing to get their features in before the weekend.

* TDD Step
    * Red - Green - Refactor - Integrate (Optional)

* 커밋하는 모든 기능은 그에 해당하는 Test를 가져야한다.
* CD는 TDD를 요구하지 않지만 TDD를 장려하는 역할을 한다.



# Tackling Legacy Code

Michael Feathers defines "Legacy Code" as code without test coverage.


    It is the developers' responsibility to ensure that any code
    they're touching has adequate test coverage before committing.

When you touch an area of the codebase you haven't touched before, **try breaking it intentionally and rerunning the test suite**. If the tests don't fail, improve them before moving on to your new code.

* (?) Initial development on a brand new project.
Does this require extra investment in test coverage?
    * This does not require extra test investment.
* (?) A new service that talks to legacy code over public APIs. Does this require extra investment in test coverage?
    * This does not require extra test investment.
* (?) A small feature that extends existing legacy code. Does this require extra investment in test coverage?
    * This requires extra test investment.

# Incremental Deployment Strategies

* Risk First
    * 불확실성을 빠르게 제거하기 위해 최고로 risk한 변경부터 배포
* Persist First
    * domain로직부터 적용/배포하면, 이를 나중에 쓸수 있고 점진적으로 변화를 꾀할 수 있음.
* UI First
    * 이해당사자들과 빨리 interact하기 위해 빠르고 값싸게 적용하고 아이디어들을 발전시킴
    * stakeholder들이 ok할때까지 domain/persistence개발을 늦춤
* Refactor First
    * 코드는 지속적으로 꾸준이 설계를 향상시켜야하고, 이를 한번의 Big Release로하기엔 위험함.
    * 여러번에 걸쳐 조금씩 작고, 안정적인 코드를 릴리즈함.

# Schema Changes

* 새로운 스키마로 바꾸는 동안 어떻게 코드를 기존의 스키마와도 호환되고 새 스키마와도 호환되도록 할 수 있을까?

## email column을 추가한다면?

```
# old schema
CREATE TABLE user ( first_name varchar(255), last_name varchar(255) );

# The code makes this query:
SELECT * FROM user;

```

### marching pattern

1. SELECT first_name, last_name FROM user;
1. ALTER TABLE user ADD email varchar(255);
1. SELECT first_name, last_name, email FROM user;


* 3번이나 배포해야하는데 ROI나와?
    * 사용자에게 영향을 끼치지 않음
    * 아무런 사고없이 롤백할 수 있음
    * 변화를 작게 쪼겜으로서, 리스크를 줄임

### QnA - CREATE TABLE product ( price int ) 에 price_in_cents 추가하기

```
* ALTER TABLE product ADD price_in_cents int;
* ALTER TABLE product DROP COLUMN price; and then deploy code that updates the GUI to allow prices like $9.99.
* Deploy change that removes all references to price.
* Deploy code that changes all updates to product to update both price and price_in_cents, setting price_in_cents = price * 100.
* UPDATE product SET price_in_cents=price*100;
```

**schema changing을 pipeline에 표준적인 절차로 만들어야한다**

# Exercise - Coupon Code Feature

(+) Versioned Schema

python은 south라는 녀석을씀. django.south

Implement coupon codes.
    * Add an admin page to that generates codes.
    * Each code should specify the % off.
    * Codes may be entered on the /checkout page, as part of a new form
    * The forms submit button ("Apply") should update the displayed price on the /checkout page
    * The coupon code <form> element must use the id "coupon_code"

# Feature Flipper

  더이상 머지할 필요없고, 모든 코드는 커밋되자 마자 통합된다. (그 코드가 언제 릴리즈되는지는 상관없이) - flickr

* Feature Flipper is a centrally managed configuration parameter that controls the visibility of a feature.
    * Feature Toggle과 비슷한 개념인듯
* (?) If the code I deploy isn't being run because it's behind a feature flipper that's set to "off", don't I lose the benefits of Continuous Deployment?
* (?) Why so much fuss about a boolean? Can't I just comment the code out while it's in development?


(!) Exercise 5 skip (function coupon codes)

Release To QA

* 무조건 자동화만하라는 것은 아님. QA가 필요한 경우도 있다. manual test도 pipeline의 부분이되도록 함. Trade Off가 있음.

Gradual Rollout

* 1%의 사용자에게만 먼저 배포하여 risk 줄임
* Stress Test, Userbility Test 역할을 할 수 있음
    * Google Chrome has four release channels:
        * stable, beta, dev and canary.

Dark Launch

* 사용자에게 안보이게 기능 릴리즈후, production환경에서 시뮬레이션 수행. full scale load test 가능


(!) Exercise 6 skip (Charge Discounted Price Solution)
(!) Exercise 6 skip (Releasing the Coupon Code Feature)

Production Monitoring

* 3Level Of Monitoring






----
