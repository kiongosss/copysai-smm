# CopysAI Disaster Recovery Procedures

## 1. Current Infrastructure Overview

### 1.1 Production Environment
- Single AWS Region: us-east-1
- 2 t3.medium EC2 instances for web servers
- 1 t3.medium EC2 instance for queue processing
- RDS t3.medium MySQL 8.0 instance with 100GB storage
- Redis Cache t3.micro instance
- S3 bucket for media storage (~50GB current usage)

### 1.2 Backup Environment
- Daily RDS snapshots retained for 30 days
- S3 versioning enabled with 30-day retention
- Local development backups on team machines

## 2. Recovery Time Objectives (RTO)

### 2.1 Critical Systems (4-6 hours)
- User Authentication & Session Management
- MySQL Database
- Core API Services
- Social Media Posting Engine

### 2.2 Important Systems (24 hours)
- Analytics & Reporting
- Historical Data Access
- Content Planning Features

### 2.3 Non-Critical Systems (48 hours)
- Admin Dashboard
- Beta Features
- Development Tools

## 3. Recovery Procedures

### 3.1 Database Recovery
Primary Contact: Lead Developer

1. Access AWS Console
   - Login to AWS Management Console
   - Navigate to RDS Dashboard
   - Locate latest automated snapshot

2. Initiate Database Restore
   ```bash
   # Note current timestamp for rollback
   timestamp=$(date +%Y%m%d_%H%M%S)
   
   # Create new DB instance from snapshot
   aws rds restore-db-instance-from-db-snapshot \
     --db-instance-identifier copysai-prod-$timestamp \
     --db-snapshot-identifier <snapshot-id> \
     --db-instance-class db.t3.medium
   ```

3. Update Application Configuration
   ```bash
   # Update .env file with new database endpoint
   sed -i "s/DB_HOST=.*/DB_HOST=new-endpoint/" .env
   
   # Clear application cache
   php artisan config:clear
   php artisan cache:clear
   ```

4. Verify Data Integrity
   ```sql
   -- Check recent records
   SELECT COUNT(*) FROM social_posts WHERE created_at > DATE_SUB(NOW(), INTERVAL 24 HOUR);
   
   -- Verify user sessions
   SELECT COUNT(*) FROM sessions WHERE last_activity > DATE_SUB(NOW(), INTERVAL 1 HOUR);
   ```

### 3.2 Application Server Recovery
Primary Contact: DevOps Lead

1. Launch New EC2 Instances
   ```bash
   # Launch new EC2 instance using saved AMI
   instance_id=$(aws ec2 run-instances \
     --image-id ami-copysai-latest \
     --instance-type t3.medium \
     --security-group-ids sg-prod \
     --subnet-id subnet-prod \
     --query 'Instances[0].InstanceId' \
     --output text)
   
   # Tag instance
   aws ec2 create-tags \
     --resources $instance_id \
     --tags Key=Name,Value=copysai-prod-$timestamp
   ```

2. Deploy Application Code
   ```bash
   # Clone repository
   git clone https://github.com/kiongosss/copysai-smm.git
   
   # Install dependencies
   composer install --no-dev --optimize-autoloader
   npm install --production
   
   # Build assets
   npm run build
   ```

3. Restore Configurations
   ```bash
   # Copy saved .env file
   scp -i ~/.ssh/copysai-prod.pem \
       /backup/.env \
       ubuntu@$new_instance:/var/www/copysai/.env
   
   # Set permissions
   chmod 640 .env
   chown www-data:www-data .env
   ```

### 3.3 Social Media API Recovery
Primary Contact: API Integration Lead

1. Verify API Credentials
   ```php
   # Test each platform connection
   php artisan test:facebook-api
   php artisan test:twitter-api
   php artisan test:instagram-api
   php artisan test:linkedin-api
   ```

2. Restore OAuth Tokens
   ```bash
   # Decrypt and restore OAuth tokens from backup
   php artisan oauth:restore-tokens --from-backup
   
   # Verify token validity
   php artisan oauth:verify-tokens
   ```

3. Test Posting Capabilities
   ```php
   # Send test post to development accounts
   php artisan test:social-post --platform=all
   ```

## 4. Current Limitations

### 4.1 Known Constraints
- No automatic failover capability
- Manual intervention required for recovery
- Limited to single AWS region
- No real-time database replication

### 4.2 Risk Mitigation
- Daily backups ensure maximum data loss of 24 hours
- All credentials and configurations backed up securely
- Documentation maintained for manual recovery steps
- Team trained on basic recovery procedures

## 5. Recovery Team

### 5.1 Primary Contacts
- Lead Developer: [Contact Details]
- DevOps Lead: [Contact Details]
- API Integration Lead: [Contact Details]

### 5.2 Escalation Path
1. Technical Lead (15-minute response)
2. CTO (30-minute response)
3. CEO (1-hour response)

## 6. Recovery Checklist

### 6.1 Immediate Actions
- [ ] Identify and document incident
- [ ] Notify recovery team
- [ ] Access AWS console
- [ ] Locate latest valid backup
- [ ] Begin recovery procedures

### 6.2 Communication
- [ ] Update status page
- [ ] Notify customer support
- [ ] Prepare customer communication
- [ ] Document incident timeline

### 6.3 Verification
- [ ] Test database connectivity
- [ ] Verify API integrations
- [ ] Check user authentication
- [ ] Confirm data integrity
- [ ] Test core functionality
