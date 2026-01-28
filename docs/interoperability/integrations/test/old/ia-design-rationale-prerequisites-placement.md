# Information Architecture Design Rationale: Prerequisites Placement

**Document Version**: 1.0  
**Date**: 27 January 2026  
**Status**: Design Rationale Documentation  
**Context**: MOSIP-CRVS Integration Guide IA v2.0

---

## Question Addressed

**Why is Section 11 (Operational Considerations / Prerequisites) placed after conceptual and technical content, rather than earlier in the document structure?**

---

## Design Decision

**Placement**: Section 11 (after Sections 1-10: Context, Overview, Principles, Workflows, Architecture, APIs, Security, Events, Errors, Policy)

**Rationale**: Strategic positioning based on progressive disclosure principles and reader journey optimization.

---

## Detailed Rationale

### 1. **Progressive Disclosure Principle**

The IA follows a **concept-first, implementation-later** approach:

```
Understanding WHY & WHAT (Sections 1-4)
    ↓
Understanding HOW (Architecture & APIs - Sections 5-7)
    ↓
Understanding CONFIGURE & DEPLOY (Prerequisites - Section 11)
```

**Rationale**: Readers need to understand **what they're building** before diving into **how to set it up**.

**Evidence**: 
- Decision-makers evaluate feasibility (Sections 1-3) before committing resources
- Architects design solutions (Sections 4-5) before provisioning infrastructure
- Developers understand APIs (Section 6) before configuring clients

---

### 2. **Reader Journey Optimization**

#### **Persona 1: Decision-Makers** (Reading Path 1):
- **Goal**: Determine ROI and feasibility
- **Path**: Sections 1-3 answer: "Should we do this integration?"
- **Stop Point**: Section 3 (principles)
- **Prerequisites Needed?**: ❌ No - they don't implement

#### **Persona 2: Architects** (Reading Path 2):
- **Goal**: Design integration architecture
- **Path**: Sections 1-5 answer: "How should this be designed?"
- **Stop Point**: Section 5 (architecture)
- **Prerequisites Needed?**: ❌ Not yet - implementation details come later

#### **Persona 3: Developers** (Reading Path 3):
- **Goal**: Implement API integration
- **Path**: Sections 2, 4 (workflows), 6 (APIs), 7 (security), 13 (examples)
- **When Prerequisites Matter**: After understanding APIs (Section 6)
- **Prerequisites Needed?**: ✅ Yes - **but only after** technical understanding

#### **Persona 4: Operators** (Reading Path 4):
- **Goal**: Deploy and configure systems
- **Path**: Section 11 → 7.1 (OAuth setup) → 12 (testing) → 14 (troubleshooting)
- **Prerequisites Needed?**: ✅ Yes - **as first step in their journey**

**If prerequisites were earlier** (say, Section 2 or 3):
- ❌ Developers wouldn't know **why** they're creating OAuth clients
- ❌ Keycloak configuration steps would seem **arbitrary** without API context
- ❌ Field mapping requirements unclear without seeing data schema (Section 6)
- ❌ Decision-makers and architects forced through irrelevant setup details

---

### 3. **Cognitive Load Management**

**Current Structure**:
```markdown
Sections 1-4: CONCEPTUAL (Low cognitive load - stories, principles, workflows)
Sections 5-7: TECHNICAL (Medium load - architecture, APIs, security)
Sections 8-10: ADVANCED (Medium-high load - events, errors, policies)
Section 11: OPERATIONAL (High load - hands-on configuration steps)
Sections 12-14: VALIDATION (Medium load - testing, troubleshooting)
```

**Why This Works**:
- Readers build **mental models** first (Sections 1-4)
- Then learn **technical interfaces** (Sections 5-7)
- Finally **execute** based on that foundation (Section 11)
- Then **validate** their work (Sections 12-14)

**Information Processing Theory**:
- Working memory can hold 5-9 items simultaneously
- Complex configuration requires understanding context
- Prerequisites **make sense** only after conceptual foundation

**If prerequisites were at Section 2**:
- ❌ Too much detail too early (cognitive overload)
- ❌ Readers lose the "big picture" (forest for the trees)
- ❌ Configuration steps lack context (rote copying without understanding)
- ❌ Higher error rates (misconfigurations due to lack of understanding)

---

### 4. **Industry Benchmarking Alignment**

#### **OpenCRVS Documentation Pattern**:
1. Why OpenCRVS (value proposition)
2. Product specifications (capabilities)
3. Technology architecture (design)
4. **Setup & deployment** (comes after conceptual understanding)

