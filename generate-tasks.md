# Multi-Repository YAML Patching Workflow

  Generate a plan to patch YAML files across multiple repositories with the following workflow:

  ## Step 0: Gather Required Information
  **BEFORE starting the workflow, the AI MUST ask the user to provide values for ALL placeholders:**

  Ask the user for:
  1. **Base branch name**: What is the base branch to checkout? (e.g., `main`, `develop`)
  2. **Feature branch name**: What should the new feature branch be called? (e.g.,
  `feature/yaml-update`)
  3. **Target file path**: What is the path to the YAML file to modify? (e.g.,
  `config/settings.yaml`)
  4. **Validation script path**: What is the path to the validation script? (e.g.,
  `scripts/validate-yaml.sh`)
  5. **YAML changes**: What specific changes should be made to the YAML file? (provide keys and
  values)
  6. **Commit message**: What should the commit message be? (e.g., `chore: update YAML
  configuration`)
  7. **PR target branch**: What branch should the pull request target? (e.g., `main`)
  8. **PR title**: What should the pull request title be?
  9. **PR description**: What should the pull request description include?

  **DO NOT proceed with the workflow until ALL values are provided by the user.**

  ## Prerequisites (to be filled from user input)
  - **Base branch**: `[BRANCH_NAME]` ← User provides
  - **Feature branch**: `[FEATURE_BRANCH_NAME]` ← User provides
  - **Target file**: `[FILE_PATH]` ← User provides
  - **Validation script**: `[VALIDATION_SCRIPT_PATH]` ← User provides
  - **YAML changes**: `[SPECIFY_CHANGES]` ← User provides
  - **Commit message**: `[COMMIT_MESSAGE]` ← User provides
  - **PR target branch**: `[TARGET_BRANCH]` ← User provides
  - **PR title**: `[PR_TITLE]` ← User provides
  - **PR description**: `[PR_DESCRIPTION]` ← User provides

  ## Critical Completion Requirement
  **The AI MUST NOT terminate its turn until ALL of the following are complete for EVERY
  repository:**
  - ✓ All validation passes
  - ✓ All changes committed
  - ✓ All pull requests created

  **No partial completion is acceptable. The workflow continues until 100% complete.**

  ## Workflow Steps

  ### Phase 1: Repository Setup
  For each checked-out repository:
  1. [ ] Checkout base branch `[BRANCH_NAME]`
  2. [ ] Create new feature branch `[FEATURE_BRANCH_NAME]`

  ### Phase 2: File Discovery
  3. [ ] Locate target file `[FILE_PATH]` in each repository

  ### Phase 3: File Modification with Validation Feedback Loop
  For each located file:
  4. [ ] Apply YAML patches as specified: `[SPECIFY_CHANGES]`
  5. [ ] **VALIDATION FEEDBACK LOOP** (repeat until validation passes):

     a. [ ] Run validation script `[VALIDATION_SCRIPT_PATH]` to verify:
        - File is valid YAML syntax
        - New keys and values are added to the correct locations
        - No unintended changes were made (focused, minimal changes only)

     b. [ ] If validation **PASSES**:
        - Mark this file as complete `[x]`
        - Exit the feedback loop for this file
        - Proceed to Phase 4 for this repository

     c. [ ] If validation **FAILS**:
        - Analyze the validation error message
        - Determine the root cause (syntax error, wrong location, incorrect indentation, etc.)
        - Apply fix to address the specific error
        - **Return to step 5.a** (re-validate)
        - **Continue looping indefinitely until validation passes**

     d. [ ] Track and log each iteration:
        - Attempt #N: [Error] → [Fix Applied] → [Result]

  ### Phase 4: Commit and Deploy
  For each repository with validated changes:
  6. [ ] Commit changes with message: `[COMMIT_MESSAGE]`
  7. [ ] Push feature branch to remote repository
  8. [ ] Create Bitbucket pull request targeting `[TARGET_BRANCH]` with:
     - Title: `[PR_TITLE]`
     - Description: `[PR_DESCRIPTION]`
  9. [ ] Mark repository as fully complete `[x]`

  ### Phase 5: Final Verification
  10. [ ] Verify ALL repositories have:
      - Passing validation `[x]`
      - Committed changes `[x]`
      - Created pull requests `[x]`
  11. [ ] Generate summary report showing completion status of all repositories
  12. [ ] **Only terminate after confirming 100% completion**

  ## Constraints and Requirements

  ### MUST (Required)
  - ✓ **Ask user for ALL placeholder values BEFORE starting** (Step 0)
  - ✓ **Complete ALL repositories before terminating** - no partial work
  - ✓ Validate changes before moving to the next step
  - ✓ Only make changes explicitly requested by the user
  - ✓ Always create a new feature branch (never reuse existing branches)
  - ✓ **Always fix validation errors in a feedback loop** (validate → fix → repeat)
  - ✓ Continue the feedback loop until validation passes for EVERY file
  - ✓ Create pull requests for EVERY repository with changes
  - ✓ Log each validation attempt with errors and fixes applied
  - ✓ Mark completed tasks with `[x]`
  - ✓ Number all tasks with checkboxes `[ ]`
  - ✓ **Persist until 100% completion across all repositories**

  ### MUST NOT (Prohibited)
  - ✗ Start the workflow without collecting all placeholder values from the user
  - ✗ Make assumptions about placeholder values
  - ✗ Make changes outside the specified feature scope
  - ✗ Skip or bypass validation failures
  - ✗ Terminate before ALL repositories are complete
  - ✗ Terminate before ALL pull requests are created
  - ✗ Leave any repository in a partial state
  - ✗ Modify files other than the specified target file
  - ✗ Proceed to commit/push without passing validation
  - ✗ Stop working if some repositories succeed but others haven't completed

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
  Track progress across all repositories:
  Repository 1: [REPO_NAME]
  - Setup: [x] / [ ]
  - File found: [x] / [ ]
  - Validation passed: [x] / [ ]
  - Committed: [x] / [ ]
  - PR created: [x] / [ ]

  Repository 2: [REPO_NAME]
  - Setup: [x] / [ ]
  - File found: [x] / [ ]
  - Validation passed: [x] / [ ]
  - Committed: [x] / [ ]
  - PR created: [x] / [ ]

  ...

  Overall Progress: [N/M] repositories complete
  Status: [IN_PROGRESS / COMPLETE]

  ### Error Reporting Format
  For each validation attempt, log:
  Repository: [REPO_NAME]
  File: [FILE_PATH]
  Attempt: #[N]
  Error: [VALIDATION_ERROR_MESSAGE]
  Fix Applied: [DESCRIPTION_OF_FIX]
  Result: [PASS/FAIL]

  **INITIALIZATION RULE**: AI must ask for all placeholder values first.
  **TERMINATION RULE**: The AI's turn ends ONLY when all repositories show `[x]` for PR created.

