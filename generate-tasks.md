# Multi-Repository YAML Patching Workflow

  Generate a plan to patch YAML files across multiple repositories with the following workflow:

  ## Step 0: Initial Information Gathering
  **The AI MUST ask the user for ONLY the following information:**
  1. **Target filename**: What is the name of the YAML file to modify? (e.g., `settings.yaml`,
  `config.yaml`)

  **That's it. The AI will discover everything else automatically.**

  ## Step 1: Discovery and Plan Generation
  **After receiving the filename, the AI MUST:**

  1. [ ] Search all checked-out repositories for the target file `[FILENAME]`
  2. [ ] Collect all locations where `[FILENAME]` exists
  3. [ ] Build a comprehensive list of:
     - Repository paths
     - Full file paths
     - Number of instances found
  4. [ ] Present the discovered locations to the user for confirmation
  5. [ ] Generate a dynamic execution plan based on discovered locations

  **Example Discovery Output:**
  Found [FILENAME] in the following locations:
  1. /path/to/repo1/config/settings.yaml
  2. /path/to/repo2/app/settings.yaml
  3. /path/to/repo3/settings.yaml

  Total: 3 instances across 3 repositories

  Proceeding with plan generation...

  ## Step 2: Auto-Generate Required Parameters
  **The AI MUST automatically generate/determine:**

  1. **Feature branch name**: Generate using format `feature/yaml-patch-[filename]-[date]`
     - Example: `feature/yaml-patch-settings-20250127`
  2. **Base branch**: Detect from current git branch or use `main`/`master`
  3. **Validation script**: Look for validation scripts in repository (e.g.,
  `scripts/validate-yaml.sh`, `validate.py`)
  4. **Commit message**: Generate based on changes: `chore: update [filename] configuration`
  5. **PR target branch**: Use detected base branch
  6. **PR title**: Generate as `Update [filename] across repositories`
  7. **PR description**: Auto-generate based on changes made

  ## Step 3: Present Plan for User Approval
  **Before executing, the AI MUST present the complete plan:**

  Execution Plan:

  Target file: [FILENAME]
  Instances found: [N]

  For each repository:
  1. Repository: [REPO_NAME]
    - File location: [FILE_PATH]
    - Base branch: [AUTO_DETECTED]
    - Feature branch: [AUTO_GENERATED]
    - Changes to apply: [USER_SPECIFIED_OR_DETECTED]

  Actions per repository:
  [ ] 1. Checkout base branch
  [ ] 2. Create feature branch
  [ ] 3. Modify YAML file
  [ ] 4. Validate changes (loop until pass)
  [ ] 5. Commit changes
  [ ] 6. Push to remote
  [ ] 7. Create pull request

  Proceed with execution? (The AI will complete all steps automatically)

  ## Critical Completion Requirement
  **The AI MUST NOT terminate its turn until ALL of the following are complete for EVERY discovered
  instance:**
  - ✓ All validation passes
  - ✓ All changes committed
  - ✓ All pull requests created

  **No partial completion is acceptable. The workflow continues until 100% complete.**

  ## Workflow Execution Steps

  ### Phase 1: Repository Setup
  For each repository containing the target file:
  1. [ ] Checkout base branch `[AUTO_DETECTED]`
  2. [ ] Create new feature branch `[AUTO_GENERATED]`
  3. [ ] Log the branch names for tracking

  ### Phase 2: File Modification with Validation Feedback Loop
  For each discovered file instance:
  4. [ ] Apply YAML patches to `[FILE_PATH]`
  5. [ ] **VALIDATION FEEDBACK LOOP** (repeat until validation passes):

     a. [ ] Run validation (auto-detect validation method):
        - If validation script exists: run it
        - Otherwise: use YAML syntax validation
        - Verify:
          * File is valid YAML syntax
          * New keys and values are added to the correct locations
          * No unintended changes were made (focused, minimal changes only)

     b. [ ] If validation **PASSES**:
        - Mark this file as complete `[x]`
        - Exit the feedback loop for this file
        - Proceed to Phase 3 for this repository

     c. [ ] If validation **FAILS**:
        - Analyze the validation error message
        - Determine the root cause (syntax error, wrong location, incorrect indentation, etc.)
        - Apply fix to address the specific error
        - **Return to step 5.a** (re-validate)
        - **Continue looping indefinitely until validation passes**

     d. [ ] Track and log each iteration:
        - Attempt #N: [Error] → [Fix Applied] → [Result]

  ### Phase 3: Commit and Deploy
  For each repository with validated changes:
  6. [ ] Commit changes with auto-generated message: `chore: update [filename] configuration`
  7. [ ] Push feature branch to remote repository
  8. [ ] Create Bitbucket pull request with:
     - Title: `Update [filename] across repositories`
     - Description: Auto-generated summary of changes
     - Target: `[AUTO_DETECTED_BASE_BRANCH]`
  9. [ ] Mark repository as fully complete `[x]`

  ### Phase 4: Final Verification
  10. [ ] Verify ALL discovered instances have:
      - Passing validation `[x]`
      - Committed changes `[x]`
      - Created pull requests `[x]`
  11. [ ] Generate summary report showing completion status of all repositories
  12. [ ] **Only terminate after confirming 100% completion**

  ## Constraints and Requirements

  ### MUST (Required)
  - ✓ **Ask user ONLY for the target filename** - nothing else
  - ✓ **Search and discover ALL locations** where the filename exists
  - ✓ **Build a dynamic plan** based on discovered locations
  - ✓ **Auto-generate all other parameters** (branches, commit messages, PR details)
  - ✓ Present the plan to the user before execution
  - ✓ **Complete ALL discovered instances before terminating** - no partial work
  - ✓ Validate changes before moving to the next step
  - ✓ Only make changes explicitly requested by the user
  - ✓ Always create a new feature branch (never reuse existing branches)
  - ✓ **Always fix validation errors in a feedback loop** (validate → fix → repeat)
  - ✓ Continue the feedback loop until validation passes for EVERY file
  - ✓ Create pull requests for EVERY repository with changes
  - ✓ Log each validation attempt with errors and fixes applied
  - ✓ Mark completed tasks with `[x]`
  - ✓ Number all tasks with checkboxes `[ ]`
  - ✓ **Persist until 100% completion across all discovered instances**

  ### MUST NOT (Prohibited)
  - ✗ Ask the user for information other than the filename
  - ✗ Skip the discovery phase
  - ✗ Execute without presenting the plan first
  - ✗ Make assumptions about which repositories to process (discover them all)
  - ✗ Make changes outside the specified feature scope
  - ✗ Skip or bypass validation failures
  - ✗ Terminate before ALL repositories are complete
  - ✗ Terminate before ALL pull requests are created
  - ✗ Leave any repository in a partial state
  - ✗ Modify files other than the specified target file
  - ✗ Proceed to commit/push without passing validation
  - ✗ Stop working if some repositories succeed but others haven't completed

  ### Auto-Generation Guidelines

  **Feature Branch Naming:**
  - Format: `feature/yaml-patch-[filename-without-extension]-[YYYYMMDD]`
  - Example: `feature/yaml-patch-settings-20250127`

  **Base Branch Detection:**
  - Check current branch with `git branch --show-current`
  - If on feature branch, find parent branch
  - Default to `main` or `master` if uncertain

  **Validation Script Detection:**
  - Search for common validation script patterns:
    - `scripts/validate-yaml.sh`
    - `scripts/validate.py`
    - `.github/scripts/validate-yaml`
    - `tools/validate-yaml`
  - If none found, use built-in YAML syntax validation

  **Commit Message Template:**
  - `chore: update [filename] configuration`
  - Or based on changes: `feat: add [keys] to [filename]`

  **PR Title Template:**
  - `Update [filename] across repositories`
  - Or: `chore: standardize [filename] configuration`

  ### Feedback Loop Behavior
  - **Validation errors ALWAYS trigger fixes** - never give up
  - **The loop continues indefinitely** until validation passes
  - **Each iteration must**:
    1. Identify the specific validation error
    2. Apply a targeted fix
    3. Re-validate immediately
    4. Log the attempt with details
  - **Exit condition**: Only when validation script returns success for ALL files
  - **No early termination**: Continue until every repository has a created PR

  ### Completion Tracking
  Track progress across all discovered instances:
  Discovery Results:

  Target filename: [FILENAME]
  Total instances: [N]

  Repository 1: [REPO_NAME]
  - File path: [FILE_PATH]
  - Feature branch: [AUTO_GENERATED]
  - Setup: [x] / [ ]
  - Modification: [x] / [ ]
  - Validation passed: [x] / [ ]
  - Committed: [x] / [ ]
  - PR created: [x] / [ ]

  Repository 2: [REPO_NAME]
  - File path: [FILE_PATH]
  - Feature branch: [AUTO_GENERATED]
  - Setup: [x] / [ ]
  - Modification: [x] / [ ]
  - Validation passed: [x] / [ ]
  - Committed: [x] / [ ]
  - PR created: [x] / [ ]

  ...

  Overall Progress: [N/M] repositories complete
  Status: [IN_PROGRESS / COMPLETE]

  ### Error Reporting Format
  For each validation attempt, log
  Repository: [REPO_NAME]
  File: [FILE_PATH]
  Attempt: #[N]
  Error: [VALIDATION_ERROR_MESSAGE]
  Fix Applied: [DESCRIPTION_OF_FIX]
  Result: [PASS/FAIL]

  **INITIALIZATION RULE**:
  1. Ask user ONLY for filename
  2. Discover all instances
  3. Generate all other parameters automatically
  4. Present complete plan
  5. Execute upon approval

  **TERMINATION RULE**: The AI's turn ends ONLY when all discovered instances show `[x]` for PR
  created.