# Authentication Patterns

Authentication is the process of verifying the identity of a user, application, service, or device before allowing access to a system.

One of the most important responsibilities of a Solution Architect is selecting the right authentication model based on the business requirements, user population, security requirements, scalability needs, and system architecture.

Choosing the wrong authentication mechanism can introduce security vulnerabilities, scalability issues, poor user experience, and operational complexity.

---

# Core Principle

Authentication answers:

```text
Who are you?
```

Authorization answers:

```text
What are you allowed to do?
```

Authentication always happens first.

```text
User

↓

Authentication

↓

Authorization

↓

Application Access
```

---

# Authentication Decision Matrix

| Scenario | Recommended Pattern |
|-----------|-------------------|
| Traditional MVC Application | Session Authentication |
| Internal Portal | Session Authentication |
| Mobile Application | JWT |
| SPA (React/Angular) | JWT + OAuth2 |
| Microservices | JWT |
| Enterprise SSO | OAuth2 + OpenID Connect |
| Government Portal | OAuth2 + OIDC |
| Citizen Portal | Azure AD B2C |
| Banking Mobile App | OAuth2 + JWT |
| Service-to-Service Communication | Client Credentials Flow |

---

# Evolution of Authentication

Authentication patterns evolved over time.

```text
Basic Authentication

↓

Session Authentication

↓

JWT Authentication

↓

OAuth2

↓

OpenID Connect

↓

Federated Identity

↓

Zero Trust Authentication
```

---

# 1. Session-Based Authentication

The most common authentication pattern for traditional web applications.

---

## How It Works

```text
User Login

↓

Validate Credentials

↓

Create Session

↓

Store Session

↓

Return Session Cookie

↓

Subsequent Requests Use Cookie
```

---

## Architecture

```text
Browser

↓

MVC Application

↓

Session Store

↓

SQL Server
```

---

## Example

ASP.NET MVC

```text
Forms Authentication

Cookie Authentication

Session State
```

---

## Advantages

✅ Easy Implementation

✅ Mature Technology

✅ Easy Logout

✅ Suitable For Traditional Web Apps

✅ Good Internal Applications

---

## Disadvantages

❌ Server Memory Usage

❌ Stateful

❌ Difficult Horizontal Scaling

❌ Not Ideal For Mobile Apps

❌ Not Suitable For APIs

---

## Best For

```text
Employee Portal

HR Systems

Meeting Management

Internal Workflow Systems

Government Internal Applications
```

---

# Business Scenario

Employee Self Service Platform

```text
Employees

↓

Corporate Network

↓

Web Portal

↓

Cookie Authentication
```

Recommended:

✅ Session Authentication

---

# 2. JWT Authentication

JWT (JSON Web Token) is a stateless authentication mechanism commonly used for APIs and modern applications.

---

# How JWT Works

```text
User Login

↓

Validate Credentials

↓

Generate JWT

↓

Return Token

↓

Client Stores Token

↓

Token Sent With Every Request
```

---

# Architecture

```text
Client

↓

API

↓

JWT Validation

↓

Authorized Access
```

---

# JWT Structure

```text
Header

Payload

Signature
```

Example:

```json
{
  "sub": "123",
  "name": "Noman",
  "role": "Admin"
}
```

---

# Request Flow

```text
Login

↓

JWT Issued

↓

Store Token

↓

Send Token

↓

Validate Token

↓

Process Request
```

---

# Advantages

✅ Stateless

✅ Excellent For APIs

✅ Easy Horizontal Scaling

✅ Mobile Friendly

✅ Supports Microservices

---

# Disadvantages

❌ Harder Logout

❌ Token Expiration Challenges

❌ Revocation Complexity

❌ Token Theft Risk

---

# Best For

```text
REST APIs

React Applications

Angular Applications

Mobile Apps

Microservices
```

---

# Business Scenario

Permit Management API

```text
React SPA

↓

JWT

↓

.NET API

↓

SQL Server
```

Recommended:

✅ JWT Authentication

---

# 3. OAuth 2.0

OAuth2 is an authorization framework that allows applications to access resources on behalf of users.

OAuth2 does not define authentication itself.

It defines delegated access.

---

# Why OAuth Exists

Without OAuth:

```text
User Gives Password

↓

Application Stores Password
```

Security risk.

OAuth:

```text
User

↓

Identity Provider

↓

Token

↓

Application
```

Application never sees credentials.

---

# Typical Flow

```text
User

↓

Identity Provider

↓

Access Token

↓

Application
```

---

# Popular Identity Providers

```text
Azure AD

Google

Facebook

GitHub

Okta

Auth0
```

---

# Advantages

✅ Secure

✅ Enterprise Ready

✅ Industry Standard

✅ Delegated Access

✅ Supports SSO

---

# Disadvantages

❌ More Complex

❌ Learning Curve

❌ Additional Configuration

---

# Best For

```text
Enterprise Applications

Government Platforms

B2B Integrations

Mobile Applications
```

---

# Business Scenario

Government Employee Portal

```text
Employee

↓

Azure AD

↓

OAuth2 Token

↓

Portal
```

Recommended:

✅ OAuth2

---

# 4. OpenID Connect (OIDC)

OIDC extends OAuth2 by adding identity capabilities.

Authentication + Authorization

---

# OAuth2 vs OIDC

OAuth:

```text
What Can User Access?
```

OIDC:

```text
Who Is User?
```

---

# Architecture

```text
User

↓

Identity Provider

↓

ID Token

↓

Application

↓

Access Granted
```

---

# Additional Token

```text
ID Token
```

Contains:

```text
Name

Email

Role

Tenant

Claims
```

---

# Advantages

✅ Single Sign-On

✅ Modern Web Authentication

✅ Enterprise Standard

✅ Better User Experience

