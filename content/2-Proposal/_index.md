---
title: "Proposal"
date: 2025-10-08
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Cinema Management System
## React + Spring Boot + AWS Cloud Solution

### 1. Executive Summary
Building a comprehensive web application for cinema management, including online ticket booking, showtime management, POS system at the counter, and business reporting. The solution uses modern technology (React, Spring Boot, RDS/S3) with optimized costs to suit small and medium cinemas looking to digitize their processes.

### 2. Problem Statement
#### What's the Problem?
Small and medium-sized cinemas currently still use manual processes for ticket sales, showtime management, and revenue reporting. This causes many problems such as long ticket processing times, difficulty in tracking real-time revenue, and lack of online booking systems to increase revenue.

#### The Solution
The Cinema Management System uses monolithic architecture with React frontend, Spring Boot backend, Amazon RDS MySQL for database, S3 for storage, and Redis for caching. The system supports online ticket booking with real-time seat maps, POS system for staff, VNPay/MoMo payment gateway integration, and reporting dashboard for management. Everything is deployed on AWS with Docker containerization.

#### Benefits and Return on Investment
The solution helps increase revenue through online ticket sales, reduce ticket processing time at the counter from 5-10 minutes to 1-2 minutes, automate real-time revenue reporting. Operating costs are estimated at ~$15-25/month for dev environment.
### 3. Solution Architecture
The system uses monolithic architecture containerized with Docker, deployed on AWS EC2 with Application Load Balancer. React SPA frontend is served via CloudFront CDN, connecting to Spring Boot backend through REST APIs. Database uses Amazon RDS MySQL for main data, Redis for caching and seat locking, S3 for static assets and backups.

![Cinema Management System Architecture](/images/2-Proposal/image1.jpg)
### Data Flow (AWS Data Flow)

| Step | Source → Destination | Description |
|---:|---|---|
| 1 | User → Route 53 | The user issues a DNS query to resolve the application's address. |
| 2 | Route 53 → CloudFront | Route 53 directs the request to the CDN (CloudFront). |
| 3 | CloudFront → Application Load Balancer (ALB) | CloudFront forwards dynamic/API requests to the ALB inside the VPC. |
| 4 | CloudFront → Amazon S3 | CloudFront serves static assets (images, resources) directly from S3 (origin). |
| 5 | User / CloudFront → Amazon Cognito | Authentication flow: user sign-in and identity management. |
| 6 | ALB → Amazon EC2 | ALB distributes traffic to EC2 instances (private subnets). |
| 7 | EC2 → Amazon RDS | The application on EC2 queries the relational database (primary data). |
| 8 | EC2 → Amazon ElastiCache | The application queries a high-speed cache to offload the database. |
| 9 | EC2 (Private Subnet) → NAT Gateway | EC2 uses the NAT Gateway as a secure egress to access the Internet. |
| 10 | NAT Gateway → Internet Gateway | The NAT Gateway forwards outbound traffic to the Internet Gateway. |
| 11 | EC2 / Other services → Amazon SNS | The application sends notifications/events to an SNS topic. |
| 12 | SNS → AWS Lambda | SNS notifications trigger Lambda functions for asynchronous processing. |
| 13 | Lambda / Other services → Amazon SES | Lambda or other services use SES to send transactional emails/notifications. |
| 14 | Amazon S3 → AWS Lambda | S3 object create/modify events trigger Lambda for file processing. |
| 15 | Services in the VPC → Amazon CloudWatch | Services publish logs and metrics for performance and error monitoring. |
| 16 | Dev → GitHub | Developers push source code to the GitHub repository. |
| 17 | GitHub → AWS CodePipeline | CodePipeline is triggered by changes in GitHub to start CI/CD. |
| 18 | CodePipeline → AWS CodeBuild | CodeBuild performs compiling, testing, and produces artifacts. |
| 19 | CodePipeline → AWS CloudFormation | CodePipeline invokes CloudFormation to deploy/update infrastructure as code. |
| 20 | CloudFormation → AWS Services | CloudFormation creates/updates/deletes AWS resources according to templates. |


### AWS Services Used
- **Amazon EC2**: Host application containers with Docker
- **Application Load Balancer**: Traffic distribution and SSL termination
- **Amazon RDS MySQL**: Main database for business data
- **Amazon S3**: Store static assets, backups and file uploads
- **CloudFront CDN**: Cache frontend and reduce bandwidth costs
- **Amazon SES**: Send email notifications and tickets
- **Amazon SNS** (optional): Push notifications for mobile


### Component Design
- **Frontend (React)**: PWA-ready with Tailwind CSS, responsive design support
- **Backend Application (Spring Boot Monolith)**: 
  - User Management Module: Authentication and user management
  - Movie Management Module: Movie and showtime management
  - Booking Module: Ticket booking logic and seat locking
  - Payment Module: VNPay/MoMo webhook integration
  - Notification Module: Email/SMS notifications