#### **Stripe API Documentation**:
1. Overview (what Stripe does)
2. Quickstart (simple example)
3. API reference (detailed specs)
4. **Setup guides** (authentication, webhooks) - comes after API understanding

#### **AWS Documentation**:
1. What is [Service]? (introduction)
2. How it works (concepts)
3. Getting started (guided tutorial with prerequisites embedded)
4. **Prerequisites** (detailed infrastructure) - links from getting started

**Applied to MOSIP-CRVS**:
- Sections 1-10 = Conceptual + Technical understanding
- Section 11 onwards = Practical implementation
- **Pattern Match**: Industry leaders place prerequisites **contextually**, not chronologically first

---

### 5. **Task-Oriented Documentation Standards**

**Technical Writing Best Practice**: 
> Prerequisites should appear **immediately before the task** they support, not at document start.

**In This Guide**:
```
Section 11 (Prerequisites) 
    ↓
Section 12 (Testing & Validation)
    ↓
Section 13 (Code Examples & Implementation)
```

This creates a **"setup → validate → implement"** workflow.

**If prerequisites were at Section 2**:
- ❌ **Temporal disconnect**: 50+ pages between setup instructions and actual use
- ❌ **Forgotten details**: Readers forget Keycloak client IDs by Section 6
- ❌ **Redundant re-reading**: Developers flip back to Section 2 repeatedly
- ❌ **Maintenance burden**: Prerequisites duplicated in multiple sections

**Current Placement Benefits**:
- ✅ **Just-in-time information**: Prerequisites appear when readers are ready to implement
- ✅ **Reduced cognitive load**: No need to remember setup details for 50 pages
- ✅ **Natural workflow**: Understand → Setup → Implement → Test
- ✅ **Single source of truth**: Prerequisites documented once, referenced elsewhere

---

### 6. **Document Type Classification**

This guide is:
- ✅ **Comprehensive reference** for multiple personas
- ✅ **Decision-support** for architects and managers
- ✅ **Implementation guide** for experienced integrators
- ❌ NOT a quickstart tutorial
- ❌ NOT a beginner's guide
- ❌ NOT a single-task walkthrough

**Implications for Prerequisites Placement**:

| Document Type | Prerequisites Placement | Rationale |
|---------------|------------------------|-----------|
| **Quick Start Guide** | Section 1-2 (early) | Get users running ASAP |
| **Tutorial** | Section 2-3 (early) | Hands-on from page 1 |
| **Reference Guide** | Section 10+ (late) | Context before configuration |
| **API Documentation** | Embedded per endpoint | Just-in-time per task |
| **Architecture Guide** | Near end or appendix | Implementation follows design |

**MOSIP-CRVS Guide Type**: **Comprehensive Reference + Implementation Guide**

**Correct Placement**: Section 11 (after conceptual foundation, before validation)

---

## Evidence from IA Design Principles

From the Information Architecture document:

### **Principle: Progressive Disclosure**
> Information layered from **high-level concepts → detailed specifications**:
> - Start with business context and objectives
> - Move to integration patterns and workflows
> - End with technical implementation details

**Application**: Prerequisites are **technical implementation details** → belong near end.

### **Principle: User-Centered Structure**
> Content organized by **user goals**, not system components:
> - Decision-makers need: Why integrate, what's the scope, what are the constraints
> - Architects need: System boundaries, data flows, security models
> - Developers need: APIs, schemas, error handling, code examples
> - Operators need: Monitoring, troubleshooting, configuration

**Application**: Only **operators** need prerequisites early → they can jump directly to Section 11.

### **Principle: Separation of Concerns**
> Clear boundaries between:
> - **What** (capabilities, use cases) vs **How** (implementation)
> - **Default behavior** vs **Optional extensions**
> - **Policy decisions** vs **Technical constraints**

**Application**: Prerequisites are **"How"** (implementation) → come after **"What"** (Sections 1-4).

---

## Design Decisions Log Entry

| Decision | Rationale | Date | Supporting Evidence |
|----------|-----------|------|---------------------|
| **Prerequisites at Section 11** | Progressive disclosure; reader journey optimization | 2026-01-22 | SME feedback, OpenCRVS benchmark |
| **Workflows before Architecture** | Users understand tasks before system internals | 2026-01-22 | Technical writing standards |
| **Architecture after Workflows** | Context before implementation details | 2026-01-22 | Cognitive load management |
| **Operations after Technical** | Setup requires conceptual foundation | 2026-01-22 | Industry best practices |

---

