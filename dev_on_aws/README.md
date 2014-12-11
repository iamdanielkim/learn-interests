# Dev On AWS

## AWS 환경에 대해 이해한다.

* by [aws overview prezi]

* edge location은 53군데 cdn, dns만 있음. 한국에도 있음
* 1Region - N AZ. AZ사이에 대역폭은 0~10ms latency 수준임.

* EC2 - 가상 서버
* EBS - 물리 저장장치
    * 1EC2 - nEBS (O),  nEC2 - 1EBS (X)
    * 1Gb - 16Tb
    * snapshot 생성가능
* AMI - root  volume image
* RDS - rdb
    * mysql, oracle, mssql, post.sql, aurora 지원
    * 5Gb - 3Tb
* S3 - object storage
    * AMI, EBS snapshot가 저장되는 장소
    * AZ가 아닌 특정 Region에 위치
* DynamoDB
    * Key/Value, Document model 지원됨
    * AZ가 아닌 특정 Region에 위치
    * multiple AZ에 replication됨
* Auto Scaling
    * pre/post hook같은 것을 제공할듯(?)
* ELB
    * In same region / multiple AZ
    * (+) DynamoDB에 세션 넣고 쓰는게 BP
* lambda (!)
    * code를 올리면 그냥 돌아감. event에대한 sub만들때 사용하면 좋을듯
* CodeDeploy
* Cognito
    * n Screen상의 계정정보를 동기화할때 사용할 수 있음


# AWS IAM 을 통한 역할과 권한 관리 방법에 대해 안다.

* access key 가 주로 사용됨
* x.509 cert.는 soap api용 (없어지는 추세)
* Role
    * ec2 띄우때 role을 붙임. launching할때만 role을 붙일 수 있음
    * 정확하게는 ec2의 role profile에 role을 붙이는 거임. 그래서 launch할때 profile을 표준적으로 붙이도록 하는게 필요. (sdk에서만 됨. 관리콘솔UI에서는 안됨)
* EC2 ssh 접속과 IAM 계정과는 관계가 없다.

# S3 사용하기

* 두개의 endpoint가 있다.
    * enduser 서비스용, 관리용
* CloudFront 엣지 로게이션의 origin으로 사용
* "버킷 - 객체" 2단계 구성
* 객체 목록 조회 시, 가져올 수 있는 양이 한정되고 페이징 처럼 가져옴. 특정 옵션을 통해 필터링함.
    * max 1000개만 가져옴
    * 갯수 카운트 안됨
* 버킷 목록 표시 API는 region구분없이 다 나옴

# Dynamo 사용하기
* key/Value 스토어 형태
* 테이블 - 아이템 - 속성
* 아이템 크기: max 400k
* 지원 데이터 타입
    * 문자열/숫자/바이너리/set/map/list
* 트랜젝션 지원안함
    * 조건부 업데이트수행하도록 구현해야함.
     (ex- precondition을 통해 확인후 수행/ 다르면 operation 실패시킴)

* 기본이eventually consistent 임.
* 1 strongly consistent면 eventually consistent 두개 읽기 연산
* DynamoDBMapper 가 있음.

* provisiong한 값에 따라 가져오도록 읽는 값 조정 필수
* scaleout으로 성능보장. hash값이 같으면 같은 파티션

----

* provisiong 쓰기? 햇갈림?
* key 가 hash 또는 range ?

# SQS & SNS 사용하기

* SQS
    * region만 지정
    * 분산 queue/pull기반 메커니즘
    * (+) 메시지 여러개를 동시에 가져갈 수 있음/ long polling 가능
    * (!) 메시지 순서가 보장되지는 않음
    * (!) 여러번 전달될 가능성 존재
        * 설정한 timeout이 되면 처리실패로 간주하고 큐에서 다시 보임. 이전에 가져간 worker가 정상 실행해도 이런경우 중복실행

