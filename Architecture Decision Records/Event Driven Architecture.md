# Event Driven Architecture (EDA)

Event Driven Architecture (EDA) is an architectural pattern where systems communicate by publishing and consuming events rather than making direct synchronous calls.

Instead of one service calling another service directly, a service publishes an event describing something that happened, and other interested services react to that event.

EDA is commonly used in Banking, FinTech, Government Platforms, E-Commerce, IoT, Enterprise Integration, and Cloud-Native Systems.

---

# Core Principle

Traditional applications work like this:

```text
Service A

↓

Calls

↓

Service B

↓

Calls

↓

Service C

↓

Calls

↓

Service D
```

This creates tight coupling between components.

Event Driven Architecture changes the communication model.

```text
Service A

↓

Publishes Event

↓

Event Bus

↓

Service B
Service C
Service D
```

Services become independent.

---

# What Is An Event?

An event is a record of something that already happened.

Examples:

```text
CustomerCreated

PermitApproved

InvoiceGenerated

PaymentReceived

CardIssued

CardBlocked

UserRegistered

DocumentUploaded
```

Events describe facts.

Events do not tell other systems what to do.

---

# Real World Example

Imagine a Card Management Platform.

Traditional Design:

```text
Card Service

↓

Call Notification Service

↓

Call Fraud Service

↓

Call CRM Service

↓

Call Audit Service
```

Problems:

```text
Strong Coupling

Complex Dependencies

Hard To Scale

Hard To Maintain
```

---

# Event Driven Design

Instead:

```text
Card Service

↓

Publishes

CardIssuedEvent

↓

Event Bus

↓

Notification Service

Fraud Service

CRM Service

Audit Service
```

Card Service has no knowledge of downstream systems.

---

# Why Event Driven Architecture Exists

Large enterprise applications commonly face:

```text
Multiple Integrations

Growing Business Processes

Independent Teams

High Transaction Volume
```

As systems grow, direct service-to-service communication becomes difficult to manage.

EDA reduces dependencies and improves flexibility.

---

# Traditional Integration Problem

Example:

Citizen Service Portal

```text
Citizen Service

↓

Notification Service

↓

SMS Service

↓

Email Service

↓

Audit Service

↓

Reporting Service
```

Adding a new consumer requires modifying existing systems.

---

# Event Driven Solution

```text
CitizenCreated

↓

Event Bus

↓

Notification Service

Audit Service

Reporting Service

Analytics Service

Search Service
```

New consumers can subscribe without modifying the source system.

---

# High Level Architecture

```text
Client

  │

  ▼

Business Service

  │

  ▼

Event Bus

  │

 ┌──────────────┬──────────────┬──────────────┐

 ▼              ▼              ▼              ▼

Notification   Audit        Reporting      Analytics
Service        Service      Service        Service
```

---

# Event Flow Example

## Permit Approval Process

```text
Engineer Approves Permit

↓

Permit Service

↓

PermitApproved Event

↓

Azure Service Bus

↓

Notification Service

↓

Send Email

↓

Audit Service

↓

Store Audit Log

↓

Reporting Service

↓

Update Dashboard
```

No direct dependencies exist.

---

# Components Of Event Driven Architecture

## Producer

Creates and publishes events.

Example:

```text
Customer Service

Card Service

Billing Service

Permit Service
```

---

## Event Bus

Responsible for transporting events.

Examples:

```text
Azure Service Bus

Kafka

RabbitMQ

AWS SNS/SQS

Azure Event Grid
```

---

## Consumer

Receives and processes events.

Examples:

```text
Notification Service

Fraud Service

Audit Service

Analytics Service
```

---

# Business Scenario #1

## Card Management Platform

System Actions:

```text
Issue Card

Activate Card

Change Limits

Block Card
```

When card is issued:

```text
CardIssued Event
```

Subscribers:

```text
Notification Service

Audit Service

Fraud Service

CRM Service
```

Recommended:

✅ Event Driven Architecture

---

# Business Scenario #2

## Digital Wallet

Transaction:

```text
Wallet Transfer
```

Event:

```text
FundsTransferred Event
```

Consumers:

```text
Notification

Ledger

Analytics

Compliance Monitoring

Fraud Detection
```

Recommended:

✅ Event Driven Architecture

---

# Business Scenario #3

## Government Citizen Portal

When a citizen submits an application:

```text
ApplicationSubmitted
```

Consumers:

```text
Workflow Engine

Notification Service

Analytics Platform

Audit Service
```

Recommended:

✅ Event Driven Architecture

---

# Business Scenario #4

## Billing & Collections

When invoice is generated:

```text
InvoiceGenerated
```

Consumers:

```text
Email Service

Customer Portal

Reporting Platform

Collections Service
```

Recommended:

✅ Event Driven Architecture

---

# Business Scenario #5

## E-Commerce Platform

Order Created

```text
OrderPlaced
```

Consumers:

```text
Inventory Service

Payment Service

Delivery Service

Notification Service

Analytics Service
```

Recommended:

✅ Event Driven Architecture

---

# Azure Event Driven Architecture

Typical Azure Design:

```text
Application

↓

Azure App Service

↓

Publish Event

↓

Azure Service Bus

↓

Azure Functions

↓

Consumer Services

↓

Azure SQL

Cosmos DB

Notifications
```

---

# Event Driven vs Synchronous Communication

## Synchronous

```text
Customer Service

↓

Calls Billing Service

↓

Wait

↓

Calls Notification Service

↓

Wait

↓

Response
```

