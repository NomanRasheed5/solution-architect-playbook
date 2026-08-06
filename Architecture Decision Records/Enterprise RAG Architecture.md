# Distributed Systems Design

Distributed Systems Design is the practice of designing applications where multiple independent components, services, databases, and infrastructure nodes work together to deliver a single business capability.

Instead of building one large application that runs in one place, distributed systems split responsibilities across multiple services, machines, networks, or cloud resources.

Distributed systems are commonly used in:

```text
Microservices Platforms

Banking Systems

Payment Systems

Government Portals

Digital Wallets

Enterprise SaaS

RAG Platforms

IoT Systems

Large-Scale Cloud Applications
```

---

# Core Principle

A distributed system is a system where multiple components communicate over a network.

From the user's perspective:

```text
One Application
```

Internally:

```text
Multiple Services
Multiple Databases
Multiple Queues
Multiple Servers
Multiple Regions
```

---

# Simple Application

```text
User

↓

Web Application

↓

Database
```

This is simple and easy to maintain.

---

# Distributed Application

```text
User

↓

API Gateway

↓

Customer Service

Payment Service

Notification Service

Document Service

Reporting Service

↓

Multiple Databases

↓

Message Broker

↓

External Integrations
```

This provides scalability and flexibility but introduces complexity.

---

# Why Distributed Systems Exist

As systems grow, one application may no longer be enough.

Common reasons:

```text
High User Traffic

Multiple Teams

Independent Deployments

Different Scaling Needs

Global Access

High Availability

Business Domain Separation

Integration With External Systems
```

---

# Real World Example

## Digital Wallet Platform

A wallet platform may include:

```text
Customer Service

Wallet Service

Transaction Service

Fraud Service

Notification Service

Audit Service

Reporting Service
```

Each service has a specific responsibility.

```text
Wallet Service

Handles Balance

----------------

Transaction Service

Handles Payments

----------------

Fraud Service

Detects Risk

----------------

Notification Service

Sends Alerts
```

This is a distributed system.

---

# High Level Architecture

```text
Client Applications

        │

        ▼

API Gateway

        │

 ┌──────┼──────────────────────────────────────┐

 ▼      ▼           ▼            ▼             ▼

User   Payment    Billing      Document     Notification
Svc    Svc        Svc          Svc          Svc

 │      │           │            │             │

 ▼      ▼           ▼            ▼             ▼

DB     DB          DB           Storage       Queue

        │

        ▼

Message Broker

        │

 ┌──────┼───────────┐

 ▼      ▼           ▼

Audit  Reporting   Analytics
Svc    Svc         Svc
```

---

# Key Characteristics

Distributed systems usually have:

```text
Multiple Services

Network Communication

Independent Databases

Message Queues

Asynchronous Processing

Fault Tolerance

Observability

Scalability
```

---

# Business Scenario #1

## Government Citizen Services Platform

Features:

```text
Citizen Registration

Permit Applications

Document Upload

Online Payment

Case Tracking

Notifications
```

Recommended Architecture:

✅ Distributed System

Reason:

```text
Different departments may own different business capabilities.

Some services need independent scaling.

Payments, documents, notifications, and workflows can evolve separately.
```

---

# Business Scenario #2

## Card Management Platform

Features:

```text
Card Issuance

Card Activation

PIN Management

Limit Management

Fraud Monitoring

Transaction Alerts
```

Recommended Architecture:

✅ Distributed System

Reason:

```text
Card lifecycle, fraud rules, notifications, audit, and transaction monitoring are separate domains.
```

---

# Business Scenario #3

## Billing & Collections Platform

Features:

```text
Invoice Generation

Payment Allocation

Refunds

Collections

Aging Reports

Notifications
```

Recommended Architecture:

✅ Distributed System

Reason:

```text
Billing jobs may run in batches.

Collections may follow workflow rules.

Reporting may need optimized read models.

Payments may integrate with external gateways.
```

