---
name: box-legal-ma
description: Build and manage M&A Virtual Data Rooms with Box MCP — create secure folder structures with numbered prefixes for due diligence, assign role-based access to internal teams and external parties (counsel, auditors, buyers), validate permissions before sharing sensitive deal information, use Box AI for cross-document due diligence questions, and organize uploaded files by category. Use this skill when the user mentions M&A, deal rooms, data rooms, due diligence, VDR, mergers and acquisitions, or needs to set up a secure repository for deal documents with controlled external access.
---

# M&A Deal Room Management

> **PREREQUISITES:**
> - Read `box:box` for Box MCP auth, tool selection, base workflows. If missing, run: `npx skills add https://github.com/box/box-for-ai --skill box`
> - Read `box-legal-shared` for risk frameworks, confidentiality, human-in-the-loop requirements, Box AI governance. If missing, ensure it's installed from the same skill package.

M&A Virtual Data Rooms require strict access controls, organized folder structures, and audit trails. This skill guides deal room creation, role-based permissions, and Box AI due diligence.

**Core principles:** Need-to-know access, audit readiness, compartmentalization, validation before sharing.

---

## Permission Architecture

### Internal Roles
| Role | Access Level | Scope |
|------|-------------|-------|
| Deal Lead / M&A Team | Editor or Co-Owner | Root folder |
| Finance Team | Viewer | Financial Statements folder only |
| Legal Team | Editor | Legal Documents folder only |
| Executive Stakeholders | Viewer | Root folder (read-only overview) |

### External Roles

**[CONFIRM WITH USER: External permissions]**
Before granting external access, confirm:
- Permission level? (Upload-Only, Viewer, Editor)
- Which folders? (entire deal room or specific folders)
- Expiration date?
- Shared link or direct collaboration?

**Common patterns:**
- **External Counsel**: Uploader on "External Counsel" folder (can upload, can't see others' files)
- **Auditors**: Viewer on Financial Statements folder only
- **Prospective Buyer**: Viewer on curated subset (not full deal room)

**Always confirm before:**
- Granting Editor/Co-Owner to external parties
- Creating Open shared links
- Granting root folder access outside core team

---

## Standard Folder Structure

**[CONFIRM: Folder structure]**
Before creating, confirm:
- Organization has standard M&A template?
- Additional categories? (Environmental, Insurance)
- Folders to omit?

**Standard structure:**
```
[Deal Name] M&A Deal Room/
├── 01 - Financial Statements/
│   ├── Annual Reports/
│   ├── Quarterly Reports/
│   ├── Audited Financials/
│   └── Tax Returns/
├── 02 - Legal Documents/
│   ├── Corporate Documents/
│   ├── Material Contracts/
│   ├── Litigation/
│   └── Regulatory Filings/
├── 03 - HR & Employment/
├── 04 - Intellectual Property/
├── 05 - Commercial Contracts/
├── 06 - Real Estate & Assets/
├── 07 - IT & Cybersecurity/
└── 08 - External Submissions/
```

**Why numbered prefixes:** Consistent ordering across users, matches DD checklists, practice area alignment, segregates external submissions.

---

## Tool Selection

| Task | Primary Tool | Notes |
|------|--------------|-------|
| Create folders | `create_folder` | Batch create hierarchy |
| Upload new files | `upload_file` | For new documents |
| Copy from Box | `copy_file` | Copy existing Box files |
| Grant internal access | `create_collaboration` | Viewer/Editor/Co-Owner |
| Grant external access | **CONFIRM**, then `create_collaboration` | Always confirm first |
| Validate permissions | `list_item_collaborations` | Audit before sharing |
| Search documents | `search_files_keyword` | Find relevant docs |
| DD Q&A | `ai_qa_multi_file` | Cross-document analysis |
| Extract terms | `ai_extract_structured_from_fields_enhanced` | High accuracy extraction |
| Organize files | `copy_file` | Copy submissions to categories |

---

## Implementation Workflow