Problems:

```text
Slow

Tightly Coupled

Failure Propagation
```

---

## Event Driven

```text
Customer Service

↓

Publish Event

↓

Return Success

↓

Consumers Process Separately
```

Benefits:

```text
Fast

Scalable

Resilient
```

---

# Advantages

## Loose Coupling

Producer knows nothing about consumers.

```text
Card Service

↓

CardIssued Event
```

That's all.

---

## Scalability

Consumers can scale independently.

```text
Notifications
2 Instances

Analytics
8 Instances

Audit
1 Instance
```

---

## Extensibility

Adding subscribers requires no producer changes.

Example:

```text
Add Search Service

↓

Subscribe To Event
```

Done.

---

## Resilience

Temporary consumer failures do not affect producers.

```text
Notification Service Down

↓

Card Issuance Continues
```

---

## Better Team Independence

Different teams own different consumers.

```text
Payments Team

Notifications Team

Analytics Team
```

No direct dependency.

---

# Disadvantages

## Eventual Consistency

Data updates may not be immediately visible.

Example:

```text
Customer Updated

↓

Event Published

↓

Reporting Updated 5 Seconds Later
```

Systems become eventually consistent.

---

## More Infrastructure

Requires:

```text
Service Bus

Message Broker

Dead Letter Queues

Monitoring

Tracing
```

---

## Harder Debugging

Request processing becomes distributed.

```text
Producer

↓

Event Bus

↓

Consumer A

↓

Consumer B

↓

Consumer C
```

Tracing tools become essential.

---

## Duplicate Processing

Consumers must be idempotent.

Possible scenario:

```text
Same Event

Processed Twice
```

Must not create duplicate records.

---

# Event Design Best Practices

## Events Describe Facts

Good:

```text
InvoiceGenerated

CardIssued

CustomerCreated
```

Bad:

```text
SendEmail

GenerateReport

CreateAuditRecord
```

Events should describe what happened.

---

## Include Required Information

Example:

```json
{
  "EventId": "123",
  "CardId": "ABC123",
  "CustomerId": "567",
  "CreatedDate": "2026-08-06"
}
```

Consumers should receive enough information.

---

## Version Events

Example:

```text
CardIssuedV1

CardIssuedV2
```

Allows safe evolution.

---

# Event Driven + CQRS

Common enterprise combination.

```text
Command

↓

Update Permit

↓

PermitApproved Event

↓

Read Model Updated

↓

Dashboard Updated
```

Used extensively in:

```text
ERP

Government Platforms

Financial Systems

Digital Wallets
```

---

# Event Driven + Microservices

Very common.

```text
Customer Service

↓

CustomerRegistered

↓

Event Bus

↓

Billing

Notification

CRM

Analytics
```

Services communicate through events rather than direct calls.

---

# Event Driven + Saga Pattern

Example:

Loan Processing

```text
LoanCreated

↓

ReserveFunds

↓

RepaymentScheduleCreated

↓

NotificationSent
```

Each step publishes events.

Failures trigger compensating events.

---

# When To Use Event Driven Architecture

Use EDA when:

✅ Multiple Systems Need The Same Information

✅ Large Enterprise Platforms

✅ High Transaction Volume

✅ Independent Teams

✅ Cloud-Native Systems

✅ Banking Applications

✅ Digital Wallets

✅ Government Platforms

✅ IoT Platforms

✅ Enterprise Integrations

✅ Workflow Systems

---

# When NOT To Use Event Driven Architecture

Avoid EDA when:

❌ Simple CRUD Applications

❌ Small Internal Systems

❌ Small Team Applications

❌ No Integration Requirements

❌ Limited Business Complexity

Examples:

```text
Meeting Management

Survey Portal

Leave Application System

Small Inventory System
```

may not justify EDA.

---

# Migration Strategy

Recommended approach:

```text
Phase 1

Traditional CRUD

↓

Phase 2

Introduce Domain Events

↓

Phase 3

Add Service Bus

↓

Phase 4

Move Non-Critical Functions

↓

Phase 5

Expand Event Ecosystem
```

Do not make everything event-driven on Day 1.

---

# Architect Decision Matrix

| Requirement | Event Driven Architecture |
|------------|---------------------------|
| Simple CRUD System | ❌ |
| Internal Workflow Portal | ⚠️ |
| ERP Platform | ✅ |
| Citizen Services Platform | ✅ |
| Permit Management System | ✅ |
| Card Management Platform | ✅ |
| Billing Platform | ✅ |
| Digital Wallet | ✅ |
| Banking Platform | ✅ |
| E-Commerce Platform | ✅ |
| IoT Platform | ✅ |

---

# Real World Enterprise Example

Imagine a government permit system.

Without EDA:

```text
Permit Service

↓

Call Notification

↓

Call Reporting

↓

Call Audit

↓

Call Analytics
```

With EDA:

```text
PermitApproved Event

↓

Service Bus

↓

Notification

Audit

Reporting

Analytics

Search
```

Future systems simply subscribe to the event.

No code changes required in Permit Service.

---

# Key Takeaway

Event Driven Architecture is not about messaging technology.

It is about reducing coupling and enabling systems to evolve independently.

A good Solution Architect adopts Event Driven Architecture when:

- Business domains are growing
- Integrations are increasing
- Teams need autonomy
- Scalability requirements increase

Architecture should allow systems to communicate without becoming dependent on each other's implementation details.

Build services around business events, not service calls.