---

# Business Scenario #4

## Enterprise RAG Platform

Features:

```text
Document Ingestion

Chunking

Embedding Generation

Vector Search

LLM Response Generation

Chat History

Audit Logging
```

Recommended Architecture:

✅ Distributed System

Reason:

```text
Document ingestion, embedding generation, retrieval, LLM calls, and chat APIs have different scaling and processing requirements.
```

---

# Business Scenario #5

## Simple Internal Admin Portal

Features:

```text
Create Users

Manage Roles

View Reports

Basic CRUD
```

Recommended Architecture:

❌ Not Distributed

Better Choice:

```text
Monolith

or

Modular Monolith
```

Reason:

```text
Distributed architecture would introduce unnecessary complexity.
```

---

# Core Challenges In Distributed Systems

Distributed systems solve many problems but introduce new challenges.

```text
Network Failure

Latency

Partial Failure

Data Consistency

Distributed Transactions

Duplicate Messages

Service Discovery

Monitoring

Security

Versioning
```

---

# Challenge 1: Network Failure

In a monolith:

```text
Method Call

↓

Returns Result
```

In distributed systems:

```text
Service A

↓

Network

↓

Service B
```

The network can fail.

Possible issues:

```text
Timeout

Connection Failure

DNS Issue

Firewall Issue

Service Down
```

---

# Design Response

Use:

```text
Timeouts

Retries

Circuit Breakers

Fallbacks

Monitoring
```

---

# Challenge 2: Latency

Local call:

```text
1 ms
```

Network call:

```text
50 ms

100 ms

500 ms
```

Multiple service calls increase response time.

---

# Bad Design

```text
API Gateway

↓

Customer Service

↓

Billing Service

↓

Payment Service

↓

Notification Service
```

Every call waits for the previous one.

---

# Better Design

```text
API Gateway

↓

Parallel Calls

Customer Service
Billing Service
Payment Service
```

or use:

```text
Events

Async Processing
```

---

# Challenge 3: Partial Failure

In distributed systems, one component may fail while others continue.

Example:

```text
Payment Service

Working

Notification Service

Down
```

The system must decide:

```text
Should payment continue?

Should notification retry later?
```

In most cases:

```text
Payment should continue.

Notification should retry asynchronously.
```

---

# Challenge 4: Data Consistency

In monolith:

```text
Single Database Transaction
```

In distributed systems:

```text
Multiple Services

Multiple Databases
```

Traditional transaction is not available.

---

# Example

Order Processing:

```text
Create Order

Deduct Inventory

Process Payment

Send Notification
```

If payment succeeds but inventory fails, the system needs compensation.

---

# Design Response

Use:

```text
Saga Pattern

Eventual Consistency

Outbox Pattern

Idempotency

Compensation Actions
```

---

# Challenge 5: Duplicate Messages

Message brokers can deliver the same message more than once.

Example:

```text
PaymentReceived Event

Delivered Twice
```

Bad result:

```text
Payment Applied Twice
```

---

# Design Response

Make consumers idempotent.

```text
If event already processed

Do not process again
```

Store:

```text
EventId

ProcessedDate

Status
```

---

# Challenge 6: Service Discovery

Services need to find each other.

In small systems:

```text
Hardcoded URL
```

In distributed systems:

```text
Services scale up and down dynamically
```

---

# Design Response

Use:

```text
API Gateway

Service Registry

Container Orchestration

Kubernetes DNS

Azure Container Apps Environment

Azure API Management
```

---

# Challenge 7: Observability

Debugging distributed systems is difficult.

A request may travel through:

```text
API Gateway

↓

Customer Service

↓

Billing Service

↓

Payment Service

↓

Notification Service
```

If something fails, architects must trace the full path.

---

# Design Response

Use:

```text
Centralized Logging

Distributed Tracing

Correlation IDs

Metrics

Alerts

Dashboards
```

---

