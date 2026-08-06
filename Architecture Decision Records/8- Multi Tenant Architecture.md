# Multi-Tenant Architecture

Multi-Tenant Architecture is a software architecture pattern where a single application instance serves multiple customers (tenants) while keeping their data isolated and secure.

A tenant can be:

- Organization
- Government Entity
- Ministry
- Department
- Company
- Customer
- Business Unit

Multi-Tenancy is one of the most important architectural decisions when designing SaaS platforms, Government Shared Services Platforms, Enterprise Portals, ERP Systems, and Cloud Applications.

---

# Core Principle

Instead of deploying a separate application for every customer:

```text
Customer A

Application A

Database A

-----------------

Customer B

Application B

Database B
```

A Multi-Tenant system shares infrastructure.

```text
Single Application

↓

Tenant A
Tenant B
Tenant C
Tenant D
```

Each tenant sees only its own data.

---

# Why Multi-Tenant Architecture Exists

Imagine building:

```text
Employee Self Service Platform
```

for:

```text
Ministry Of Finance

Ministry Of Health

Ministry Of Education

Ministry Of Transport
```

Instead of deploying:

```text
4 Applications

4 Databases

4 Deployments
```

We deploy:

```text
1 Platform

Multiple Tenants
```

This dramatically reduces:

- Infrastructure Cost
- Maintenance
- Deployment Effort
- Operational Overhead

---

# What Is A Tenant?

A tenant represents a logical customer boundary.

Example:

```text
Tenant 1

Ministry of Education

---------------

Tenant 2

Ministry of Health

---------------

Tenant 3

Ministry of Finance
```

Each tenant can have:

```text
Users

Roles

Permissions

Configurations

Workflows

Documents

Reports
```

while sharing the same application.

---

# Single Tenant vs Multi Tenant

## Single Tenant

```text
Customer A

Application

Database
```

Dedicated resources.

---

## Multi Tenant

```text
Application

↓

Tenant A
Tenant B
Tenant C

↓

Database
```

Shared resources with isolation.

---

# Business Scenario #1

## Government Shared Services Platform

Departments:

```text
HR

Finance

Transportation

Municipality

Public Works
```

Requirements:

```text
Shared Platform

Separate Data

Independent Users
```

Recommended:

✅ Multi-Tenant Architecture

---

# Business Scenario #2

## SaaS Employee Portal

Customers:

```text
Company A

Company B

Company C

Company D
```

Each customer needs:

```text
Leave Management

Claims

Training

Employee Directory
```

Recommended:

✅ Multi-Tenant Architecture

---

# Business Scenario #3

## Permit Management Platform

Organizations:

```text
Municipality A

Municipality B

Municipality C
```

Need:

```text
Same Features

Different Data
```

Recommended:

✅ Multi-Tenant Architecture

---

# Business Scenario #4

## Banking Platform

Different banks use a common solution.

```text
Bank A

Bank B

Bank C
```

Can share platform while isolating:

```text
Customers
Accounts
Transactions
Cards
```

---

# High Level Architecture

```text
Users

  │

  ▼

Application

  │

  ▼

Tenant Resolution

  │

 ┌─────────────┬─────────────┬─────────────┐

 ▼             ▼             ▼

Tenant A    Tenant B     Tenant C

  │             │             │

  ▼             ▼             ▼

Data Access Layer
```

Before processing a request:

```text
Identify Tenant

↓

Apply Tenant Context

↓

Execute Business Logic
```

---

# Tenant Identification Methods

## Method 1 - Subdomain

Example:

```text
finance.company.com

health.company.com

education.company.com
```

Gateway identifies tenant from URL.

---

## Method 2 - Header

Example:

```http
X-Tenant-Id: Finance
```

API reads tenant identifier.

---

## Method 3 - JWT Claims

Example:

```json
{
  "UserId": "123",
  "TenantId": "Finance"
}
```

Most common enterprise approach.

---

## Method 4 - Custom Domain

