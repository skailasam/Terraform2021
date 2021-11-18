# Terraform2021
Two Tier App
resource "aws_instance" "JJtech_EC2" {
    ami ="ami-04ad2567c9e3d7893" 
    instance_type = "t2.micro"
    iam_instance_profile = aws_iam_instance_profile.EC2_profile.name
}
resource "aws_iam_role" "EC2_role" {
    name = "full_access_S3"
    assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Effect": "Allow",
      "Sid": ""
    }
  ]
}
EOF
    
}
resource "aws_iam_role_policy" "EC2_policy" {
  name = "full_access_full_policy"
  role = aws_iam_role.EC2_role.id
  policy =  jsonencode(
            {
        "Version": "2012-10-17",
        "Statement": [
            {
            "Sid": "s3getandputObject",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Effect": "Allow",
            "Resource": "*"
            }
        
        ]
        })
  
}
resource "aws_iam_instance_profile" "EC2_profile" {
    name = "EC2_name_profile"
    role = aws_iam_role.EC2_role.name
  
}