### Phase 1: Deal Room Setup
1. **Authenticate**: `who_am_i`
2. **Create root**: `create_folder` with deal name
3. **Create subfolders**: **[CONFIRM: Customize?]** → batch create
4. **Grant internal access**: **[CONFIRM: Emails and roles?]** → `create_collaboration`

### Phase 2: Content Upload & Organization
5. **Upload/copy**: **[CONFIRM: Source?]** → `upload_file` or `copy_file`
6. **Organize submissions**: `get_file_details` → `ai_qa_single_file` (classify) → `copy_file` (copy to category)

### Phase 3: External Access & Sharing
7. **Audit permissions**: `list_item_collaborations` (before external sharing)
8. **Grant external access**: **[CONFIRM: Who, folders, permission, expiration?]** → `create_collaboration` or `add_folder_shared_link`
9. **Verify**: `list_item_collaborations` (confirm correct)

### Phase 4: Due Diligence & Analysis
10. **Answer DD questions**: `search_files_keyword` → `ai_qa_multi_file` → surface citations
11. **Extract terms**: **[CONFIRM: Fields?]** → `ai_extract_structured_from_fields_enhanced`
12. **Write metadata**: **[CONFIRM: Template?]** → `set_file_metadata`

---

## Guardrails

**See box-legal-shared for:** Human-in-the-loop requirements, confidentiality, Box AI governance.

**M&A-specific:**

**ALWAYS confirm before:**
1. External access grants (any external party)
2. Creating shared links (especially Open)
3. Creating initial folder structure (default template vs. custom)
4. Copying files to externally-accessible folders (if file was NOT already externally accessible)

**CONFIRM if uncertain:**
5. Risk assessment decisions (if factors are ambiguous)
6. Metadata field selection (if unclear which fields are needed)
7. File categorization (if document type is unclear)

**Proceed autonomously when confident:**
- Copying/organizing files between internal folders (internal-only to internal-only, or external to external)
- Writing metadata when extraction is clear
- Categorizing documents by type when obvious
- Extracting terms from financial or legal documents with high confidence
- Creating due diligence summary reports

**Default behaviors:**
- Default to Viewer for external parties
- Prefer folder-specific over root access
- Validate with `list_item_collaborations` before external links
- Pace Box AI calls 1-2 seconds apart
- Surface citations from Box AI

---

## Example Workflows

### Example 1: Create Deal Room + Internal Access
**Request:** "Create M&A deal room for Project Thunder, give Sarah editor access, John viewer access."

**Flow:**
1. `who_am_i`
2. **[CONFIRM]**: "Standard M&A structure? Customize?"
3. `create_folder` (root + all subfolders)
4. **[CONFIRM]**: "Grant Sarah Editor, John Viewer on root?"
5. `create_collaboration` (both)
6. `list_item_collaborations` (verify)
7. Report: folder IDs, access summary

### Example 2: External Counsel Upload Access
**Request:** "Give external law firm upload access to Legal Documents."

**Flow:**
1. **[CONFIRM]**: "Email? Upload-Only or Editor? Expiration? Which folder?"
2. `list_item_collaborations` (audit current)
3. `create_collaboration` (with confirmed params)
4. Verify and report

### Example 3: DD Question
**Request:** "What was EBITDA for last 3 years?"

**Flow:**
1. `search_files_keyword` ("EBITDA", Financial Statements folder)
2. Identify annual reports (last 3 years)
3. `ai_qa_multi_file` (specific question)
4. Present answer with citations
5. Offer additional metrics extraction

### Example 4: Permission Audit
**Request:** "Check who has access before sharing with buyer's team."

**Flow:**
1. `list_item_collaborations` (root)
2. Categorize internal vs. external
3. Present audit report
4. **[CONFIRM]**: "Buyer's team access: folders? permission? expiration?"
5. Wait for confirmation

---

## See Also
- `box:box` — MCP auth, tool selection, base Box workflows
- `box-legal-shared` — Risk frameworks, confidentiality, human-in-the-loop patterns
