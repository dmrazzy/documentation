# Release Information Architecture Proposal

## Context
The current release listing model has become difficult to navigate as entries grow across both MOSIP platform releases and independent component releases.

## Current Issues

1. Single mixed timeline for different release streams
- Platform and component releases appear together, making it hard to identify relevance quickly.

2. Scale and scanability problem
- The release list has grown long enough that users struggle to locate a specific release quickly.

3. Naming and linkage inconsistency
- Mixed naming styles, occasional typos, and mixed relative/absolute linking patterns create ambiguity.

4. Duplicate index maintenance
- Both `summary.md` and releases index pages act as master lists, increasing drift and maintenance errors.

5. Ambiguity in applicability
- Users cannot easily determine whether an item applies to MOSIP platform, a specific component, or both.

## Why Users Feel Overwhelmed

1. Too much mixed information in one list
2. No first-class route by component
3. Hard to compare what changed and whether migration is needed
4. No clear support status framing (for example LTS, stable, beta, patch)

## How Leading Product Docs Typically Handle This

1. Separate release streams first
- Platform releases and component releases are indexed separately.

2. Keep a unified feed as secondary
- One combined feed may exist, but it is not the primary navigation surface.

3. Add filterable metadata
- Filters typically include product/component, release type, date range, support status, and breaking-change indicator.

4. Include "what changed for me" summaries
- Highlight breaking changes, upgrade actions, migration steps, and security items.

5. Generate indexes from structured metadata
- Avoid hand-maintained long lists where ordering and links drift.

## Proposed Solution for MOSIP Docs

### 1) Two-layer release IA

Layer 1: Release Hub
- Clear entry cards:
  - MOSIP Platform Releases
  - Component Releases
  - All Releases (combined)

Layer 2: Stream pages
- Platform Releases page (latest first)
- Component Releases page (latest first)
- Optional per-component pages (Android RC, DSL, API Test Commons, PMS, Resident Services, and others)

### 2) Metadata-driven release records

Use structured release metadata fields (YAML frontmatter or manifest), such as:
- `release_date`
- `stream` (platform/component)
- `component_name`
- `version`
- `release_type` (major/minor/patch/beta/dp)
- `support_status`
- `breaking_changes` (true/false)
- `upgrade_from` (optional)

### 3) Standardized release summary blocks

Each release page should include:
- Scope: platform/component
- Compatibility / target audience
- Breaking changes
- Migration steps (if needed)
- Key enhancements and bug fixes
- Test report links

### 4) Navigation simplification

In `summary.md`:
- Keep only a concise "Latest Releases" section for each stream
- Add "View all" links to dedicated stream index pages
- Avoid maintaining one very long mixed chronological list

## Suggested Rollout Plan

### Phase 1 (Quick Win)

1. Split current release list into:
- Platform Releases
- Component Releases

2. Show latest 10 in each list
3. Add "View all" links to stream pages

### Phase 2

1. Create component-specific release index pages
2. Standardize release page headers and summary blocks

### Phase 3

1. Introduce metadata manifest and auto-generated index pages
2. Remove manual ordering burden from `summary.md`

### Phase 4

1. Add release comparison and filter views
2. Add breaking-changes-only and migration-required views

## Expected Outcomes

1. Faster release discovery
2. Reduced ambiguity about applicability
3. Better upgrade decision-making
4. Lower maintenance overhead and fewer broken/misaligned links

## Notes for Implementation

- Preserve existing release page URLs to avoid link breakage.
- Introduce new index pages and progressively redirect navigation there.
- Treat the combined feed as secondary; stream-first navigation should be the default.
