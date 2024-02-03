terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "4.67.0"
    }
  }
}
provider "aws" {
  region = "us-east-1"
}

resource "aws_lightsail_instance" "custom" {
  name              = "myserver"
  availability_zone = "us-east-1b"
  blueprint_id      = "amazon_linux_2"
  bundle_id         = "nano_1_0"
  user_data         = "sudo yum install -y httpd && sudo systemctl start httpd && sudo systemctl enable httpd && echo '<h>This is my first terraform projet</h1>' | sudo tee /var/www/html/index.html"
}

resource "aws_iam_user" "lb" {
  name = "rayan"
}

resource "aws_iam_group" "developers" {
  name = "developers"
  
}

output "instance_ip" {
  value = aws_lightsail_instance.custom.public_ip_address
}
output "my-arn" {
  value = aws_lightsail_instance.custom.arn
}