---
name: privacy-compliance
description: Privacy and compliance framework for GDPR, data protection, and consent management. Use when handling personal data, designing consent flows, or ensuring regulatory compliance.
version: 1.0.0
source: custom
rating: 10/10
---

# Privacy & Compliance Framework

Comprehensive privacy compliance covering GDPR, data protection, and consent management.

## When to Use This Skill

- Handling personal/user data
- Designing consent and preference systems
- Implementing data retention policies
- Responding to data subject requests
- Pre-launch compliance review

---

## Core Principles

> **Privacy by Design, not Privacy as Afterthought**

```
1. Proactive, not reactive
2. Privacy as the default
3. Privacy embedded into design
4. Respect for user privacy
5. End-to-end security
```

---

## Data Classification

```markdown
## Data Classification Matrix

| Category | Examples | Protection Level | Retention |
|----------|----------|-----------------|-----------|
| **PII** | Name, email, phone | High | Minimize |
| **Sensitive PII** | SSN, health, finance | Critical | Encrypt + minimize |
| **Behavioral** | Clicks, views, preferences | Medium | Anonymize after use |
| **Technical** | IP, device ID, logs | Low-Medium | Auto-delete |
| **Anonymous** | Aggregated stats | Low | Retain |
```

### Personal Data Inventory

```markdown
## Personal Data Inventory: [Product Name]

| Data Field | Category | Legal Basis | Retention | Encrypted? | Deletion Process |
|------------|----------|-------------|-----------|------------|------------------|
| Email | PII | Consent | Until delete request | Yes | Soft delete → Hard delete after 30d |
| Name | PII | Contract | Until delete request | Yes | Same as email |
| IP Address | Technical | Legitimate interest | 90 days | No | Auto-purge |
| Payment info | Sensitive PII | Contract | 7 years (legal) | Yes (PCI) | After legal period |
| Usage logs | Behavioral | Legitimate interest | 1 year | No | Anonymize after 30d |
```

---

## GDPR Compliance Checklist

### Legal Basis for Processing

| Basis | When to Use | Requirements |
|-------|-------------|--------------|
| **Consent** | Marketing, tracking | Freely given, specific, informed, unambiguous |
| **Contract** | Service delivery | Necessary for contract performance |
| **Legal Obligation** | Tax records, compliance | Required by law |
| **Legitimate Interest** | Analytics, security | Documented LIA, user can object |
| **Vital Interest** | Emergency health | Life-threatening situations only |

### Consent Requirements

```javascript
// ❌ INVALID - Pre-checked box
<input type="checkbox" checked id="marketing">
<label>Send me marketing emails</label>

// ✅ VALID - Explicit action required
<input type="checkbox" id="marketing">
<label>I want to receive marketing emails about [specific topics]</label>

// ❌ INVALID - Bundled consent
"By signing up you agree to our Terms and receive marketing emails"

// ✅ VALID - Separate consents
"I agree to Terms of Service" [checkbox]
"I want to receive marketing emails (optional)" [checkbox]
```

### Cookie Consent Implementation

```html
<!-- Cookie Banner Template -->
<div id="cookie-banner" class="cookie-banner">
  <h3>We use cookies</h3>
  <p>We use cookies to improve your experience. You can customize your preferences.</p>
  
  <div class="cookie-options">
    <label>
      <input type="checkbox" checked disabled> 
      Essential (required)
    </label>
    <label>
      <input type="checkbox" id="analytics"> 
      Analytics
    </label>
    <label>
      <input type="checkbox" id="marketing"> 
      Marketing
    </label>
  </div>
  
  <button onclick="acceptAll()">Accept All</button>
  <button onclick="savePreferences()">Save Preferences</button>
  <button onclick="rejectNonEssential()">Reject Non-Essential</button>
</div>
```

---

## Data Subject Rights (GDPR)

### Rights Implementation Matrix

| Right | Description | Response Time | Implementation |
|-------|-------------|--------------|----------------|
| **Access** | Copy of all personal data | 1 month | Export API |
| **Rectification** | Correct inaccurate data | 1 month | Edit in settings |
| **Erasure** | Delete all data ("Right to be forgotten") | 1 month | Delete account flow |
| **Portability** | Data in machine-readable format | 1 month | JSON/CSV export |
| **Object** | Stop processing for specific purpose | Immediate | Unsubscribe flow |
| **Restrict** | Limit processing temporarily | Immediate | Account freeze |

