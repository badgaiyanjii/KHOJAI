# KhojAI Intelligent Item Recovery Portal

KhojAI is a campus-focused lost-and-found management system designed to replace informal, manual recovery processes with a centralized digital workflow. Across the project documents, it is also described as a **Campus Lost & Found Portal (Department Controlled System)**. The system is intended for educational institutions and similar controlled environments where misplaced items need structured reporting, intelligent matching, verified claims, and transparent handover.

This README is derived from the documents in `docs/` and summarizes the project as documented. The current repository contains documentation PDFs only, so the implementation details below describe the **documented system design**, not verified source code in this repo.

## Document Sources

- `docs/lost and found.pdf`
- `docs/🏫 COMPLETE SYSTEM DESIGN.pdf`

## Implementation Docs

The repo now includes implementation-facing Markdown specs that lock the future build on a single target stack:

- frontend: `React + Vite`
- UI layer: `Tailwind CSS`
- backend: `Node.js + Express`
- data/auth: `MongoDB + JWT`
- AI matching: internal `Python` service

Primary implementation docs:

- [`docs/requirements.md`](docs/requirements.md)
- [`docs/architecture.md`](docs/architecture.md)
- [`docs/workflow.md`](docs/workflow.md)
- [`docs/framework.md`](docs/framework.md)
- [`docs/ui-tech.md`](docs/ui-tech.md)

Supporting specs:

- [`docs/api-spec.md`](docs/api-spec.md)
- [`docs/data-model.md`](docs/data-model.md)
- [`docs/ai-matching.md`](docs/ai-matching.md)

## Abstract

In many colleges and institutions, lost-and-found handling still depends on notice boards, verbal communication, social media messages, or manual registers. These methods are unstructured, slow, and difficult to track. They often lead to delayed recovery, poor record maintenance, low transparency, and unnecessary administrative effort.

KhojAI proposes an AI-enabled web platform that digitizes this process. Students and staff can report lost or found items with structured metadata such as item name, category, description, date, location, and image. The platform then stores the data centrally, supports search and filtering, and uses image similarity matching to identify likely lost/found pairs. Claim verification remains admin-controlled so ownership is validated before physical handover.

## Problem Statement

The project addresses several recurring issues in institutional lost-and-found workflows:

- No centralized system for storing and managing lost and found reports.
- Manual comparison of notices, messages, and registers is slow and error-prone.
- Item records often lack consistent fields such as images, locations, dates, and descriptions.
- Users have no reliable way to track whether an item has been reported or matched.
- Claim approval is often informal, with weak ownership verification.
- Administrative staff carry the entire matching and verification burden manually.

The project therefore targets a centralized, structured, and partially automated solution that improves recovery efficiency while maintaining security and accountability.

## Objectives

The documented objectives of KhojAI are to:

- replace manual and informal lost-and-found handling with a structured digital platform
- provide a centralized repository for item data and claim records
- integrate AI-assisted image similarity matching for likely lost/found pair detection
- reduce manual searching effort and improve recovery efficiency
- implement secure authentication and role-based access control
- support backend processing, data storage, and API communication through a modular web architecture
- provide admin-controlled claim verification to reduce misuse
- enable real-time search and item status tracking
- keep the system scalable for future mobile, cloud, and multi-campus expansion

## Scope

The project scope, as described in the documents, is focused on a software-based solution for controlled environments such as:

- colleges and universities
- hostels and residential campuses
- libraries
- office or institutional campuses
- event-managed environments

The system is intentionally designed without hardware dependencies such as physical tracking devices. Its value comes from structured software workflows, centralized data storage, AI-assisted image comparison, and a physical handover process managed by a central department or lost-and-found office.

## Intended Users and Roles

### Student or User

The standard user can:

- register and log in
- report a lost item
- upload a found item
- search item listings
- receive match notifications
- submit a claim request with additional proof
- track item status

### Department Admin

The admin role is described as a department-controlled or central office role, such as an IT department, admin office, or lost-and-found cell. The admin can:

