# Terraform-Project
If you are new to Terraform, practice this simple project to understand how it works.

## **Terraform AWS Web Server Project**

This project demonstrates how to use Terraform to deploy a simple web server on AWS.


## Prerequisites
- Terraform installed
- AWS account with credentials configured
- AWS CLI installed and configured

---

#### 1. **`main.tf`**
```hcl
provider "aws" {
  region = "us-east-1" # Change to your preferred region
}

resource "aws_instance" "web_server" {
  ami           = "ami-0c02fb55956c7d316" # Amazon Linux 2 AMI (us-east-1)
  instance_type = "t2.micro"              # Free tier eligible

  tags = {
    Name = "WebServer"
  }

  user_data = <<-EOF
              #!/bin/bash
              sudo yum update -y
              sudo yum install -y httpd
              sudo systemctl start httpd
              sudo systemctl enable httpd
              echo "<h1>Hello, World! This is my Terraform Web Server.</h1>" | sudo tee /var/www/html/index.html
              EOF
}

output "public_ip" {
  value = aws_instance.web_server.public_ip
}
```

---

#### 2. **`outputs.tf`**
```hcl
output "public_ip" {
  value = aws_instance.web_server.public_ip
}
```

---

#### 3. **`variables.tf`** (Optional)
```hcl
variable "region" {
  default = "us-east-1"
}

variable "instance_type" {
  default = "t2.micro"
}
```

---

## Usage
1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   ```
2. Navigate to the project directory:
   ```bash
   cd your-repo-name
   ```
3. Initialize Terraform:
   ```bash
   terraform init
   ```
4. Apply the configuration:
   ```bash
   terraform apply
   ```
5. Access the web server using the public IP output by Terraform.

## Clean Up
To destroy the resources:
```bash
terraform destroy
```
