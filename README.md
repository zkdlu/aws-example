### Amazon S3 (Amazon Simple Storage Service)
- 인터넷용 스토리지 서비스. 개발자가 쉽게 웹 규모 컴퓨팅 작업을 수행할 수 있도록 설계
- 용량당 비용 계산
> 데이터 스토리지는 저장되는 데이터에 대한 제한이 없지만 DB는 데이터에 대한 논리적 검사를 시행할 수 있음.

### CDN(Contents Delivery Network)
- 물리적으로 떨어져 있는 사용자에게 컨텐츠를 빠르게 제공할 수 있는 기술
- Origin Server의 리소스를 사용자에 가까운 곳에 위치한 Cache Server에 Content를 캐싱하고 요청시 Cache Server가 응답해줘 Latency를 낮춤
- 주로 캐싱이 되기 때문에 정적 파일에 사용 (동적 파일의 경우 주기적으로 업데이트 필요 ex) 주간 게임 랭킹)
- **Amazon CloudFront** (시간당 비용 계산)
