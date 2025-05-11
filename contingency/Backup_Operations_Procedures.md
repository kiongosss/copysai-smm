# CopysAI Backup Operations Procedures

## 1. Current Backup Infrastructure

### 1.1 Production Database
```json
{
    "instance": "RDS MySQL 8.0 (t3.medium)",
    "storage": "100GB",
    "backup_window": "03:00-04:00 UTC",
    "maintenance_window": "Sun 05:00-06:00 UTC"
}
```

### 1.2 Application Servers
```json
{
    "web_servers": {
        "count": 2,
        "type": "t3.medium",
        "root_volume": "20GB"
    },
    "worker_server": {
        "count": 1,
        "type": "t3.medium",
        "root_volume": "20GB"
    }
}
```

### 1.3 Storage Usage (Current)
```bash
# Database Size
mysql -e "SELECT table_schema, 
         ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS 'Size (MB)' 
         FROM information_schema.tables 
         GROUP BY table_schema;"

# Media Storage
aws s3 ls s3://copysai-media --recursive | wc -l
aws s3 ls s3://copysai-media --recursive --human-readable | awk '{total += $3} END {print total/1024/1024" MB"}'
```

## 2. Backup Schedule

### 2.1 Automated Backups
```bash
# Database Snapshots
aws rds create-db-snapshot \
    --db-instance-identifier copysai-prod \
    --db-snapshot-identifier "copysai-prod-$(date +%Y%m%d)"

# Verify Snapshot Status
aws rds describe-db-snapshots \
    --db-instance-identifier copysai-prod \
    --query 'DBSnapshots[*].[DBSnapshotIdentifier,Status]'
```

### 2.2 Media Backups
```bash
# Sync media to backup bucket
aws s3 sync s3://copysai-media s3://copysai-backup/media/$(date +%Y%m%d)

# Enable versioning on backup bucket
aws s3api put-bucket-versioning \
    --bucket copysai-backup \
    --versioning-configuration Status=Enabled
```

### 2.3 Configuration Backups
```bash
# Backup environment files
timestamp=$(date +%Y%m%d_%H%M%S)
tar -czf "/backup/env_$timestamp.tar.gz" \
    /var/www/copysai/.env \
    /etc/nginx/sites-available/copysai.conf

# Upload to S3
aws s3 cp "/backup/env_$timestamp.tar.gz" \
    s3://copysai-backup/config/
```

## 3. Verification Procedures

### 3.1 Database Integrity
```sql
-- Check table status
CHECK TABLE users, social_posts, schedules;

-- Verify recent records
SELECT 
    table_name, 
    update_time,
    table_rows
FROM information_schema.tables
WHERE table_schema = 'copysai'
ORDER BY update_time DESC;
```

### 3.2 Backup Validation
```php
# Verify backup integrity
php artisan backup:verify

# Test restore in staging
php artisan backup:restore \
    --environment=staging \
    --backup=latest

# Verify data consistency
php artisan backup:compare-data
```

## 4. Recovery Procedures

### 4.1 Database Recovery
```bash
# Stop application
sudo systemctl stop copysai-api copysai-worker

# Restore database
aws rds restore-db-instance-from-db-snapshot \
    --db-instance-identifier copysai-prod-restore \
    --db-snapshot-identifier "copysai-prod-$timestamp"

# Update configuration
sed -i "s/DB_HOST=.*/DB_HOST=$new_endpoint/" .env

# Verify restoration
php artisan db:check-integrity
```

### 4.2 Media Recovery
```bash
# Restore specific file
aws s3 cp s3://copysai-backup/media/$date/$file \
    s3://copysai-media/$file

# Restore entire directory
aws s3 sync s3://copysai-backup/media/$date \
    s3://copysai-media

# Verify restoration
php artisan media:verify-integrity
```

## 5. Current Retention Policy

### 5.1 Database Backups
```yaml
Daily Snapshots:
  retention: 7 days
  storage: RDS automated backups
  cost: Included in RDS pricing

Weekly Snapshots:
  retention: 30 days
  storage: Manual RDS snapshots
  cost: $0.095 per GB-month

Monthly Snapshots:
  retention: 90 days
  storage: Manual RDS snapshots
  cost: $0.095 per GB-month
```

### 5.2 Media Storage
```yaml
Active Files:
  location: S3 Standard
  retention: Indefinite
  versioning: Enabled (30 days)

Backup Files:
  location: S3 Standard-IA
  retention: 90 days
  lifecycle: Move to Glacier after 90 days

Archival Files:
  location: S3 Glacier
  retention: 1 year
  retrieval: 3-5 hours
```

## 6. Monitoring and Alerts

### 6.1 Backup Monitoring
```bash
# CloudWatch Alarm for Failed Backups
aws cloudwatch put-metric-alarm \
    --alarm-name backup-failure \
    --metric-name BackupFailed \
    --namespace CopysAI \
    --statistic Sum \
    --period 300 \
    --threshold 1 \
    --comparison-operator GreaterThanThreshold \
    --evaluation-periods 1 \
    --alarm-actions arn:aws:sns:us-east-1:123456789012:backup-alerts
```

### 6.2 Storage Monitoring
```bash
# Check S3 Usage
aws cloudwatch get-metric-statistics \
    --namespace AWS/S3 \
    --metric-name BucketSizeBytes \
    --dimensions Name=BucketName,Value=copysai-media \
    --start-time $(date -d '7 days ago' --iso-8601=seconds) \
    --end-time $(date --iso-8601=seconds) \
    --period 86400 \
    --statistics Average
```

## 7. Known Limitations

### 7.1 Current Constraints
- Single region backups
- Manual verification required
- No automated restore testing
- Limited backup windows

### 7.2 Cost Considerations
```json
{
    "monthly_costs": {
        "rds_snapshots": "~$9.50 (100GB)",
        "s3_standard": "~$2.30 (100GB)",
        "s3_glacier": "~$0.40 (100GB)",
        "data_transfer": "~$0.90 (100GB)"
    },
    "total_monthly": "~$13.10"
}
```

## 8. Improvement Plan

### 8.1 Short-term (Q3 2025)
- Implement automated integrity checks
- Set up backup monitoring dashboard
- Create restore testing environment
- Document recovery procedures

### 8.2 Long-term (Q4 2025)
- Multi-region backup replication
- Automated restore testing
- Improved backup windows
- Enhanced monitoring