---

# Best For

```text
Enterprise Portals

Government Platforms

Cloud Applications

Internal Systems
```

---

# Business Scenario

Corporate Portal

```text
Employee

↓

Azure AD

↓

OIDC

↓

Portal
```

Recommended:

✅ OIDC

---

# 5. Single Sign-On (SSO)

SSO allows users to authenticate once and access multiple applications.

---

# Architecture

```text
Identity Provider

↓

User Login

↓

Access Multiple Applications
```

---

# Example

```text
Email

↓

Intranet

↓

HR Portal

↓

Finance System

↓

Meeting Portal
```

Single login.

---

# Advantages

✅ Better User Experience

✅ Fewer Passwords

✅ Central Security

✅ Easier User Management

---

# Disadvantages

❌ Identity Provider Dependency

❌ More Complex Setup

---

# Best For

```text
Government Departments

Enterprise Organizations

Universities

Large Corporations
```

---

# 6. Azure Active Directory Authentication

One of the most common enterprise solutions.

---

# Architecture

```text
User

↓

Azure AD

↓

Token

↓

Application
```

---

# Features

```text
MFA

Conditional Access

SSO

Device Policies

Identity Governance
```

---

# Best For

```text
Microsoft Ecosystem

Government

Enterprise

Internal Applications
```

---

# Business Scenario

Ashghal Employee Portal

```text
Employee

↓

Azure AD

↓

Single Sign-On

↓

Applications
```

Recommended:

✅ Azure AD Authentication

---

# 7. Azure AD B2C

Identity platform for external users.

---

# Typical Users

```text
Citizens

Customers

Contractors

Partners
```

---

# Architecture

```text
Citizen

↓

Azure AD B2C

↓

Application
```

---

# Features

```text
Social Login

Self Registration

MFA

Password Reset

External Users
```

---

# Best For

```text
Citizen Portals

Public Services

Customer Platforms

External Applications
```

---

# 8. Service-to-Service Authentication

Used in microservices.

No human users involved.

---

# Architecture

```text
Service A

↓

Service Token

↓

Service B
```

---

# OAuth2 Client Credentials Flow

```text
Service A

↓

Identity Provider

↓

Access Token

↓

Service B
```

---

# Best For

```text
Microservices

Background Jobs

Integration Services

API Communication
```

---

# Multi-Factor Authentication (MFA)

Additional security layer.

---

# Flow

```text
Username

↓

Password

↓

OTP / Authenticator App

↓

Access Granted
```

---

# Recommended For

✅ Banking

✅ Financial Services

✅ Government Systems

✅ Privileged Users

✅ Production Systems

---

# Authentication For Different Architectures

---

## Monolith

```text
Session Authentication

Cookie Authentication
```

Preferred:

✅ Session

---

## Modular Monolith

```text
Cookie Authentication

OIDC

Azure AD
```

Preferred:

✅ OIDC + Azure AD

---

## Microservices

```text
JWT

OAuth2

OpenID Connect
```

Preferred:

✅ OAuth2 + JWT

---

## Mobile Applications

```text
OAuth2

JWT

Refresh Tokens
```

Preferred:

✅ OAuth2

---

## Government Platforms

```text
Azure AD

OIDC

MFA
```

Preferred:

✅ Azure AD + OIDC

---

## Citizen Portals

```text
Azure AD B2C

OIDC

MFA
```

Preferred:

✅ Azure AD B2C

---

# Security Best Practices

## Never Store Passwords

Use:

```text
BCrypt

PBKDF2

Argon2
```

---

## Always Use HTTPS

```text
TLS 1.2+

TLS 1.3
```

---

## Enable MFA

Especially for:

```text
Administrators

Finance Users

Government Users
```

---

## Short-Lived Tokens

Example:

```text
Access Token

15 Minutes
```

Refresh Token:

```text
7 Days
```

---

## Use Claims-Based Authorization

Example:

```json
{
  "Role": "Admin",
  "Department": "Finance",
  "Tenant": "Ashghal"
}
```

---

# Common Mistakes

## Mistake #1

Using Session Authentication for APIs

Bad choice.

Use JWT instead.

---

## Mistake #2

Building Custom Authentication

Avoid unless absolutely necessary.

Use:

```text
Azure AD

Azure AD B2C

Identity Server

Auth0
```

---

## Mistake #3

Long-Lived Tokens

Creates security risks.

---

## Mistake #4

No MFA

Unsafe for enterprise systems.

---

## Mistake #5

Storing Sensitive Data Inside JWT

JWT payload is readable.

Never store:

```text
Passwords

Secrets

Credit Card Data
```

---

# Architect Decision Guide

| Scenario | Recommended Pattern |
|-----------|-------------------|
| MVC Application | Session Authentication |
| Internal Portal | Azure AD + OIDC |
| React SPA | JWT + OAuth2 |
| Mobile App | OAuth2 + JWT |
| Microservices | OAuth2 + JWT |
| Government Platform | Azure AD + MFA |
| Citizen Portal | Azure AD B2C |
| Banking Application | OAuth2 + MFA |
| Service-to-Service | Client Credentials Flow |
| Enterprise SaaS | OAuth2 + OIDC |

---

# Recommended Modern Enterprise Stack

For most enterprise systems:

```text
Azure AD

↓

OpenID Connect

↓

OAuth2

↓

JWT Tokens

↓

API Gateway

↓

Microservices / APIs
```

This architecture provides:

✅ Single Sign-On

✅ Scalability

✅ Security

✅ MFA Support

✅ Cloud Compatibility

✅ Modern Web & Mobile Support

---

# Key Takeaway

Authentication is not a technical implementation detail.

It is a foundational security architecture decision.

A good Solution Architect chooses authentication based on:

- User Type
- Security 
