# My Project README

## Branching Strategy

Here is a visual representation of our branching strategy, which uses integration branches for QC validation before merging features to main:

```mermaid
gitGraph
    commit id: "Initial Commit"
    %% --- Release 1.0 Development ---
    branch integration/release-1.0
    checkout integration/release-1.0
    commit id: "Start Integ 1.0" %% Base for Release 1.0 features

    checkout main
    branch feature/A-for-1.0
    checkout feature/A-for-1.0
    commit id: "Dev A1"
    commit id: "Dev A2"

    %% Feature A merged to Integration branch for QC
    checkout integration/release-1.0
    merge feature/A-for-1.0 id:"QC A on Integ 1.0"

    %% --- Release 1.1 Development (overlaps) ---
    checkout main
    commit id: "Some other work on main"
    branch integration/release-1.1
    checkout integration/release-1.1
    commit id: "Start Integ 1.1" %% Base for Release 1.1 features

    checkout main
    branch feature/B-for-1.1
    checkout feature/B-for-1.1
    commit id: "Dev B1"

    %% Feature B merged to Integration branch for QC
    checkout integration/release-1.1
    merge feature/B-for-1.1 id:"QC B on Integ 1.1"

    %% --- Feature A is validated in QC (on integ/1.0), now merge original feature to main ---
    checkout main
    merge feature/A-for-1.0 id:"Feat A merged to main"

    %% --- Prepare Release 1.0.0 for PreProd ---
    branch release/v1.0.0
    checkout release/v1.0.0
    commit id: "Stabilize 1.0" %% PreProd Testing / Bug Fixes for 1.0 only
    %% Conceptually, release/v1.0.0 is deployed to Prod

    %% --- Tag main after successful v1.0.0 deployment ---
    checkout main
    tag "v1.0.0"

    %% --- Feature B is validated in QC (on integ/1.1), now merge original feature to main ---
    checkout main  %% Ensure we are on main before merge
    merge feature/B-for-1.1 id:"Feat B merged to main"

    %% --- Hotfix for v1.0.0 ---
    checkout v1.0.0 %% Go to the production tag on main
    branch hotfix/v1.0.1-bugfix
    checkout hotfix/v1.0.1-bugfix
    commit id:"Fix critical bug"

    %% Merge hotfix back to main
    checkout main
    merge hotfix/v1.0.1-bugfix id:"Hotfix merged to main"
    checkout main  %% Explicitly checkout main again before tagging
    tag "v1.0.1"

    %% Cherry-pick hotfix onto active integration branch for 1.1
    checkout integration/release-1.1
    cherry-pick id:"Fix critical bug"

    %% (Optional: Cherry-pick hotfix onto an active release branch if v1.1.0 was already cut)
    %% Example:
    %% checkout main
    %% branch release/v1.1.0
    %% checkout release/v1.1.0
    %% cherry-pick id:"Fix critical bug"
```

## Other Sections

... rest of your README content ...