# Important Design Patterns

Distributed systems usually require several supporting patterns.

---

# 1. API Gateway Pattern

Purpose:

```text
Single entry point for clients
```

Used for:

```text
Routing

Authentication

Rate Limiting

Request Aggregation

Monitoring
```

Architecture:

```text
Client

↓

API Gateway

↓

Services
```

---

# 2. Event Driven Architecture

Purpose:

```text
Decouple services using events
```

Example:

```text
InvoiceGenerated

↓

Service Bus

↓

Notification Service

Reporting Service

Audit Service
```

---

# 3. Saga Pattern

Purpose:

```text
Manage distributed transactions
```

Example:

```text
Create Loan

Reserve Funds

Create Schedule

If Failed

Release Funds

Cancel Loan
```

---

# 4. Circuit Breaker Pattern

Purpose:

```text
Prevent repeated calls to failing services
```

Example:

```text
Payment Service Down

↓

Circuit Opens

↓

Requests Fail Fast

↓

System Recovers Later
```

---

# 5. Retry Pattern

Purpose:

```text
Handle temporary failures
```

Example:

```text
Database Timeout

↓

Retry After Delay

↓

Success
```

Use carefully.

Do not retry non-idempotent operations blindly.

---

# 6. Bulkhead Pattern

Purpose:

```text
Isolate failures
```

Example:

```text
Reporting Service Overloaded

Does Not Affect

Payment Service
```

---

# 7. Outbox Pattern

Purpose:

```text
Ensure database update and event publishing happen reliably
```

Problem:

```text
Save Order

✅

Publish Event

❌
```

Outbox solution:

```text
Save Order

+

Save Event To Outbox Table

↓

Background Worker Publishes Event
```

---

# 8. Idempotency Pattern

Purpose:

```text
Same request can be processed multiple times safely
```

Example:

```text
Payment Request Id = ABC123
```

If request comes twice:

```text
Process Once

Return Same Result
```

---

# Communication Styles

Distributed systems usually use two communication styles.

---

# 1. Synchronous Communication

Service calls another service and waits.

```text
Service A

↓

HTTP/gRPC

↓

Service B
```

Best for:

```text
Immediate Response

Simple Query

Validation
```

Example:

```text
Get Customer Profile
```

---

## Pros

✅ Simple

✅ Easy To Understand

✅ Immediate Response

---

## Cons

❌ Tight Coupling

❌ Latency

❌ Failure Propagation

---

# 2. Asynchronous Communication

Service publishes a message and continues.

```text
Service A

↓

Message Broker

↓

Service B
```

Best for:

```text
Notifications

Reports

Audit Logs

Background Processing

Integration Events
```

Example:

```text
PermitApproved Event

↓

Notification Service Sends Email Later
```

---

## Pros

✅ Loose Coupling

✅ Better Scalability

✅ Better Resilience

---

## Cons

❌ Eventual Consistency

❌ Harder Debugging

❌ Requires Message Broker

---

# Data Management Strategies

Distributed systems often avoid a single shared database.

---

# Database Per Service

Each service owns its own database.

```text
Customer Service

↓

Customer DB

----------------

Billing Service

↓

Billing DB

----------------

Payment Service

↓

Payment DB
```

---

# Advantages

✅ Clear Ownership

✅ Independent Scaling

✅ Better Domain Isolation

---

# Disadvantages

❌ Data Consistency Challenges

❌ More Operational Complexity

❌ Cross-Service Reporting Is Harder

---

# Shared Database

Multiple services use same database.

```text
Customer Service

Billing Service

Payment Service

↓

Shared DB
```

---

# Advantages

✅ Easier Reporting

✅ Simpler Transactions

✅ Lower Complexity

---

# Disadvantages

❌ Tight Coupling

❌ Schema Conflicts

❌ Harder Independent Deployment

---

# Recommendation

For early systems:

```text
Shared Database

or

Modular Monolith
```