### Data Subject Request Process

```markdown
## Data Subject Request Handling

### Step 1: Receive Request
- [ ] Log request in tracking system
- [ ] Assign request ID: DSR-[YYYY]-[NNN]
- [ ] Set deadline: [Date + 30 days]

### Step 2: Verify Identity
- [ ] Confirm requester is data subject
- [ ] Method: [Email verification / ID check / 2FA]
- [ ] Document verification: [Screenshot/notes]

### Step 3: Process Request

#### For Access/Portability:
- [ ] Query all systems for user data
- [ ] Compile data export
- [ ] Review for third-party data
- [ ] Send to user via secure channel

#### For Erasure:
- [ ] Identify all data locations
- [ ] Check legal holds/obligations
- [ ] Soft delete from primary DB
- [ ] Schedule hard delete (30 days)
- [ ] Notify third-party processors
- [ ] Confirm deletion complete

### Step 4: Response
- [ ] Send confirmation to user
- [ ] Log completion
- [ ] Archive request record
```

---

## Data Retention Policy Template

```markdown
## Data Retention Policy

| Data Type | Retention Period | After Retention | Legal Basis |
|-----------|-----------------|-----------------|-------------|
| Account data | Until deletion | Hard delete | Contract |
| Transaction records | 7 years | Archive | Legal obligation |
| Support tickets | 3 years | Anonymize | Legitimate interest |
| Session logs | 90 days | Auto-delete | Legitimate interest |
| Marketing consent | 2 years inactive | Request re-consent | Consent |
| Backups | 30 days rolling | Overwritten | Contract |

### Automated Deletion Jobs

| Schedule | Action |
|----------|--------|
| Daily | Delete expired session logs |
| Weekly | Hard delete soft-deleted accounts (>30d) |
| Monthly | Anonymize old support tickets (>3y) |
| Quarterly | Review and update retention policy |
```

---

## Privacy Impact Assessment (DPIA)

```markdown
## DPIA: [Feature/Project Name]

### 1. Description of Processing
- **What**: [Type of processing]
- **Why**: [Purpose]
- **Who**: [Data subjects affected]
- **How long**: [Retention period]
- **Where**: [Storage locations]

### 2. Necessity and Proportionality
| Question | Answer |
|----------|--------|
| Is processing necessary for the purpose? | |
| Could we achieve the goal with less data? | |
| Is the data minimized? | |
| Is retention period justified? | |

### 3. Risk Assessment

| Risk | Likelihood | Impact | Risk Level | Mitigation |
|------|-----------|--------|------------|------------|
| Data breach | Medium | High | High | Encryption, access controls |
| Unauthorized access | Low | High | Medium | RBAC, audit logs |
| Data loss | Low | Medium | Low | Backups, DR plan |

### 4. Measures to Address Risks
- [ ] Encryption at rest and in transit
- [ ] Access controls and audit logging
- [ ] Regular security testing
- [ ] Incident response plan
- [ ] Staff training

### 5. Sign-off

| Role | Name | Approval | Date |
|------|------|----------|------|
| DPO | | ☐ | |
| Legal | | ☐ | |
| Engineering | | ☐ | |
```

---

## Privacy by Design Checklist

```markdown
## Privacy Review: [Feature Name]

### Data Minimization
- [ ] Only essential data collected
- [ ] No "nice to have" fields
- [ ] Optional fields clearly optional

### Purpose Limitation
- [ ] Data used only for stated purpose
- [ ] No secondary use without consent
- [ ] Clear privacy notice

### Storage Limitation
- [ ] Retention period defined
- [ ] Auto-deletion implemented
- [ ] Archive vs delete policy clear

### Security
- [ ] Data encrypted at rest
- [ ] Data encrypted in transit
- [ ] Access controls enforced
- [ ] Audit logging enabled

### User Control
- [ ] Users can access their data
- [ ] Users can delete their data
- [ ] Users can export their data
- [ ] Users can modify consent
```

---

## Human Intervention Required

🔴 **STOP AND ASK USER** before:
- Processing data subject requests
- Making data retention decisions
- Sharing data with third parties
- Conducting DPIA for high-risk processing
- Legal compliance decisions