* SNS 사용하기
    * region 기반
    * ARN식별
    * topic별 policy붙일 수 있음
    * (?) worker권한을 iam으로 주면 안되나? 이미 admin으로 준거 아닌가?

# AWS보안/인증
* AWS개발은 AWS관리를 위한 개발 / 응용개발

* (권장 X) access key를 ec2에 그냥 복사해 넣을 경우
    * (-) auto scaling시 어려움.
    * (-) 보안에 약함
    * (-) ami에 같이 궈놀 경우. 보안 취약

* API endpoint에서 ec2에서 실행하는 건지 어떻게 아나?
    * ec2 - iam role을 갖고있음 (nego w/ security token service)
    * instance metadata service
    * ```curl http://169.254.169.254/atest/meta-data```
    * (+) access key를 직접 관리할 필요가 없음.


## 기존의 인증 체계가 있는데 어떻게 aws 권한 관리를 해야할까?

* (!) STS를 사용한다. ```임시보안자격증명```
    * 인증연동
    * 웹 신원 증명(FB, 구글) => 임시 권한 줌
        * identity broker가 sts에 요청
        * Ex) 엔드유저가 s3에 바로 업로드
    * 계정간 엑세스
        * Ex) A사용자의 Resource를 B사용자가 접근하려고 할때
    * assumeRole() 또는 getFederatedToken()


# CloudFormation/OpsWorks/ElasticBeanstalk

* Stack전체의 디플로이

# Code Deploy

* 1개 서버에 디플로이


# QnA

* 특정 role에 permission을 설정하고 이를 사용할 방법은 assume만 있나?

* ec2 붙을때 ssh key?
    * 직접 만든 ssh key로도 접근 가능하다.(권고하진않음)
* EC2에서 detach하면 EBS 데이터 날라감?
* EC2 terminate하면 EBS 데이터 날라감?
* cloud formation
* AWS의 과금모델에 대해 이해하고 저렴한 비용으로 사용하는 방법에 대해 안다.

----

[aws overview prezi]: https://prezi.com/cxpwi_og7lht/aws-regions-and-availability-zones

[CodeDeploy가 Windows Server도 지원]: http://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-set-up-new-instance.html

[SSH agent forwarding => Bastion Server 내에 Private Key 복사하고 유지해야 하는 불편함을 해소할 수 있는 구성 방법]: http://blogs.aws.amazon.com/security/post/Tx3N8GFK85UN1G6/Securely-connect-to-Linux-instances-running-in-a-private-Amazon-VPC

[S3 Resource-Level Permission 설정 방법]:  http://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html

[S3 Copy Object가 Cross Region으로 동작? 과금은? => 첫번째 링크와 같이 REST API 수준에서 지원, 두번째 링크와 같이 다운로드에 대해서만 과금되고 public internet 구간에 대한 다운로드 단가보다 Region간 단가가 저렴]: http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectCOPY.html

[s3/pricing]: http://aws.amazon.com/s3/pricing/

[최근 추가된 S3 Event Notifications을 이용해 구현 시 Eventual Consistency 고려 필요? => 여전히 update에 대해서는 고려 필요]: http://aws.amazon.com/blogs/aws/s3-event-notification/

[DynamoDB의 최신 개선 사항
    - => 아이템 당 용량 64KB에서 400KB로 증가,
    - Map/List 타입을 지원해 JSON Document 저장 및 연산 가능]: http://aws.amazon.com/blogs/aws/dynamodb-update-json-and-more

[DynamoDB 모델링 예제 => 게시판]:http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SampleTablesAndData.html

[지난주 re:Invent에서 발표된 새로운 서비스 요약]: http://aws.amazon.com/new/reinvent/?sc_ichannel=ha&sc_ipage=homepage&sc_icountry=en&sc_isegment=nc&sc_iplace=hero1&sc_icampaigntype=event&sc_icampaign=ha_en_reInvent_2014_News&sc_icategory=none&sc_idetail=ha_en_292_1&sc_icontent=ha_292&/