> Feature branch name should generated by the AI agent

⏺ Issues

  1. Spelling errors:
    - "VALIDATIOn-SCRIPT" has inconsistent capitalization
    - "corect" should be "correct"
    - "Foe" should be "For"
    - "CReate" has inconsistent capitalization
    - "there" should be "their"
  2. Ambiguous placeholders: The prompt uses placeholder syntax [[BRANCH]], [[FEATURE-BRANCH]],
  [[FILE]], [[VALIDATION-SCRIPT]] but doesn't explain what these represent or how they should be
  filled in.
    - Missing user input mechanism: The prompt doesn't instruct the AI to ask the user for
  placeholder values before starting
    - Feature branch naming not specified: No instruction that AI should generate the feature branch
  name
  3. Unclear constraint language: The "Constraints" section uses terms like "ALLOWS" and "SHOULD NOT"
   inconsistently:
    - "ALLOWS validate changes" is grammatically incorrect
    - "allows create one" is grammatically incorrect
    - The meaning of "ALLOWS" vs "SHOULD NOT" vs "NEVER" hierarchy is unclear
  4. Contradictory constraints:
    - "ALLOWS CReate new feature, and NEVER use existing one" contradicts "SHOULD NOT Make changes
  outside feature, and allows create one"
    - The phrasing suggests both creating and not creating features
  5. Missing details:
    - What should the commit message be?
    - What should the pull request title/description be?
    - What branch should the PR target?
    - No guidelines for AI-generated feature branch naming convention
  6. Inconsistent structure: Step 4 breaks the pattern of steps 1-3 by referring to "cloned
  repository" instead of maintaining consistent terminology.
  7. Unclear validation scope: "Nothing else changed" in step 3.b is vague - does this mean no
  whitespace changes, no comment changes, etc.?
  8. Missing initialization step: No step 0 or prerequisite gathering phase where user provides all
  necessary values.

  Improved Prompt

  # Multi-Repository YAML Patching Workflow

  Generate a plan to patch YAML files across multiple repositories with the following workflow:

  ## Step 0: Gather Required Information
  **BEFORE starting the workflow, the AI MUST:**

  1. **Ask the user to provide values for the following:**
     - **Base branch name**: What is the base branch to checkout? (e.g., `main`, `develop`)
     - **Target file path**: What is the path to the YAML file to modify? (e.g.,
  `config/settings.yaml`)
     - **Validation script path**: What is the path to the validation script? (e.g.,
  `scripts/validate-yaml.sh`)
     - **YAML changes**: What specific changes should be made to the YAML file? (provide keys and
  values)
     - **Commit message**: What should the commit message be? (e.g., `chore: update YAML
  configuration`)
     - **PR target branch**: What branch should the pull request target? (e.g., `main`)
     - **PR title**: What should the pull request title be?
     - **PR description**: What should the pull request description include?

  2. **Generate the feature branch name automatically:**
     - Use a descriptive naming convention based on the task
     - Format: `feature/[descriptive-name]-[timestamp]` or `feature/yaml-patch-[date]`
     - Example: `feature/yaml-config-update-20250127` or `feature/add-monitoring-keys`
     - Ensure the name is unique and meaningful

  **DO NOT proceed with the workflow until ALL user-provided values are collected AND the feature
  branch name is generated.**

  ## Prerequisites (to be filled from user input and AI generation)
  - **Base branch**: `[BRANCH_NAME]` ← User provides
  - **Feature branch**: `[FEATURE_BRANCH_NAME]` ← **AI generates**
  - **Target file**: `[FILE_PATH]` ← User provides
  - **Validation script**: `[VALIDATION_SCRIPT_PATH]` ← User provides
  - **YAML changes**: `[SPECIFY_CHANGES]` ← User provides
  - **Commit message**: `[COMMIT_MESSAGE]` ← User provides
  - **PR target branch**: `[TARGET_BRANCH]` ← User provides
  - **PR title**: `[PR_TITLE]` ← User provides
  - **PR description**: `[PR_DESCRIPTION]` ← User provides

  ## Critical Completion Requirement
  **The AI MUST NOT terminate its turn until ALL of the following are complete for EVERY
  repository:**
  - ✓ All validation passes
  - ✓ All changes committed
  - ✓ All pull requests created

  **No partial completion is acceptable. The workflow continues until 100% complete.**

  ## Workflow Steps

  ### Phase 1: Repository Setup
  For each checked-out repository:
  1. [ ] Checkout base branch `[BRANCH_NAME]`
  2. [ ] Create new feature branch `[FEATURE_BRANCH_NAME]` (AI-generated name)
  3. [ ] Log the generated feature branch name for tracking

  ### Phase 2: File Discovery
  4. [ ] Locate target file `[FILE_PATH]` in each repository

  ### Phase 3: File Modification with Validation Feedback Loop
  For each located file:
  5. [ ] Apply YAML patches as specified: `[SPECIFY_CHANGES]`
  6. [ ] **VALIDATION FEEDBACK LOOP** (repeat until validation passes):

     a. [ ] Run validation script `[VALIDATION_SCRIPT_PATH]` to verify:
        - File is valid YAML syntax
        - New keys and values are added to the correct locations
        - No unintended changes were made (focused, minimal changes only)

     b. [ ] If validation **PASSES**:
        - Mark this file as complete `[x]`
        - Exit the feedback loop for this file
        - Proceed to Phase 4 for this repository

     c. [ ] If validation **FAILS**:
        - Analyze the validation error message
        - Determine the root cause (syntax error, wrong location, incorrect indentation, etc.)
        - Apply fix to address the specific error
        - **Return to step 6.a** (re-validate)
        - **Continue looping indefinitely until validation passes**

     d. [ ] Track and log each iteration:
        - Attempt #N: [Error] → [Fix Applied] → [Result]

  ### Phase 4: Commit and Deploy
  For each repository with validated changes:
  7. [ ] Commit changes with message: `[COMMIT_MESSAGE]`
  8. [ ] Push feature branch to remote repository
  9. [ ] Create Bitbucket pull request targeting `[TARGET_BRANCH]` with:
     - Title: `[PR_TITLE]`
     - Description: `[PR_DESCRIPTION]`
  10. [ ] Mark repository as fully complete `[x]`

  ### Phase 5: Final Verification
  11. [ ] Verify ALL repositories have:
      - Passing validation `[x]`
      - Committed changes `[x]`
      - Created pull requests `[x]`
  12. [ ] Generate summary report showing completion status of all repositories
  13. [ ] **Only terminate after confirming 100% completion**

  ## Constraints and Requirements

  ### MUST (Required)
  - ✓ **Ask user for ALL required values BEFORE starting** (Step 0)
  - ✓ **AI must generate the feature branch name** - do not ask user for it
  - ✓ Use a descriptive, unique feature branch naming convention
  - ✓ **Complete ALL repositories before terminating** - no partial work
  - ✓ Validate changes before moving to the next step
  - ✓ Only make changes explicitly requested by the user
  - ✓ Always create a new feature branch (never reuse existing branches)
  - ✓ **Always fix validation errors in a feedback loop** (validate → fix → repeat)
  - ✓ Continue the feedback loop until validation passes for EVERY file
  - ✓ Create pull requests for EVERY repository with changes
  - ✓ Log each validation attempt with errors and fixes applied
  - ✓ Mark completed tasks with `[x]`
  - ✓ Number all tasks with checkboxes `[ ]`
  - ✓ **Persist until 100% completion across all repositories**

  ### MUST NOT (Prohibited)
  - ✗ Start the workflow without collecting all required values from the user
  - ✗ Ask the user for the feature branch name (AI generates this)
  - ✗ Make assumptions about user-provided placeholder values
  - ✗ Make changes outside the specified feature scope
  - ✗ Skip or bypass validation failures
  - ✗ Terminate before ALL repositories are complete
  - ✗ Terminate before ALL pull requests are created
  - ✗ Leave any repository in a partial state
  - ✗ Modify files other than the specified target file
  - ✗ Proceed to commit/push without passing validation
  - ✗ Stop working if some repositories succeed but others haven't completed

  ### Feature Branch Naming Guidelines
  - **Format options**:
    - `feature/[task-description]-[date]` (e.g., `feature/yaml-config-update-20250127`)
    - `feature/[task-description]-[timestamp]` (e.g., `feature/add-monitoring-1706345600`)
    - `feature/[short-description]` (e.g., `feature/yaml-patch`)
  - **Requirements**:
    - Must be unique per execution
    - Must be descriptive of the change
    - Use kebab-case (lowercase with hyphens)
    - Keep it concise but meaningful

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
  Track progress across all repositories:
  Repository 1: [REPO_NAME]
  - Feature branch: [AI_GENERATED_BRANCH_NAME]
  - Setup: [x] / [ ]
  - File found: [x] / [ ]
  - Validation passed: [x] / [ ]
  - Committed: [x] / [ ]
  - PR created: [x] / [ ]

  Repository 2: [REPO_NAME]
  - Feature branch: [AI_GENERATED_BRANCH_NAME]
  - Setup: [x] / [ ]
  - File found: [x] / [ ]
  - Validation passed: [x] / [ ]
  - Committed: [x] / [ ]
  - PR created: [x] / [ ]

  ...

  Overall Progress: [N/M] repositories complete
  Status: [IN_PROGRESS / COMPLETE]

  ### Error Reporting Format
  For each validation attempt, log:
  Repository: [REPO_NAME]
  File: [FILE_PATH]
  Attempt: #[N]
  Error: [VALIDATION_ERROR_MESSAGE]
  Fix Applied: [DESCRIPTION_OF_FIX]
  Result: [PASS/FAIL]

  **INITIALIZATION RULE**: AI must ask for all user-required values first, then generate feature
  branch name.
  **TERMINATION RULE**: The AI's turn ends ONLY when all repositories show `[x]` for PR created.