For mature microservices:

```text
Database Per Service
```

Do not start with database-per-service unless the organization is ready for operational complexity.

---

# Reliability Principles

A distributed system should assume failure.

---

# Principle 1

## Design For Failure

Every dependency can fail.

```text
Database

Queue

External API

Network

Storage

Identity Provider
```

---

# Principle 2

## Use Timeouts

Never wait forever.

```text
Call External Service

Timeout After Defined Duration
```

---

# Principle 3

## Use Retries Carefully

Retry only when failure is temporary.

Good retry:

```text
Network Timeout
```

Bad retry:

```text
Payment Already Processed
```

---

# Principle 4

## Use Dead Letter Queues

Failed messages should not disappear.

```text
Message Failed

↓

Dead Letter Queue

↓

Manual Investigation
```

---

# Principle 5

## Monitor Everything

Track:

```text
Latency

Failures

Throughput

Retries

Queue Length

CPU

Memory

Database Connections
```

---

# Security In Distributed Systems

Security becomes more complex when services are distributed.

---

# Common Requirements

```text
Authentication

Authorization

Service-To-Service Security

Secrets Management

Network Security

Audit Logging

Data Encryption
```

---

# Recommended Security Architecture

```text
Client

↓

API Gateway

↓

JWT Validation

↓

Backend Services

↓

Managed Identity

↓

Database / Key Vault
```

---

# Best Practices

```text
Use OAuth2 / OIDC

Use JWT For APIs

Use Managed Identity In Azure

Store Secrets In Key Vault

Encrypt Data At Rest

Encrypt Data In Transit

Apply Least Privilege

Centralize Audit Logs
```

---

# Azure Reference Architecture

```text
Users

↓

Azure Front Door

↓

Azure API Management

↓

Azure Container Apps / AKS

├── Customer Service
├── Billing Service
├── Payment Service
├── Notification Service
├── Reporting Service

↓

Azure Service Bus

↓

Azure SQL / Cosmos DB / PostgreSQL

↓

Azure Redis Cache

↓

Application Insights

↓

Azure Key Vault
```

---

# Azure Services Commonly Used

```text
Azure API Management

Azure Service Bus

Azure Event Grid

Azure Container Apps

Azure Kubernetes Service

Azure App Service

Azure SQL

Cosmos DB

Azure Cache for Redis

Application Insights

Key Vault

Azure Front Door
```

---

# When To Use Distributed Systems

Use when:

✅ Large Scale Requirements

✅ Multiple Teams

✅ Independent Deployments

✅ Independent Scaling

✅ Complex Business Domains

✅ High Availability Requirements

✅ Global Applications

✅ Banking Platforms

✅ Digital Wallets

✅ Government Super Apps

✅ Enterprise SaaS Platforms

✅ Event Driven Workflows

---

# When NOT To Use Distributed Systems

Avoid when:

❌ Small Team

❌ Simple CRUD Application

❌ Internal Utility

❌ No High Scale Requirement

❌ No Independent Deployment Need

❌ Limited DevOps Maturity

❌ Limited Monitoring Capability

Examples:

```text
Meeting Management System

Survey Application

Small HR Portal

Simple Admin Dashboard

Basic Reporting Portal
```

A monolith or modular monolith is usually better.

---

# Pros

## Scalability

Scale only the services that need it.

```text
Payment Service

10 Instances

Notification Service

2 Instances
```

---

## Independent Deployment

Teams can release services separately.

---

## Fault Isolation

Failure in one service does not necessarily bring down the system.

---

## Technology Flexibility

Different services can use different technologies if justified.

Example:

```text
Fraud Service

Python

----------------

Payment Service

.NET

----------------

Search Service

ElasticSearch
```

---

## Team Autonomy

Different teams can own different business domains.

---

# Cons

## Complexity

More moving parts.

```text
Services

Databases

Queues

Gateways

Monitoring

Security

Deployments
```

