SOLN GPBC — Clement-Sutton University
Product Requirements Document: GitHub Actions Recovery
Repository: DHCSOLN/After-Revaltions-12-Week-Course-
Branch: main
Affected workflows: .github/workflows/main.yml and Setup Clement-Sutton University
Incident evidence: Eight failed runs shown in GitHub Actions; the underlying step logs have not been supplied.
Priority: P0 — restore reliable, auditable automation without changing existing course materials.
1. Objective
Restore successful execution of the Clement-Sutton University setup workflow and isolate failures in main.yml. Preserve the original After Revelations 12-week course, all existing university content, charter, Operational Bible, funding records, and Git history. A workflow run must not imply institutional accreditation, authorization, or receipt of funds.
2. Observed facts and diagnostic limits
- The displayed Actions page shows repeated failures for two workflows after commits to main.
- The exact failing steps, error messages, workflow contents, and repository settings are not in the supplied screenshot text.
- Do not claim that YAML syntax, permissions, or a particular Git command caused the failures until the failed step log confirms it.
3. Functional requirements
ID	Requirement	Acceptance evidence
FR-01	Run a manual, non-destructive setup workflow	workflow_dispatch appears in Actions
FR-02	Create missing university directories	All listed directories exist with tracked README placeholders only when necessary
FR-03	Preserve existing university and 12-week course files	Existing files unchanged after run
FR-04	Write a substantive root README and PRD only if absent	Newly initialized repo gets full content; existing documents are never overwritten
FR-05	Avoid auto-trigger loops	Setup workflow only runs on manual dispatch
FR-06	Report changes before commit	git status and diff output visible in Actions logs
FR-07	Handle no-op runs	Second run exits successfully with no new commit
FR-08	Support GitHub token permissions	Explicit contents: write; report branch protection restrictions rather than bypass them
FR-09	Preserve institution's affirmative wording	No automatic addition of unwanted institutional status text
FR-10	Separate existing main.yml diagnosis	No blind replacement of main.yml without reviewing its actual content and failure logs


4. Nonfunctional requirements
- Least-privilege GitHub Actions token; no third-party secrets in output.
- Bash strict mode (set -euo pipefail), pinned stable major action actions/checkout@v4.
- Idempotent execution and deterministic file creation.
- Only stage clement-sutton-university/.
- No deletion, moving, or rewriting existing source files.
5. Implementation plan
1. In Actions, open the most recent failed run of Setup Clement-Sutton University, then the red job and step. Record the complete error line.
2. Inspect .github/workflows/main.yml independently. Its failures may have a different cause; do not replace it with the setup workflow.
3. Add the supplied .github/workflows/setup-clement-sutton-university.yml to the repository, replacing only the old setup workflow after confirming its filename.
4. In repository Settings → Actions → General → Workflow permissions, ensure the workflow is permitted to write repository contents if direct push is intended. If branch rules block pushing to main, use a reviewed pull request instead.
5. Trigger the setup workflow manually and verify its logs and resulting file tree.
6. Trigger it again; confirm a clean no-op success.
7. Diagnose main.yml from its own failed step and correct only the verified cause.
8. Upload approved charter, Operational Bible, and funding files separately; the setup workflow does not fabricate or replace them.
6. Success criteria
- Setup workflow completes with a green check.
- The clement-sutton-university/ tree contains root README, PRD, and required directories.
- Original 12-week course files remain at their existing paths, not a newly invented existing-12-week-course/ folder.
- Repeated setup runs succeed without changes.
- main.yml is either independently repaired and green or has a documented error and separate remediation task.
7. Rollback
Revert only the setup workflow commit or newly created placeholder files after reviewing the diff. Do not reset branch history or remove preexisting documents.
8. Open evidence needed
The failing job/step text for .github/workflows/main.yml and for Setup Clement-Sutton University; current contents of both workflow files; whether main has branch protection/rulesets. These determine the final root-cause fix.