## Addressing the "But Operators Need It First" Concern

### **Solution 1: Document Navigation Guide (Section 1.3)**

Add explicit pathway for operators:

```markdown
### Quick Access Pathways

#### For Operators (Jump-Start Configuration)
If you're ready to set up the integration immediately:
1. **Section 11**: Prerequisites & System Requirements ← START HERE
2. **Section 7.1**: OAuth2 Client Setup
3. **Section 12**: Testing & Validation
4. **Section 14**: Troubleshooting

**Prerequisite**: Assumes familiarity with MOSIP and CRVS concepts.
If new to integration, read Sections 1-4 first.
```

### **Solution 2: Cross-References in Workflow Sections**

In Section 4.2 (Birth Registration Workflow):

```markdown
**Before Starting**: Ensure prerequisites from [Section 11](#11-operational-considerations) are met:
- Keycloak client configured
- Object store accessible
- Network connectivity verified
```

### **Solution 3: Separate Quick Setup Guide** (Optional)

Create `quick-setup-guide.md`:
- Extract Section 11 content
- Add condensed context (2-3 pages)
- Link back to full guide for details

**Trade-off**: Maintenance burden (two documents to update).

---

## Alternative Placement Options (Considered & Rejected)

### **Option 1: Prerequisites at Section 2**
```
1. Introduction
2. Prerequisites ← Here
3. Integration Overview
...
```

**Pros**:
- ✅ Immediate for "doers"
- ✅ Traditional structure (some docs do this)

**Cons**:
- ❌ Breaks conceptual flow for architects/decision-makers
- ❌ Context-free configuration (why are we creating OAuth clients?)
- ❌ Cognitive overload (too much detail before understanding value)
- ❌ Poor persona support (forces non-implementers through irrelevant content)

**Verdict**: ❌ Rejected - Violates progressive disclosure

---

### **Option 2: Prerequisites at Section 6 (Before APIs)**
```
1-5. [Context & Workflows]
6. Prerequisites ← Here
7. API Reference
...
```

**Pros**:
- ✅ Right before technical usage
- ✅ Developers get setup info when starting API work

**Cons**:
- ❌ Interrupts API reference flow
- ❌ Too early for operators (who need workflow understanding first)
- ❌ Splits technical content awkwardly

**Verdict**: ❌ Rejected - Breaks content flow

---

### **Option 3: Split Prerequisites (Basic Setup Early, Advanced Later)**
```
2. Basic Prerequisites (minimal setup)
...
11. Advanced Operational Setup (detailed config)
```

**Pros**:
- ✅ Partial early access for operators
- ✅ Staged complexity

