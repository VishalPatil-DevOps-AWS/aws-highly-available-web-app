# aws-highly-available-web-app
# Highly Available App with ALB + Auto Scaling on AWS

Your server just crashed. Your users never noticed.

This hands-on guide shows how to build a self-healing, auto-scaling web app on AWS using an **Application Load Balancer (ALB)** and an **Auto Scaling Group (ASG)** across two Availability Zones.

## Architecture

```
User → ALB (AZ-a, AZ-b) → Target Group → EC2 instances managed by ASG
```

## What You Will Build

- Two security groups (`alb-sg`, `web-sg`) so only the ALB can reach your instances
- A Launch Template that boots an Apache web server
- A Target Group with health checks
- An internet-facing Application Load Balancer
- An Auto Scaling Group (min 2, desired 2, max 4) with ELB health checks
- A target tracking policy that scales on average CPU at 50%

## Steps

1. Create the security groups
2. Create the launch template
3. Create the target group
4. Create the Application Load Balancer
5. Create the Auto Scaling Group
6. Test load balancing, self-healing and scale-out

## Test It

- **Load balancing:** refresh the ALB DNS name and watch the Instance ID and AZ change
- **Self-healing:** terminate an instance and watch the ASG replace it
- **Scale out:** run `stress-ng --cpu 2 --timeout 300` and watch the ASG add instances

## Troubleshooting

| Problem | Fix |
|---|---|
| Targets show unhealthy | Allow port 80 from `alb-sg` on `web-sg` |
| ALB URL times out | Open port 80 on `alb-sg` and use public subnets |
| Blank or default page | Check `/var/log/cloud-init-output.log` |
| Instances keep getting replaced | Make sure the health check path returns 200 |
| Scaling never triggers | Increase load or lower the CPU target |

## Production Tips

- Use private subnets and keep only the ALB public
- Add an HTTPS listener with an ACM certificate
- Use a real health check path such as `/health`
- Alarm on unhealthy hosts with CloudWatch and SNS
- Define everything as code with CloudFormation or Terraform

## Clean Up

Delete in this order: ASG, ALB, Target Group, Launch Template, Security Groups.

## Read More

- Full article: [dev.to/vishal_patil_aws](https://dev.to/vishal_patil_aws)
- LinkedIn: [vishalpatil-devops-aws](https://www.linkedin.com/in/vishalpatil-devops-aws/)

By Vishal Patil | #vishalcloudjourney
