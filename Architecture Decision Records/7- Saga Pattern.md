# Saga Pattern

The Saga Pattern is a distributed transaction management pattern used in Microservices and Event-Driven Architectures where a single business process spans multiple services and databases.

Traditional database transactions work well within a single database. However, in distributed systems, a business operation often involves multiple services, each owning its own database.

The Saga Pattern provides a mechanism to maintain business consistency without relying on distributed database transactions.

---

# Core Principle

Traditional systems use:

```text
BEGIN TRANSACTION

Step 1
Step 2
Step 3

COMMIT
```

If any step fails:

```text
ROLLBACK
```

Everything returns to its original state.

This works when:

```text
Single Application

Single Database
```

---

# The Problem in Microservices

Consider a Digital Wallet Platform.

Business Flow:

```text
Create Transfer

↓

Deduct Balance

↓

Create Transaction

↓

Send Notification
```

These actions occur in different services.

```text
Wallet Service

Transaction Service

Notification Service
```

Each owns its own database.

There is no shared transaction.

---

# Business Challenge

Suppose:

```text
Balance Deducted

✅ Success

Transaction Created

✅ Success

Notification Sent

❌ Failed
```

Should the entire transfer fail?

Or continue?

What if:

```text
Loan Created

✅

Funds Reserved

✅

Repayment Schedule

❌ Failed
```

How do we undo previous steps?

This is exactly what Saga solves.

---

# What is a Saga?

A Saga is a sequence of local transactions.

Each service performs its own transaction.

When successful:

```text
Continue To Next Step
```

When failure occurs:

```text
Execute Compensation
```

Compensation reverses previously completed actions.

---

# Traditional Transaction

```text
Step 1

↓

Step 2

↓

Step 3

↓

Commit
```

Failure:

```text
Rollback Everything
```

---

# Saga Transaction

```text
Step 1

↓

Step 2

↓

Step 3

↓

Failure

↓

Compensation Step 2

↓

Compensation Step 1
```

Rollback becomes business-driven instead of database-driven.

---

# Everyday Example

Online Flight Booking.

```text
Reserve Flight

Reserve Hotel

Reserve Car
```

Suppose hotel reservation fails.

Without compensation:

```text
Flight Reserved

Hotel Failed
```

Customer is stuck.

Saga

```text
Hotel Failed

↓

Cancel Flight Reservation
```

System returns to consistent state.

---

# High Level Architecture

```text
Order Service

↓

Payment Service

↓

Inventory Service

↓

Shipping Service

↓

Notification Service
```

Each service owns:

```text
Own Database

Own Transaction
```

No distributed transaction exists.

---

# Saga Flow Example

## Card Issuance

```text
Step 1

Create Customer

✅

↓

Step 2

Issue Card

✅

↓

Step 3

Configure Limits

❌ Failed
```

Compensation:

```text
Delete Card

↓

Delete Customer
```

System consistency restored.

---

# Components

## Local Transaction

Each service transaction.

Example:

```text
Issue Card

Update Wallet

Create Invoice
```

Executed independently.

---

## Event

Signals success.

Example:

```text
CardIssued

WalletCreated

InvoiceGenerated
```

Triggers next action.

---

## Compensation Action

Undo operation.

Example:

```text
Issue Card

↓

Compensating Action

↓

Cancel Card
```

Every saga step should have a compensation.

---

# Business Scenario #1

## Loan Application

Business Process:

```text
Create Loan

Reserve Funds

Generate Repayment Schedule

Send Notification
```

---

Failure Example

```text
Create Loan

✅

Reserve Funds

✅

Generate Schedule

❌
```

Compensation:

```text
Release Funds

Cancel Loan
```

Recommended:

✅ Saga Pattern

---

# Business Scenario #2

## Digital Wallet Transfer

Process:

```text
Create Transfer

Deduct Balance

Credit Beneficiary

Record Ledger
```

Failure:

```text
Ledger Service Failed
```

Compensation:

```text
Reverse Credit

Refund Sender
```

Recommended:

✅ Saga Pattern

---

# Business Scenario #3

## Card Management Platform

Process:

```text
Create Customer

Issue Card

Generate PIN

Send Welcome Notification
```

If PIN generation fails:

```text
Cancel Card

Delete Customer
```

Recommended:

✅ Saga Pattern

---

# Business Scenario #4

## Citizen Services Portal

Process:

```text
Submit Application

Validate Documents

Collect Payment

Create Permit
```

If permit creation fails:

```text
Refund Payment

Close Case
```

Recommended:

✅ Saga Pattern

---

# Business Scenario #5

## Billing & Collections

Process:

```text
Generate Invoice

Apply Charges

Update Balance

Notify Customer
```

If balance update fails:

```text
Cancel Invoice

Reverse Charges
```

Recommended:

✅ Saga Pattern

---

# Two Saga Styles

There are two popular implementation approaches.

```text
Choreography

Orchestration
```

---

# 1. Choreography Saga

Services communicate through events.

No central coordinator exists.

---

## Flow

```text
Order Created

↓

Payment Service

↓

Payment Completed

↓

Inventory Service

↓

Inventory Reserved

↓

Shipping Service
```

Everything happens through events.

---

## Architecture

```text
Service A

↓

Event

↓

Service B

↓

Event

↓

Service C
```

---

### Advantages

```text
Simple

Loosely Coupled

No Central Controller
```

---

### Disadvantages

```text
Hard To Understand

Hard To Debug

Complex Event Chains
```

Large systems become difficult to manage.

---

# Choreography Example

## Invoice Payment

```text
InvoiceCreated

↓

Payment Service

↓

PaymentReceived

↓

Accounting Service

↓

LedgerUpdated

↓

Notification Service
```

