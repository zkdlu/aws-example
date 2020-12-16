## IAM (Identity and Access Management)
- AWS리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 

### IAM 계정 생성하기
1. AWS 로그인 후 우측 상단 계정을 클릭한 후 **[내 보안 자격 증명]** 클릭
2. 좌측 **[사용자]**탭에서 **[사용자 추가]** 클릭
3. 사용자 이름과 비밀번호 지정 후 다음
4. 권한 정책에 맞는 그룹 생성 및 선택 후 검토 (중간 태그 설정 가능)
5. 사용자 만들기
6. 이 단계에서만 액세스 키 저장 가능


## EC2 (Elastic Compute Cloud)
  1. 인스턴스 시작
  2. Amazon Machine Image(AMI) 선택
  3. 인스턴스 세부 구성
    - 아직 건들만한거 없음
  4. 스토리지 추가
    - 프리티어 최대 30GB
  5. 태그 추가
    - 인스턴스 관리 용이하게 하기 위함
  6. **보안그룹 구성**
    - **돈 나가기 싫으면 꼭 하자**
    - SSH 포트는 내 IP에서 접속 가능하도록
    > 보안그룹 설명에 한글이 들어가면 Invalid rule description. 오류가 뜨더라.
  7. 검토 및 시작
  8. 키페어 설정 후 시작
  
  
- Elastic IP 1개 무료 (연결을 안해두면 요금 청구)
- RDS 1개 무료 (생성시 Multi-AZ와 고성능 I/O인 Provisioned IOPS Storate를 사용 않도록 설정)
- ElasticCache 1개 무료 (무료 사용 대상은 t2.micro, Multi-AZ 체크 해제)
- EBS는 30GB까지 무료 (기본 세팅은 8GB) - Stop 상태여도 인스턴스를 가지고 있으면 볼륨을 차지한다. 안쓰면 삭제)

### SSH 접속하기
  - ssh -i "key pair.pem" {계정}@{public dns}
  ```bash
  $ ssh -i "key.pem" ubuntu@xxxxxxxxx.ap-northeast-2.compute.amazonaws.com
  ```
  > Permissions 0777 for 'key.pem' are too open. 라는 오류가 뜨고 접속이 안되면 키페어 파일의 권한이 공개되어있어서 그렇다.
  > - 리눅스에서는 chmod 400 을 해주면 되지만 Windows에서는 WSL을 써도 잘 안먹힌다. 
  > > WSL에서도 내부 리눅스 디렉토리로 파일을 옮겨주면 사용 가능
  ```bash
  $ cp "keypair.pem" ~/keypair.pem
  $ cd
  $ chmod 400 keypair.pem
  ```

## Amazon Code Deploy
- EC2/Lambda에 코드를 배포하는 데는 추가 비용이 부과되지 않음
  
## Amazon Systems Manager(Parameter Store)
- 패스워드, 데이터베이스 문자열, api키 등 관리를 위한 안전한 계층적 스토리지 서비스


## Amazon S3 (Amazon Simple Storage Service)
- 인터넷용 스토리지 서비스. 개발자가 쉽게 웹 규모 컴퓨팅 작업을 수행할 수 있도록 설계
- 용량당 비용 계산
> 데이터 스토리지는 저장되는 데이터에 대한 제한이 없지만 DB는 데이터에 대한 논리적 검사를 시행할 수 있음.

> accessKey랑 secretKey로 사용 가능했음.

> 서명 된 url에 유효기간이 있음. 그래서 cloudfront 써서 해결한다고 함. (조사 필요)

### 참고 
- https://ryan-han.com/post/aws/s3/

## CDN(Contents Delivery Network)
- 물리적으로 떨어져 있는 사용자에게 컨텐츠를 빠르게 제공할 수 있는 기술
- Origin Server의 리소스를 사용자에 가까운 곳에 위치한 Cache Server에 Content를 캐싱하고 요청시 Cache Server가 응답해줘 Latency를 낮춤
- 주로 캐싱이 되기 때문에 정적 파일에 사용 (동적 파일의 경우 주기적으로 업데이트 필요 ex) 주간 게임 랭킹)
- **Amazon CloudFront** (시간당 비용 계산)

> CloudFront 사용하는데 삽질을 하였다. S3처럼 sdk를 활용한 사용법이 따로 있는 줄 알았는데 S3와 연결된 도메인을 이용하면 끝났다. (cname : 대체 도메인)
> - ex) s3에 bucket에 저장된 key가 test/1.jpg라면 {cloudfront-domain}/test/1.jpg를 사용하면 됨.
> - 직접 aws에서 s3와 연결하는거는 하지 못했기 때문에 나중에 직접 해보기


## 프리티어 요금 정책
### Amazon EC2
- t2.micro 인스턴스 사용량의 월 750 시간
### Amazon S3
- 5GB의 S3 표준 저장소, 20000개의 Get 요청 및 2000개의 Put 요청
### Amazon RDS 
- db.t2.micro 인스턴스 750시간, 20GB 데이터베이스 스토리지, 1000만 I/O 및 20GB 백업 스토리지 
> EC2에 직접 설치하는게 비용적으로 저렴하나 빠른 서비스를 위한 이점이 있다고 함
### Amazon Cloudfront
- 매월 50GB 데이터 전송 및 2,000,000 개의 HTTP및 HTTPS 요청


# 비용 폭탄 사례 (경각심을 갖자 거지새끼야.)
- https://sanghaklee.tistory.com/32
- https://gun0912.tistory.com/45
- https://moonppower.tistory.com/17
- https://corikachu.github.io/articles/aws/refund-aws-unexpected-billing
