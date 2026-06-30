# LOCRMS Config Repository

Central configuration repository for LOCRMS (Loan Origination & Credit Risk Management System) microservices.

## 📋 Overview

This repository contains all configuration properties for the LOCRMS application services. These configurations can be:
- Stored locally and referenced by services
- Pushed to GitHub and managed via Spring Cloud Config Server
- Overridden by environment variables in production

## 📁 Structure

```
locrms-config-repo/
├── README.md                         (This file)
├── .gitignore                        (Git ignore rules)
├── application.properties            (Global config)
├── auth-service.properties
├── customer-service.properties
├── loan-service.properties
├── credit-service.properties
├── disbursement-service.properties
├── collections-service.properties
├── api-gateway.properties
└── eureka-server.properties
```

## 🚀 Services and Ports

| Service | Port | Database |
|---------|------|----------|
| Eureka Server | 8761 | - |
| Auth Service | 8081 | locrms_auth |
| Customer Service | 8082 | locrms_customer |
| Loan Service | 8083 | locrms_loan |
| Credit Service | 8084 | locrms_credit |
| Disbursement Service | 8085 | locrms_disbursement |
| Collections Service | 8086 | locrms_collections |
| API Gateway | 9090 | - |

## 🔧 Database Setup

Create the following MySQL databases:

```sql
CREATE DATABASE IF NOT EXISTS locrms_auth CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE IF NOT EXISTS locrms_customer CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE IF NOT EXISTS locrms_loan CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE IF NOT EXISTS locrms_credit CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE IF NOT EXISTS locrms_disbursement CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE IF NOT EXISTS locrms_collections CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

## 🔑 Important Configuration Values

### JWT Secret (auth-service.properties)
```properties
jwt.secret=your-secret-key-change-this-in-production-locrms-2026-secure-key
jwt.expiration=86400000  # 24 hours
```

⚠️ **Change this in production!**

### Database Credentials
```properties
spring.datasource.username=root
spring.datasource.password=root
```

### File Upload (customer-service.properties)
```properties
file.upload.max-size=5242880  # 5MB
file.upload.allowed-types=application/pdf,image/jpeg,image/jpg
```

## 🌐 API Gateway Routes

The API Gateway (port 9090) routes requests to:
- `/api/auth/**` → auth-service:8081
- `/api/customers/**` → customer-service:8082
- `/api/loans/**` → loan-service:8083
- `/api/credits/**` → credit-service:8084
- `/api/disbursements/**` → disbursement-service:8085
- `/api/collections/**` → collections-service:8086

## 📝 Configuration Profiles

Customize configurations based on environment:

### Development

```properties
spring.jpa.hibernate.ddl-auto=update
logging.level.root=INFO
```

### Production

```properties
spring.jpa.hibernate.ddl-auto=validate
logging.level.root=WARN
spring.datasource.hikari.maximum-pool-size=20
```

## 🔄 Using with Spring Cloud Config Server

1. Push this repository to GitHub:
```bash
git remote add origin https://github.com/your-org/locrms-config.git
git push -u origin main
```

2. Create a Config Server application with:
```properties
spring.cloud.config.server.git.uri=https://github.com/your-org/locrms-config.git
spring.cloud.config.server.git.default-label=main
```

3. Services will fetch configs from: `http://config-server:8888/{service-name}/{profile}`

## 🌍 Environment Variables

Override any property using environment variables:

```bash
# Example
export SPRING_DATASOURCE_PASSWORD=prod-password
export JWT_SECRET=production-secret-key
export SERVER_PORT=8081
```

## 📊 Logging Configuration

All services use Logback with:
- Console output for debugging
- Rolling file appenders (max 10MB per file)
- 10 files retention policy
- Total cap: 100MB

Logs are stored in: `logs/{service-name}.log`

## 🔐 Security Best Practices

1. **Never commit secrets** to version control
2. **Use environment variables** for sensitive data in production
3. **Rotate JWT secrets** regularly
4. **Limit database access** to specific IPs
5. **Use HTTPS** in production (configure in Spring)

## 📤 Pushing to GitHub

```bash
# Initialize repository
cd locrms-config-repo
git init
git add .
git commit -m "Initial LOCRMS configuration"

# Add remote and push
git remote add origin https://github.com/your-org/locrms-config.git
git branch -M main
git push -u origin main
```

## ✅ Verification Checklist

- [x] All service properties files exist
- [x] All ports are configured
- [x] All databases are configured
- [x] JWT secret is set
- [x] Logging is configured
- [x] File upload limits are set
- [x] Gateway routes are configured
- [x] Eureka is configured

## 📚 Additional Documentation

- See `CONFIG-REPO-SUMMARY.md` for detailed configuration breakdown
- See main project `README.md` for overall system architecture
- See service-specific `application.properties` for defaults

## 🤝 Contributing

When adding new configurations:
1. Update the relevant service properties file
2. Update this README with any new requirements
3. Document new properties clearly
4. Test configurations before pushing

## 📞 Support

For configuration issues:
1. Check the service-specific properties file
2. Verify database connections
3. Check Spring Boot logs
4. Ensure all databases are created

---

**Last Updated:** June 30, 2026  
**Version:** 1.0  
**Status:** Production Ready

