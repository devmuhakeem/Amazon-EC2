# Introduction to Amazon EC2

A hands-on AWS lab covering the full lifecycle of an EC2 instance: launching it with termination protection, monitoring it, opening it up to web traffic, resizing it live, and finally testing that termination protection actually does its job.

## Scenario
A foundational lab walking through the core EC2 operations a cloud practitioner uses day to day — not just launching an instance, but monitoring, securing, resizing, and safely decommissioning one.

## What I did

### 1. Launched an EC2 instance with termination protection enabled
Launched a `t3.micro` Amazon Linux 2023 instance into a public subnet, with termination protection turned on from the start and a user data script that installed and started an Apache web server, serving a simple HTML page on boot.

### 2. Monitored the instance
Checked the instance's status checks (system reachability, instance reachability, attached EBS reachability), reviewed CloudWatch monitoring metrics, pulled the system log to confirm the user data script had run and installed httpd, and captured an instance screenshot — a useful fallback for troubleshooting when SSH/RDP access isn't available.

### 3. Diagnosed and fixed a security group block
Tried loading the web server's public IP and got nothing — the security group had no inbound rules at all, so port 80 traffic was blocked by default. Added an inbound rule allowing HTTP (port 80) from anywhere, then confirmed the web page loaded successfully.

### 4. Resized the instance live
Stopped the instance, changed its instance type from `t3.micro` to `t3.small` (doubling available memory), modified its EBS root volume from 8 GiB to 10 GiB, then started it back up with the new specs.

### 5. Tested termination protection
Attempted to terminate the instance and got exactly the expected error — termination blocked because protection was enabled. Disabled the `disableApiTermination` attribute, then successfully terminated the instance.

## Key takeaways
- A security group with zero inbound rules blocks everything by default — the "nothing loads" result wasn't a bug, it was the firewall doing exactly what it's supposed to do until explicitly opened
- Termination protection is a deliberate speed bump, not a hard block — it forces a second, explicit decision before something irreversible happens, which is exactly the right design for a destructive action
- Resizing an instance type and its EBS volume are two separate operations (compute vs. storage), and both require the instance to go through a stop/modify/start cycle rather than happening live

## Tools
Amazon EC2, Amazon CloudWatch, Amazon EBS, Security Groups

---
*Completed as an AWS hands-on lab, including a passed knowledge check assessment.*
