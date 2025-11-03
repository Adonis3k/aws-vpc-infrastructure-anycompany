# Configuration Details — AnyCompany AWS VPC

| Resource Type       | Name/Tag                        | CIDR Block         | Route Table Association     |
|---------------------|----------------------------------|---------------------|------------------------------|
| **VPC**             | AnyCompany VPC                  | 10.0.0.0/18         | N/A                          |
| **Internet Gateway**| AnyCompany IGW                  | N/A                 | N/A                          |
| **Public Subnet 1** | AnyCompany subnet public-1      | 10.0.0.0/20         | AnyCompany public RT         |
| **Public Subnet 2** | AnyCompany subnet public-2      | 10.0.16.0/20        | AnyCompany public RT         |
| **Private Subnet 1**| AnyCompany subnet private-1     | 10.0.32.0/20        | AnyCompany private RT        |
| **Private Subnet 2**| AnyCompany subnet private-2     | 10.0.48.0/20        | AnyCompany private RT        |
| **Route Table**     | AnyCompany public RT            | `0.0.0.0/0 → IGW`   | Public Subnets               |
| **Route Table**     | AnyCompany private RT           | Default only        | Private Subnets              |
