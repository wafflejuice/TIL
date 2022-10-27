# 2022-10-23
## Database instance should be in VPC
Database instance should be in VPC.

# 2022-10-24
## Make RDS publicly accessible
RDS > Databases > Database instance에서 Modify 버튼 클릭

Connectivity > Additional configuration > Public access에서 Publicly accessible 선택

RDS > Databases > Database instance에서 instance 선택 후 security group 선택, Inbound rules에 inbound rule 추가

# 2022-10-25
## access RDS over EC2
ssh로 EC2 설정 후 RDS 접근
1. set SSH tunnel configuration

# 2022-10-27
## CIDR(Classless Inter-Domain Routing)
classful network를 대체하기 위해 등장. IP 주소를 여러 영역으로 나누는 방식 중 하나.

10.10.1.0/24 -> leftmost 24 bits가 고정되고, 2^8=256개의 가용 IP address 존재