- manage all item records
- review claim requests
- validate user and claim details
- approve or reject claims
- monitor overall system activity through a dashboard
- coordinate final physical item handover

## User Details Required at Registration

The system design document emphasizes institution-specific registration fields to reduce fake users and simplify claim verification. The documented fields are:

- full name
- roll number or unique institutional ID
- branch or department
- year of study
- email ID
- phone number
- password

These details support identity validation and department-wise tracking during claim review.

## Core Features

### 1. Centralized Item Management

All lost and found reports are stored in one system instead of scattered notices or informal messages. This improves organization, retrieval, record maintenance, and reporting.

### 2. Lost and Found Item Upload

Users can submit structured item information including:

- item name
- category
- description
- date
- location
- image
- type: lost or found

### 3. AI-Assisted Image Similarity Matching

The platform uses pre-trained models to extract visual features from uploaded item images and compare them against stored records. Similarity scores are then used to return top potential matches between lost and found items.

### 4. Smart Search and Filtering

The documents describe real-time search and filtering using criteria such as:

- keyword
- category
- item type
- status
- location

This is meant to reduce browsing effort across a growing item inventory.

### 5. Role-Based Authentication

The system separates normal user capabilities from administrative privileges to protect records and prevent unauthorized claim actions.

### 6. Claim Request and Verification

Users can submit a claim request for a matched or relevant item. The admin reviews the request, validates ownership evidence, and decides whether to approve or reject the claim.

### 7. Item Status Tracking

The documented item lifecycle is:

`Open -> Under Review -> Claimed`

This keeps users informed while making the handover process auditable.

### 8. Administrative Dashboard

The admin dashboard is described as showing:

- total items
- total lost items
- total found items
- pending claims
- successful returns

### 9. Structured Database Management

The system stores user details, item records, images, claim status, and claim history in a structured database model.

### 10. Scalable Modular Design

The project is intended to support future expansion without disrupting current functionality.

## End-to-End Workflow

The most complete workflow appears in the system design document. Consolidated from both PDFs, the documented flow is:

### Step 1. User Login

- The user registers with institution-specific details.
- The user logs in using roll number or unique ID and password.
- The system verifies the credentials and role.

### Step 2. Lost Item Report

- The user enters item name, description, category, date, location, and image.
- The system stores the report in the database.
- AI matching is triggered against relevant found-item records.

### Step 3. Found Item Upload

- A user uploads the found item details and image.
- The item is also physically deposited with a central department, such as an IT department office or lost-and-found cell.
- The system stores the record and informs the admin.

### Step 4. AI Matching Process

When a new relevant item is added:

1. image features are extracted
2. the image is compared with stored item images
3. a similarity score is calculated
4. top likely matches are returned

The expected result is a "possible matches found" style outcome for further review.

### Step 5. Notification System

The documents mention notifications for:

- the user who reported a lost item
- the user who uploaded a found item
- the admin

The notification channels documented are:

- in-app notifications
- optional email notifications

### Step 6. Claim Request

- The user clicks claim on a relevant item.
- The user submits additional proof or identifying details.
- The system records the claim and marks it for admin review.

### Step 7. Admin Verification

The admin validates:

- user identity
- roll number and branch or department
- item description consistency
- proof provided by the claimant

The admin then approves or rejects the request.

### Step 8. Item Handover

- If approved, the user collects the item from the central department.
- Identity is physically verified at handover.
- The system updates the item status to `Claimed`.

## AI Matching Implementation Concept

The PDFs describe the AI module at a conceptual level rather than with model-level code. The documented implementation approach is:

- user uploads an image of a lost or found item
- the system performs feature extraction using a pre-trained model
- extracted visual features are compared against existing item images
- similarity scores are computed
- the highest scoring candidates are returned as potential matches

The project explicitly frames this as an image similarity or image retrieval problem rather than a hardware tracking solution.

## Physical and Digital Integration

One important part of the design is that the system is not only digital. For found items:

- the found item is recorded in the portal
- the physical item is deposited at a central office or department
- the portal shows where the item is physically available
- final item collection happens through the department after approval

This hybrid model is presented in the documents as a major reason the system is practical in real institutional environments.

## Conceptual Data Model

The system design PDF presents the storage model as three main logical tables or collections.

### Users

- name
- roll number
- branch
- year
- email
- phone number
- password

### Items

- item ID
- name
- category
- description
- image URL
- status
- type: lost or found
- location
- date

### Claims

- claim ID
- user ID
- item ID
- status
- admin decision
- proof or supporting details

## System Architecture Summary

Across the two documents, the system is described as a full-stack web application with:

- a frontend for user registration, reporting, search, status tracking, and claim submission
- a backend for authentication, business logic, AI matching triggers, and admin workflows
- a data storage layer for users, items, images, and claims
- an AI or computer vision module for feature extraction and similarity scoring
- an admin dashboard for review, statistics, and claim handling

Although the PDFs include dedicated pages for system architecture, data flow, and use case diagrams, the repository does not contain separate editable diagram source files, so this README summarizes those diagrams from the available text.

## Technology Stack

The main project PDF lists the following stack and technology areas:

- React.js for frontend development
- Node.js and Express.js for backend services
- MongoDB/Firebase for data storage
- Artificial Intelligence for intelligent matching
- Machine Learning for feature extraction model usage
- Computer Vision for image processing and pattern recognition
- RESTful APIs for client-server communication
- NoSQL storage for structured and unstructured data handling

## Expected Outcomes

The project documents expect the system to deliver:

- faster recovery of lost items
- reduced manual administrative effort
- accurate image-based similarity matching
- improved operational transparency
- centralized item tracking
- better user satisfaction
- a practical demonstration of real-world AI deployment

## Applications

The documented application areas include:

- educational institutions
- universities and colleges
- hostels and residential communities
- corporate campuses
- public institutions
- libraries
- event management environments

## Future Enhancements

The documents propose several future extensions:

- mobile application development
- push notifications through email or SMS
- cloud-hosted AI models
- blockchain-based ownership validation
- multi-language support
- stronger deep learning models for improved accuracy
- campus ID system integration
- multi-campus deployment

## Repository Status

This repository currently contains documentation only. At the time of writing, the repo includes the two PDFs in the `docs` folder and does not include application source code, deployment configuration, or executable project modules. For that reason:

- this README documents the intended design and workflow
- it does not claim verified API routes, database schemas, or runnable setup steps
- implementation details are limited to what the PDFs explicitly describe

## Documented Inconsistencies and Notes

The two PDFs are aligned on the core concept, but they are not perfectly consistent. The main differences are:

- **Project naming**
  - one document uses `KhojAI Intelligent Item Recovery Portal`
  - the system design PDF uses `Campus Lost & Found Portal (Department Controlled System)`
- **Storage wording**
  - the main project PDF mentions `MongoDB/Firebase`
  - the design PDF explains storage through conceptual `Users`, `Items`, and `Claims` tables
- **Architecture detail depth**
  - the main PDF is more report-oriented and stack-oriented
  - the system design PDF is more workflow-oriented and emphasizes institution-specific operations such as physical deposit to a department office

This README normalizes the project under the name **KhojAI** while preserving those differences explicitly.

## Conclusion

KhojAI is documented as an intelligent, centralized, and scalable lost-and-found solution for campus and institutional use. Its main value is the combination of:

- structured digital reporting
- AI-assisted image similarity matching
- transparent status tracking
- admin-verified claim approval
- physical handover through a central office

The project demonstrates how AI, computer vision, and full-stack web development can be applied to a practical institutional problem without requiring specialized hardware.

## References Mentioned in the Documents

- OpenCV official documentation
- TensorFlow or PyTorch documentation
- MongoDB official guide
- React.js documentation
- Node.js documentation
- research papers on image retrieval and similarity matching
- REST API design best practices
