# Prompt Examples (EN)

Use these prompts as a starting point when working with **Specification-Driven Development (SDD)**. Adjust to your feature context and always attach artifacts (`spec.md`, `plan.md`) when applicable. Remember to follow the workflow: first create the SPEC (define WHAT to build), then the PLAN (define HOW to build it), and finally implement.

> **📚 Complete SDD Guide**: For a detailed explanation about the SDD methodology, its principles, workflow and best practices, consult /specs/user-guide/en/SDD_GUIDE.MD

## Spec Generation

The SPEC defines **WHAT** should be built, without implementation details. Use the template in `specs/templates/spec-template.md` as a base.

### Example 1: Basic feature with project context
```
Create a SPEC for "Item listing with filters". This feature allows users to search and filter products in the catalog. It must support two types of users: administrators (who see all inventory) and buyers (who filter by category, tags, and price). In this initial phase we want to allow filters for 3 main categories (Electronics, Furniture, Clothing) and combinations of up to 5 simultaneous tags. Users should be able to see results in real time with pagination of 20 items per page.

Use the template in @spec-template.md as a base to create the file @spec.md.

IMPORTANT: Do not modify the template, use it only as a reference to structure the new file. If you find ambiguous requirements or missing information, ask me clarifying questions before assuming details.
```

### Example 2: Feature with reference to existing documentation (POST /items)
```
Create a SPEC for the POST /items functionality that allows creating new items in the catalog. Review the file @items-api.yaml to understand the context of the existing API and maintain consistency with other endpoints.

This feature must allow users to create new items with the following information:
- Required fields: name (string, maximum 200 characters), category (string, must exist in the categories catalog)
- Optional fields: description (string, maximum 1000 characters), tags (array of strings, maximum 10 tags, each tag maximum 50 characters), attributes (JSON object with custom attributes)

Specify the complete validation rules (maximum lengths, allowed formats, special characters), what to respond in case of success (status 201, include generated ID and creation timestamp), and how to handle validation errors (status 400 with details) or conflicts (status 409 if category does not exist). Document whether new categories are allowed or only existing ones, limits on tags/attributes, performance objectives (response time < 500ms), and security requirements (input sanitization).

Use the template in @spec-template.md as a base to create the file @spec.md.

IMPORTANT: Do not modify the template, use it only as a reference to structure the new file. If you find unclear validations or ambiguous business rules, ask before assuming.
```

### Example 3: Feature with MCP usage for platform context
```
Create a SPEC to implement a notifications service that uses Fury's BigQueue. I need the service to send asynchronous notifications when a new item is created. 

Use the backend namespace and the fury__search_sdk_docs tool to consult the BigQueue library documentation. Execute fury__search_sdk_docs with service="bigqueue", language="go", and a specific query such as "message queue capabilities and usage" to understand how BigQueue works and its capabilities. If you cannot obtain information from the MCP or the corresponding tool, let me know and stop execution. Define the functional requirements (what types of notifications, to whom, when), non-functional requirements (latency, throughput), and acceptance criteria.

Use the template in @spec-template.md as a base to create the file @spec.md.

IMPORTANT: Do not modify the template, use it only as a reference to structure the new file. Ask questions if something is unclear.
```

### Example 4: Feature related to an existing spec
```
Create a SPEC for "Item update" (PUT /items/{id}). This feature is related to the item creation feature that is already documented in @spec.md. 

Use the previously created spec as a guide and documentation to maintain consistency in validations, data structure, and error handling. Define which fields can be updated, specific validation rules for updates, and behavior when the item does not exist.

Use the template in @spec-template.md as a base to create the file @spec.md.

IMPORTANT: Do not modify the template, use it only as a reference to structure the new file. If you find inconsistencies or need clarifications, ask before continuing.
```

## Plan Creation

The PLAN defines **HOW** the solution will be implemented. It must be created after having a validated SPEC and must cover all SPEC requirements.

### Example 1: Plan based on existing SPEC
```
Create a PLAN to implement "Item listing with filters" based on the SPEC in @spec.md.

Design the architecture for GET /items with filters. Handler accepts query params: ?category=, ?tags= (comma-separated), ?page=, ?limit= (default 20). Service builds SQL query with WHERE and JOIN item_tags. Response JSON with structure {"items": [...], "pagination": {...}}. Errors: 400 if invalid parameters, 200 empty array if no results. 

Include: complete technical design, data structures, API contracts, testing strategy (unit, integration, e2e), and granular tasks. Review existing project files such as internal/handlers/item_handlers.go to maintain architectural consistency.

Use the template in @plan-template.md as a base to create the file @plan.md.

IMPORTANT: Do not modify the template, use it only as a reference to structure the new file.
```

