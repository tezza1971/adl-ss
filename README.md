# ADL-SS: Atomic Definition Language

[![CI Status](https://img.shields.io/github/actions/workflow/status/username/adl/ci.yml?branch=main&style=flat-square)](https://github.com/username/adl/actions)
[![Crates.io](https://img.shields.io/crates/v/adl.svg?style=flat-square)](https://crates.io/crates/adl)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)

**ADL-SS** (Atomic Definition Language - Specification; Software) is a set of **guidelines** and **conventions** for writing human-readable and machine-parsable software requirements. It is designed from the ground up to enable true spec-driven development with AI code assistants.

This project provides a clear alternative to ambiguous Markdown-based specs by promoting a structured, verifiable YAML format.

---

### 💬 The Problem: Markdown is Ambiguous

For years, the default for writing software specs has been `README.md` files or other Markdown documents. This is a fundamental problem: **Markdown is designed for presentation, not for definition.**

This semantic ambiguity is a critical failure point for AI-driven development. An AI assistant (or a human, for that matter) cannot definitively know if a bullet point is a:

* Hard requirement (`MUST`)
* Optional feature (`SHOULD`)
* A simple note or comment
* A task for a specific component
* A user story

This ambiguity leads to misinterpretation, wasted effort, and code that doesn't match the intended design.

### 💡 The Solution: A Convention for Atomic YAML

ADL solves this by using YAML as both the **definition language** and the **specification format**. The core idea is to follow a simple set of conventions to remove ambiguity.

1.  **Strict Structure:** The ADL guideline promotes a clear, hierarchical YAML schema. The *structure* of the YAML file *is* the meaning.
2.  **Controlled Vocabulary:** Inspired by the principles of [ASD-STE100 (Simplified Technical English)](https://www.asd-ste100.org/), ADL uses a "meta-dictionary" file to define the exact, approved nomenclature (words, components, and their meanings) for your project.
3.  **Machine Parsable:** An AI or a simple parser can read an `.adl.yml` file and know *exactly* what the requirements are, which components they apply to, their relationships, and their priority.

### 🏛️ Core Concepts

The ADL ecosystem is built on four main ideas:

1.  **File Naming Convention:**
    By convention, ADL files follow a `thing.language-token.adl.yml` pattern.
    * **`dictionary.en.adl.yml`**: A meta-dictionary in English.
    * **`dictionary.es.adl.yml`**: A meta-dictionary in Spanish.
    * **`auth-feature.en.adl.yml`**: A feature specification in English.

2.  **The Meta-Dictionary (e.g., `dictionary.en.adl.yml`):**
    This is the "dictionary" and "grammar" for your project's specifications. It is a YAML file you create that defines the allowed `components` (e.g., `api-server`, `database`), `requirement_types` (e.g., `functional`), and other key terms. The ADL linter uses this file to validate your specs.

3.  **The ADL Spec (e.g., `*.adl.yml`):**
    This is the actual specification you write for a feature, bugfix, or system. It is a YAML file that *conforms* to the rules you defined in your meta-dictionary.

4.  **The Render Pipeline (YAML → XML → XSLT → HTML):**
    A core principle is the **strict separation of content from presentation**. This is achieved by first converting the semantic YAML (content) into an intermediate semantic XML (the "byte code"), which is then transformed using XSLT (presentation) to produce the final document.

### 🧩 Example: A Simple ADL Spec

Here is a simple example of an ADL spec for a user authentication feature.

**File: `features.en.adl.yml`**
```yaml
# This spec follows the rules defined in `dictionary.en.adl.yml`
# The 'specification' field is optional and user-defined.
# It's a good practice for internal tracking.
specification: ADL-SS-EN-01

feature:
  id: FEAT-001
  name: User Authentication
  description: Provides a secure login endpoint for registered users.
  status: specified

requirements:
  - id: REQ-001
    # 'type' and 'component' must be defined in the meta-dictionary
    type: MUST
    component: auth_service
    description: The service MUST expose a POST endpoint at `/api/v1/login`.

  - id: REQ-002
    type: MUST
    component: auth_service
    description: The endpoint MUST accept a JSON body with 'email' and 'password'.

  - id: REQ-003
    type: MUST
    component: auth_service
    description: The service MUST return a JWT on successful authentication.

  - id: REQ-004
    type: SHOULD
    component: auth_service
    description: The service SHOULD implement rate limiting to prevent brute-force attacks.

tasks:
  - id: TASK-001
    component: auth_service
    description: Implement POST /api/v1/login endpoint.
  
  - id: TASK-002
    component: database
    description: Add 'last_login_at' timestamp to 'users' table.
