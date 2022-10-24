# 2022-10-23
## Database instance should be in VPC
Database instance should be in VPC.

# 2022-10-24
## Make RDS publicly accessible
RDS > Databases > Database instance에서 Modify 버튼 클릭

Connectivity > Additional configuration > Public access에서 Publicly accessible 선택

RDS > Databases > Database instance에서 instance 선택 후 security group 선택, Inbound rules에 inbound rule 추가