### Example 2: Plan with reference to similar implementations
```
Create a PLAN for POST /items based on the SPEC in @spec.md. 

Review the existing implementation of GET /items in internal/handlers/item_handlers.go and internal/services/item_service.go to maintain consistency in patterns and structure. 

Request body according to schemas/create_item_request.json with fields name (required), category (required), description (optional), tags (optional, max 10). Handler validates schema, returns 400 with {"error": "validation_failed", "details": [...]} if fails. Service validates category exists (SELECT categories), returns 409 if invalid. Storage uses transaction: INSERT items → INSERT item_tags/attributes → COMMIT. Response 201 with header Location: /items/{id} and body with created item. DB timeout: 3s, no retry. 

Include metrics (creation_time, validation_errors), tests (unit validations, integration MySQL, contract status 201), and all tasks necessary for complete implementation.

Use the template in @plan-template.md as a base to create the file @plan.md.

IMPORTANT: Do not modify the template, use it only as a reference to structure the new file.
```

### Example 3: Plan with Fury SDKs usage
```
Create a PLAN to implement the notifications service using Fury's BigQueue, based on the SPEC in @spec.md.

Use the backend namespace and the fury__search_sdk_docs tool to consult the BigQueue library documentation. Execute fury__search_sdk_docs with service="bigqueue", language="go", and a specific query such as "message queue capabilities and usage" to understand how BigQueue works and its capabilities. If you cannot obtain information from the MCP or the corresponding tool, let me know and stop execution.

Include: service technical design, BigQueue integration, error handling and retries, testing strategy, observability metrics, and granular implementation tasks.

Use the template in @plan-template.md as a base to create the file @plan.md.

IMPORTANT: Do not modify the template, use it only as a reference to structure the new file. Make sure to cover all SPEC requirements.
```

### Example 4: Plan with existing architecture review
```
Create a PLAN for PUT /items/{id} based on the SPEC in @spec.md.

Before designing, review:
- The existing architecture in internal/handlers/ and internal/services/
- The item creation SPEC in @spec.md to maintain consistency
- The data models in internal/models/item.go

Design the complete implementation including: validations, transaction handling, relationship updates (tags, attributes), exhaustive testing strategy, and all necessary tasks. Make sure the plan is coherent with the project's existing architecture.

Use the template in @plan-template.md as a base to create the file @plan.md.

IMPORTANT: Do not modify the template, use it only as a reference to structure the new file.
```

## Implementation

Once you have a validated SPEC and a complete and reviewed PLAN, you can proceed with implementation.

### Example 1: Implementation with attached artifacts
```
Implement the "Item listing with filters" feature following the PLAN in @plan.md.

I attach the SPEC (@spec.md) and the PLAN (@plan.md) as reference. Follow the plan step by step, implement all listed tasks, and make sure to comply with the project's code standards defined in CODING_GUIDELINES.md.
```

### Example 2: Iterative implementation
```
Implement the first phase of POST /items according to the PLAN in @plan.md, section "Phase 1: Handler and basic validations".

I attach the complete SPEC (@spec.md) and PLAN (@plan.md). Focus only on Phase 1 tasks. Once completed, we will continue with the following phases.
```

## Additional Tips

- **Separate chats**: Create independent chats for SPEC, PLAN and implementation to optimize token usage
- **Attach artifacts**: Always attach `spec.md` and `plan.md` when working on implementation
- **Cross-references**: Mention related features and their artifacts to maintain consistency
- **Clarification questions**: Explicitly indicate to the agent to ask when finding ambiguities
- **MCP usage**: When working with Fury services, use the backend namespace and the fury__search_sdk_docs tool to consult official SDK documentation. Execute fury__search_sdk_docs with the corresponding service (kvs, bigqueue, os, ds, stream, workqueue, locks, sequence, core), your project's language, and specific queries about the functionality you need to implement. **It is important to mention to the agent that if it cannot use the MCP or obtain information from the MCP, it must stop and notify you**. This prevents the agent from making incorrect assumptions based on generic knowledge instead of Fury's official documentation, which could lead to implementations that are incompatible or incorrect with the platform.