Example:

```text
companyA.com

companyB.com
```

Useful for SaaS platforms.

---

# Multi-Tenant Database Strategies

This is the most important architectural decision.

---

# Strategy 1

## Shared Database Shared Schema

All tenants share:

```text
One Database

One Schema
```

Example:

```text
Employees

------------------------------------------------

EmployeeId

TenantId

Name

Email
```

Data separated via:

```text
TenantId
```

---

## Architecture

```text
Application

↓

SQL Server

↓

Employees

LeaveRequests

Claims

Documents
```

All include:

```text
TenantId
```

---

## Advantages

✅ Lowest Cost

✅ Easiest Operations

✅ Shared Infrastructure

✅ Easy Deployment

---

## Disadvantages

❌ Data Isolation Risk

❌ Complex Queries

❌ Larger Databases

❌ Compliance Challenges

---

## Best For

```text
Small SaaS

Internal Platforms

Government Shared Portals
```

---

# Strategy 2

## Shared Database Separate Schema

One database.

Separate schemas.

Example:

```text
Database

├── TenantA.*
├── TenantB.*
├── TenantC.*
```

---

## Architecture

```text
Application

↓

Database

├── finance
├── health
├── transport
```

---

## Advantages

✅ Better Isolation

✅ Easier Backup

✅ Easier Reporting

---

## Disadvantages

❌ Schema Proliferation

❌ Management Complexity

---

## Best For

```text
Government Systems

ERP Platforms

Medium Size SaaS
```

---

# Strategy 3

## Separate Database Per Tenant

Highest isolation.

```text
Tenant A

Database A

-------------

Tenant B

Database B

-------------

Tenant C

Database C
```

---

## Architecture

```text
Application

↓

Tenant Resolver

↓

Database A

Database B

Database C
```

---

## Advantages

✅ Strong Isolation

✅ Independent Backups

✅ Independent Scaling

✅ Better Compliance

✅ Easier Tenant Migration

---

## Disadvantages

❌ Higher Cost

❌ More Databases

❌ Operational Complexity

---

## Best For

```text
Banking

Healthcare

Government Critical Systems

Large Enterprise SaaS
```

---

# Real World Examples

## Microsoft 365

Conceptually uses multi-tenancy.

```text
Organization A

Organization B

Organization C
```

Same platform.

Separate data.

---

## Salesforce

```text
Customer A

Customer B

Customer C
```

Shared platform.

Logical isolation.

---

## Government Shared Services

```text
Ministry A

Ministry B

Ministry C
```

Same software.

Independent data access.

---

# Tenant Aware Request Flow

Example:

Employee Login

```text
User Login

↓

Identity Provider

↓

JWT Generated

↓

Contains TenantId

↓

Application

↓

Tenant Context Loaded

↓

Database Query

↓

Tenant Filter Applied

↓

Return Data
```

---

# Tenant Aware Query

Bad:

```sql
SELECT *
FROM Employees
```

Dangerous.

---

Good:

```sql
SELECT *
FROM Employees
WHERE TenantId = @TenantId
```

Always isolate tenant data.

---

# Security Considerations

## Tenant Isolation

Most critical requirement.

```text
Tenant A

Can Never Access

Tenant B Data
```

---

## Authorization

Roles must be tenant-specific.

```text
Admin

Tenant A

≠

Admin

Tenant B
```

---

## Audit Logging

Track:

```text
Tenant

User

Operation

Timestamp
```

---

## Encryption

Sensitive tenant data should be encrypted.

---

# Scaling Strategies

## Horizontal Scaling

```text
Application Instance 1

Application Instance 2

Application Instance 3
```

All tenants share servers.

---

## Tenant Specific Scaling

Large tenant:

```text
Tenant A
```

may receive dedicated resources.

---

## Database Partitioning

Large platforms often implement:

```text
Tenant Partitioning

Sharding
```

for scalability.

---

# Advantages

## Reduced Cost

One platform serves many customers.