- **Database Design**: MySQL with main tables: users, theaters, screens, movies, showtimes, bookings, payments, tickets
- **Caching Layer**: Redis for session management and seat reservation locks

### 4. Technical Implementation
**Implementation Phases**
The project is divided into 3 main phases over 3 months:

**Phase 1: MVP Core (Month 1)**
- Week 1-2: Repository structure, CI/CD pipeline, database schema, skeleton Spring Boot
- Week 3: Core APIs (authentication, movies, showtimes, basic booking)
- Week 4: Basic React frontend (movie listing, seat selection)

**Phase 2: Feature Complete (Month 2)**
- Week 1: Complete booking flow and payment integration
- Week 2: QR code generation, POS staff tools
- Week 3: Mobile QR scanner, webhook handling
- Week 4: Basic dashboard, alpha testing

**Phase 3: Production & Polish (Month 3)**
- Week 1-2: Load testing, security audit, performance optimization
- Week 3: Bug fixes, UI/UX improvements
- Week 4: Production deployment, monitoring setup

**Technical Requirements**
- **Frontend Stack**: React 18, Tailwind CSS.
- **Backend Stack**: Spring Boot 3, Java 21, Spring Security, JPA/Hibernate, Maven
- **Database**: MySQL 8.0 with connection pooling and read replicas
- **Infrastructure**: Docker containers, AWS EC2, Application Load Balancer
- **Monitoring**: CloudWatch logs, application metrics, uptime monitoring

### 5. Timeline & Milestones
**Project Timeline**
- **Pre-Development (Month 0)**: Requirements gathering, architecture design, technology selection
- **Phase 1 - MVP (Month 1)**: Core functionality development
  - Milestone 1: Backend APIs and database (Week 3)
  - Milestone 2: Frontend booking flow (Week 4)
- **Phase 2 - Feature Complete (Month 2)**: Complete features and integrations
  - Milestone 3: Payment integration (Week 1)
  - Milestone 4: Staff tools and testing (Week 4)
- **Phase 3 - Production (Month 3)**: Optimization and deployment

**Key Deliverables**
- Functional MVP with online booking system
- Staff POS system with QR code scanning
- Management dashboard with real-time analytics
- Mobile-responsive web application
- Production deployment with monitoring

### 6. Budget Estimation
Cost estimation optimized for startup and SMB requirements:

**Infrastructure Costs (Monthly)**
- **Amazon EC2**: 
  - Development: t3.micro ($8.50/month)
  - Production: t3.small ($16.50/month)
- **Amazon RDS MySQL**: t3.micro ($13.14/month)
- **Application Load Balancer**: $16.20/month
- **Amazon S3**: $5-10/month (depending on usage)
- **CloudFront**: $0/month (free tier)
- **Domain & SSL**: $15/year

**Development Environment**: ~$15-25/month
**Production Environment**: ~$50-70/month

**One-time Development Costs**
- Development time: 3 months (Phase 1-3)
- Third-party integrations: VNPay/MoMo setup fees
- Domain registration and SSL certificates


### 7. Risk Assessment
#### Risk Matrix
- **Race Conditions (Seat Booking)**: High impact, medium probability
- **Payment Integration Issues**: High impact, medium probability  
- **Performance Bottlenecks**: Medium impact, medium probability
- **Security Vulnerabilities**: High impact, low probability
- **Cost Overruns**: Medium impact, low probability

#### Mitigation Strategies
- **Race Conditions**: Database locks, optimistic locking, Redis distributed locks for seat reservations
- **Payment Issues**: Sandbox testing early, webhook retry logic, transaction logging
- **Performance**: Database indexing, Redis caching, connection pooling, load testing
- **Security**: Spring Security, input validation, SQL injection prevention, regular security audits
- **Cost Control**: AWS budget alerts, resource monitoring, auto-scaling policies

#### Contingency Plans
- **Payment Gateway Backup**: Integrate multiple payment providers
- **Database Failover**: RDS Multi-AZ deployment for production
- **Application Scaling**: Auto-scaling groups and load balancer health checks
- **Disaster Recovery**: Automated backups, infrastructure as code with Terraform

### 8. Expected Outcomes
#### Technical Improvements
- **Uptime Target**: >99.5% availability
- **Performance**: <500ms response time (95th percentile)
- **Error Rate**: <0.1% for payment transactions
- **Scalability**: Support 1000+ concurrent users per cinema

#### Business Value
- **Revenue Growth**: 15-25% increase from online bookings
- **Operational Efficiency**: 50% reduction in ticket processing time
- **Customer Experience**: Self-service booking, mobile-friendly interface
- **Data Insights**: Real-time dashboard for business intelligence

#### Long-term Value
- **Market Expansion**: Template for franchise cinemas
- **Technology Foundation**: Monolithic architecture with modular design for future scalability
- **Competitive Advantage**: Modern tech stack and customer experience
- **Revenue Streams**: SaaS model with monthly subscriptions per cinema