No orchestrator.

Services simply react.

---

# 2. Orchestration Saga

A central Saga Orchestrator controls the workflow.

---

## Flow

```text
Orchestrator

↓

Call Payment Service

↓

Success

↓

Call Inventory Service

↓

Success

↓

Call Shipping Service
```

---

## Architecture

```text
Saga Orchestrator

├── Payment Service
├── Inventory Service
├── Shipping Service
└── Notification Service
```

---

### Advantages

```text
Easier Monitoring

Easier Debugging

Centralized Control
```

---

### Disadvantages

```text
Extra Component

Potential Bottleneck
```

---

# Recommended Approach

## Small Workflow

```text
Few Services

Simple Flow
```

Use:

✅ Choreography

---

## Complex Enterprise Workflow

```text
Many Services

Compensation Logic

Business Approvals
```

Use:

✅ Orchestration

---

# Saga vs Traditional Transaction

| Feature | DB Transaction | Saga |
|----------|----------------|-------|
| Single Database | ✅ | ❌ |
| Multiple Services | ❌ | ✅ |
| Distributed Systems | ❌ | ✅ |
| Rollback | Database Rollback | Compensation |
| Complexity | Low | Medium-High |
| Scalability | Limited | Excellent |

---

# Azure Architecture Example

```text
API Gateway

↓

Loan Service

↓

Azure Service Bus

↓

Saga Orchestrator

↓

Payment Service

↓

Notification Service

↓

Audit Service
```

---

# Azure Services

For Saga implementations:

```text
Azure Service Bus

Azure Functions

Azure Durable Functions

Azure Logic Apps

Azure Container Apps

AKS
```

---

# Saga + Event Driven Architecture

They are frequently used together.

Example:

```text
LoanCreated

↓

ReserveFunds

↓

FundsReserved

↓

CreateSchedule

↓

ScheduleCreated

↓

SendNotification
```

Events drive the process.

---

# Saga + CQRS

Common enterprise combination:

```text
Command

↓

StartLoanApplication

↓

Saga

↓

Multiple Services

↓

Events

↓

Read Model Updated
```

Very common in:

```text
Banking

Government Platforms

ERP

Digital Wallets
```

---

# Advantages

## Handles Distributed Transactions

Primary reason Saga exists.

---

## Supports Microservices

Designed specifically for distributed systems.

---

## High Scalability

No distributed database transactions required.

---

## Better Fault Recovery

Compensation logic improves resilience.

---

## Business Process Alignment

Maps naturally to business workflows.

---

# Disadvantages

## More Complexity

Must design:

```text
Events

Compensations

State Tracking
```

---

## Eventual Consistency

Data may not be immediately synchronized.

---

## Compensation Design

Undo operations are not always obvious.

Example:

```text
Refund Payment

Cancel Permit

Reverse Transaction
```

Business rules become important.

---

## Monitoring Requirements

Sagas need tracking.

Questions include:

```text
Which Step Failed?

Current Status?

Compensation Executed?
```

---

# When To Use Saga Pattern

Use when:

✅ Multiple Services Involved

✅ Multiple Databases

✅ Distributed Transactions

✅ Event Driven Architecture

✅ Financial Platforms

✅ Banking Systems

✅ Card Platforms

✅ Digital Wallets

✅ Loan Management

✅ Enterprise Workflow Systems

---

# When NOT To Use Saga

Avoid when:

❌ Single Application

❌ Single Database

❌ Simple CRUD System

❌ Internal Utility

❌ Basic Workflow

---

# Real World Examples

## Excellent Candidates

### Banking

```text
Payments

Transfers

Cards

Loans
```

---

### FinTech

```text
Wallets

Settlements

Billing
```

---

### Government

```text
Permits

Applications

Payments

Approvals
```

---

### E-Commerce

```text
Orders

Inventory

Shipping

Payments
```

---

# Common Mistakes

## Mistake #1

Using Saga in a Monolith.

If everything uses one database:

```text
Use TransactionScope
```

Don't over-engineer.

---

## Mistake #2

Not Designing Compensation

Every step should answer:

```text
How Do We Undo This?
```

---

## Mistake #3

Making Compensation Impossible

Bad example:

```text
Send Physical Package
```

Not easily reversible.

Do not rely on compensation after irreversible actions.

---

## Mistake #4

Combining Too Many Patterns Early

Avoid:

```text
Microservices

+

CQRS

+

Event Sourcing

+

Saga

Day 1
```

Start simple.

---

# Architect Decision Matrix

| Scenario | Saga Pattern |
|-----------|-------------|
| HR Portal | ❌ |
| Survey System | ❌ |
| Meeting Management | ❌ |
| Permit Management + Payments | ✅ |
| Citizen Services Platform | ✅ |
| Billing Platform | ✅ |
| Card Management Platform | ✅ |
| Digital Wallet | ✅ |
| Loan Processing | ✅ |
| Banking Platform | ✅ |

---

# Evolution Strategy

Recommended growth path:

```text
Monolith

↓

Modular Monolith

↓

Microservices

↓

Event Driven Architecture

↓

Saga Pattern
```

Do not introduce Saga until a real distributed transaction problem exists.

---

# Key Takeaway

Saga Pattern is the distributed alternative to database transactions.

Instead of:

```text
Rollback Transaction
```

Saga uses:

```text
Compensation Actions
```

A good Solution Architect introduces Saga only when business workflows span multiple services and databases.

If your system can be solved using a single transaction, use a single transaction.

If your system requires multiple services working together, Saga becomes one of the most important patterns for maintaining business consistency while preserving scalability and service independence.