---

## Easier Deployment

Deploy once.

Serve everyone.

---

## Faster Feature Delivery

New features become available to all tenants.

---

## Better Resource Utilization

Infrastructure is shared.

---

## Centralized Support

Single platform to maintain.

---

# Disadvantages

## Security Risk

Poor implementation can expose tenant data.

---

## Performance Contention

One tenant can consume excessive resources.

Example:

```text
Tenant A

High Load
```

affecting others.

---

## Complex Testing

Need to validate:

```text
Tenant Isolation

Permissions

Configurations
```

for every tenant.

---

## Customizations

Some tenants may need unique workflows.

This increases complexity.

---

# Multi Tenant + Microservices

Common enterprise architecture:

```text
API Gateway

↓

Tenant Resolution

↓

Customer Service

Billing Service

Document Service

Notification Service

↓

Databases
```

Each service remains tenant-aware.

---

# Multi Tenant + CQRS

Example:

```text
Commands

Create Employee

Tenant A

↓

Write Database

----------------

Queries

Search Employees

Tenant A

↓

Read Database
```

Tenant filtering applies everywhere.

---

# Multi Tenant + Event Driven Architecture

Example:

```text
EmployeeCreated

Tenant A

↓

Event Bus

↓

Notification Service

↓

Tenant A Email Configuration
```

Tenant context must travel with events.

---

# Multi Tenant + SaaS

This is the most popular combination.

```text
Single SaaS Platform

↓

Multiple Customers

↓

Subscription Plans

↓

Tenant Isolation
```

---

# When To Use Multi Tenant Architecture

Use when:

✅ SaaS Platforms

✅ Government Shared Platforms

✅ ERP Systems

✅ Employee Self Service

✅ Permit Management

✅ Customer Portals

✅ Enterprise Platforms

✅ Cloud Solutions

---

# When NOT To Use Multi Tenant

Avoid when:

❌ Single Customer Solution

❌ One Organization Only

❌ Extremely Strict Compliance Requirements

❌ Small Internal Utility

---

# Architect Decision Matrix

| Requirement | Shared Schema | Separate Schema | Separate Database |
|------------|---------------|----------------|------------------|
| Lowest Cost | ✅ | ⚠️ | ❌ |
| Easy Operations | ✅ | ⚠️ | ❌ |
| Strong Isolation | ❌ | ✅ | ✅✅ |
| Compliance | ❌ | ✅ | ✅✅ |
| Scalability | ⚠️ | ✅ | ✅✅ |
| Banking | ❌ | ⚠️ | ✅ |
| Government | ⚠️ | ✅ | ✅ |
| SaaS Platform | ✅ | ✅ | ✅ |

---

# Common Mistakes

## Mistake #1

Forgetting Tenant Filters

```sql
SELECT *
FROM Employees
```

Can expose other tenant data.

---

## Mistake #2

Embedding Tenant Logic Everywhere

Use:

```text
Tenant Context Provider
```

instead of manual filtering.

---

## Mistake #3

Choosing Separate Databases Too Early

Start simple.

Scale when required.

---

## Mistake #4

Ignoring Performance Isolation

One tenant should not impact others.

---

# Recommended Evolution Strategy

```text
Phase 1

Shared Database
Shared Schema

↓

Phase 2

Shared Database
Separate Schema

↓

Phase 3

Separate Databases
For Large Tenants
```

This balances cost and scalability.

---

# Key Takeaway

Multi-Tenant Architecture is not just a database design strategy.

It is a platform design strategy.

A good Solution Architect must balance:

- Cost
- Security
- Isolation
- Scalability
- Compliance
- Operational Complexity

For most enterprise and government platforms:

```text
Shared Database

Separate Schema
```

provides the best balance.

For Banking and highly regulated systems:

```text
Separate Database Per Tenant
```

is often the preferred approach.

The goal is to build one platform that can securely serve many organizations while maintaining strong data isolation and operational efficiency.