---

## Debugging Difficulty

Requests cross multiple services.

---

## Data Consistency

Strong consistency becomes harder.

---

## Higher Cost

More infrastructure and operational tooling are required.

---

## DevOps Dependency

Requires mature CI/CD, monitoring, deployment automation, and incident response.

---

# Architect Decision Matrix

| Scenario | Distributed System |
|----------|-------------------|
| Simple CRUD App | ❌ |
| Internal Utility | ❌ |
| HR Portal | ⚠️ |
| Permit Management | ⚠️ |
| ERP Platform | ⚠️ |
| Citizen Services Platform | ✅ |
| Card Management Platform | ✅ |
| Billing Platform | ✅ |
| Digital Wallet | ✅ |
| Payment Switch | ✅ |
| Enterprise SaaS | ✅ |
| Enterprise RAG Platform | ✅ |
| IoT Platform | ✅ |

---

# Recommended Evolution Path

Do not start with a complex distributed system unless required.

Recommended path:

```text
Phase 1

Monolith

↓

Phase 2

Modular Monolith

↓

Phase 3

Introduce Async Processing

↓

Phase 4

Extract High-Value Services

↓

Phase 5

Distributed System

↓

Phase 6

Cloud-Native Platform
```

---

# Example Evolution

## Permit Management Platform

Start:

```text
Modular Monolith

Modules:

Applicant

Permit

Inspection

Notification

Reporting
```

Later Extract:

```text
Notification Service

Document Service

Payment Service
```

Final:

```text
Hybrid Distributed Architecture
```

This is safer than starting with full microservices immediately.

---

# Common Mistakes

## Mistake #1

Starting With Microservices Too Early

```text
Small Team

Simple App

10 Services
```

This creates unnecessary complexity.

---

## Mistake #2

Using Synchronous Calls Everywhere

Too many direct calls create fragile systems.

---

## Mistake #3

No Observability

Without logs, tracing, and metrics, production support becomes painful.

---

## Mistake #4

Ignoring Data Consistency

Distributed systems require explicit consistency design.

---

## Mistake #5

Shared Database Across Many Microservices

This reduces service independence.

---

## Mistake #6

No Idempotency

Duplicate requests or messages can corrupt business data.

---

# Practical Design Checklist

Before designing a distributed system, answer:

```text
What are the business domains?

Which services need independent scaling?

Which services need independent deployment?

Where is strong consistency required?

Where is eventual consistency acceptable?

What happens when a service fails?

How will messages be retried?

How will duplicate requests be handled?

How will logs be correlated?

How will secrets be managed?

How will APIs be secured?

How will data be backed up?

How will deployments be automated?
```

---

# Recommended Architecture For Enterprise Systems

For most large enterprise platforms:

```text
API Gateway

+

Modular Services

+

Message Broker

+

Database Per Critical Domain

+

Redis Cache

+

Centralized Logging

+

Distributed Tracing

+

CI/CD Pipelines
```

---

# Recommended Architecture For Azure

```text
Azure Front Door

↓

Azure API Management

↓

Azure Container Apps

↓

Azure Service Bus

↓

Azure SQL / PostgreSQL / Cosmos DB

↓

Azure Redis Cache

↓

Application Insights

↓

Azure Key Vault
```

For very large platforms:

```text
Azure Kubernetes Service

+

Service Mesh

+

Advanced Observability
```

---

# Key Takeaway

Distributed Systems Design is not about splitting an application into many services.

It is about designing systems that can scale, survive failures, evolve independently, and support large business capabilities.

A good Solution Architect does not choose distributed architecture because it is modern.

A good Solution Architect chooses it only when the business needs:

```text
Scale

Reliability

Team Independence

Domain Separation

High Availability

Integration Flexibility
```

If these needs do not exist, a modular monolith is usually the better choice.

Distributed systems are powerful, but they must be designed with discipline, observability, security, and failure handling from day one.