**Cons**:
- ❌ Duplication and maintenance burden (two prerequisite sections)
- ❌ Readers miss dependencies (what's "basic" vs "advanced"?)
- ❌ Arbitrary split (hard to define boundary)

**Verdict**: ❌ Rejected - Creates maintenance problems

---

### **Option 4: Prerequisites as Appendix**
```
1-14. [Main content]
15. Appendices
    ├─ A. Prerequisites
    └─ B. Glossary
```

**Pros**:
- ✅ Doesn't interrupt main flow
- ✅ Reference-style access

**Cons**:
- ❌ Too hidden (operators might miss it)
- ❌ Feels secondary (prerequisites are actually critical)
- ❌ Poor discoverability

**Verdict**: ❌ Rejected - Under-emphasizes critical content

---

## Why Section 11 is Optimal

### **Placement Evaluation Matrix**

| Criterion | Section 2 | Section 6 | Section 11 (Current) | Appendix |
|-----------|-----------|-----------|---------------------|----------|
| **Progressive Disclosure** | ❌ Poor | ⚠️ Fair | ✅ Excellent | ⚠️ Fair |
| **Decision-Maker Path** | ❌ Blocks | ✅ Doesn't block | ✅ Doesn't block | ✅ Doesn't block |
| **Architect Path** | ❌ Interrupts | ⚠️ Interrupts | ✅ Doesn't interrupt | ✅ Doesn't interrupt |
| **Developer Path** | ⚠️ Too early | ✅ Good timing | ✅ Perfect timing | ⚠️ Too late |
| **Operator Path** | ✅ Immediate | ⚠️ Early enough | ⚠️ Requires jump | ❌ Hidden |
| **Cognitive Load** | ❌ Overload | ⚠️ Moderate | ✅ Optimal | ✅ Low |
| **Discoverability** | ✅ High | ✅ High | ✅ High | ❌ Low |
| **Maintenance** | ✅ Single source | ✅ Single source | ✅ Single source | ✅ Single source |
| **Industry Alignment** | ❌ Non-standard | ⚠️ Uncommon | ✅ Standard | ⚠️ Uncommon |

**Score**: Section 11 wins on 7/9 criteria.

---

## Conclusion

### **Section 11 Placement is Optimal Because:**

1. ✅ **Serves Multiple Personas**
   - Decision-makers stop early (Sections 1-3)
   - Architects focus on design (Sections 4-5)
   - Developers get prerequisites when ready (Section 11, after APIs)
   - Operators can jump directly (Section 11 via TOC)

2. ✅ **Respects Progressive Disclosure**
   - Concept → Design → Implementation
   - Context before configuration
   - Understanding before execution

3. ✅ **Aligns with Industry Standards**
   - OpenCRVS, Stripe, AWS follow similar patterns
   - Technical writing best practices
   - Task-oriented documentation principles

4. ✅ **Maintains Logical Flow**
   - Understand system (Sections 1-5)
   - Learn interfaces (Sections 6-7)
   - Configure system (Section 11)
   - Test system (Section 12)
   - Implement features (Section 13)
   - Troubleshoot (Section 14)

5. ✅ **Reduces Cognitive Load**
   - Prerequisites make sense AFTER technical understanding
   - Just-in-time information delivery
   - No need to remember setup details for 50+ pages

6. ✅ **Supports Document Type**
   - This is a **comprehensive reference**, not a quickstart
   - Readers are **integrators with MOSIP knowledge**, not beginners
   - **Correct setup requires understanding WHY**, not just copying commands

---

## Design Principle Statement

> **IA Principle for Integration Guides**:
> 
> Prerequisites and operational setup should appear **after** conceptual understanding and **before** validation procedures, positioned where implementers have sufficient context to configure correctly.
>
> For comprehensive reference guides serving multiple personas, this typically means Section 10-12 (after APIs, before testing).
>
> For quick-start guides serving single persona (operators), this means Section 1-2 (immediate access).

---

## Recommendations

### **For Current MOSIP-CRVS Integration Guide**

1. ✅ **Keep Section 11 placement** (optimal for multi-persona reference guide)

2. ✅ **Enhance Section 1.3 (Document Navigation Guide)** with explicit operator pathway:
   ```markdown
   ### Quick Access: Setup & Configuration
   Operators ready to configure integration, jump to:
   - **Section 11**: Prerequisites & System Setup
   - **Section 7.1**: OAuth2 Client Setup
   - **Section 12**: Testing & Validation
   ```

3. ✅ **Add cross-references** in workflow sections (Section 4.x):
   ```markdown
   **Prerequisites**: Before starting, ensure [Section 11 prerequisites](#11-operational-considerations) are met.
   ```

4. ⚠️ **Consider separate Quick Setup Guide** (optional, if operator feedback indicates need):
   - 5-10 page condensed version
   - Prerequisites upfront
   - Links to full guide for details

### **For Future Integration Guides**

1. **Reference Guides** (like MOSIP-CRVS): Prerequisites at Section 10-12
2. **Quick Start Guides**: Prerequisites at Section 1-2
3. **Tutorials**: Prerequisites embedded just-in-time per task
4. **API Documentation**: Prerequisites per endpoint or in "Before You Begin" section

---

## Document Control

**Author**: Information Architect (GitHub Copilot)  
**Review Status**: Design Rationale Documented  
**Next Review**: When user feedback indicates placement issues  
**Related Documents**:
- `_information-architecture.md` (IA blueprint)
- `mosip-crvs-integration-guide.md` (implementation)

---

## Appendix: Reader Feedback Scenarios

### **Scenario 1: "I can't find the prerequisites!"**
**Diagnosis**: Discoverability issue  
**Solution**: Enhance TOC, add callout in Section 1.3, improve search keywords

### **Scenario 2: "Why are prerequisites so late in the document?"**
**Diagnosis**: Reader expects quickstart, not reference guide  
**Solution**: 
- Add document type clarification in Section 1.1
- Create separate quick setup guide
- Add operator pathway in Section 1.3

### **Scenario 3: "I had to flip back and forth between Section 6 and Section 11"**
**Diagnosis**: Expected behavior for reference guide  
**Solution**: Add cross-references, consider PDF bookmarks, improve internal linking

### **Scenario 4: "I understood prerequisites better after reading APIs first"**
**Diagnosis**: ✅ **Design working as intended**  
**Solution**: No change needed - progressive disclosure is effective

---

**End of Document**
