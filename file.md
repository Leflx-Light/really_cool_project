### 1. Safe Posix Platform:

```rust
git clone --recurse-submodules -j8 git@cc-github.bmwgroup.net:swh/safe-posix-platform.git
```

### 2. Bazel test:

```rust
bazel test --config=spp_host_gcc --test_output=all --test_arg=--gtest_print_time=1 --test_arg=--gtest_color=yes --test_arg=--gtest_brief=0 //platform/aas/mw/log/detail:composite_recorder_test
```

### 3. Bazel

```rust
bazel build --config=spp_host_gcc --verbose_failures --sandbox_debug --show_progress --progress_report_interval=1 //platform/aas/mw/log/detail:composite_recorder_test
```

### 4. Bazel Run:

```rust
bazel run --python=3.8 //platform/aas/coverage:host -- -t "//platform/aas/sysfunc/BasicSecurity/"
```

### 5. Bazel Builder-Fix:

```rust
bazel run --python=3.8 //bazel/tools/buildifier:fix
bazel run //platform/aas:clang-format -- -f && bazel run --python=3.8 //bazel/tools/buildifier:fix
```

**Clang format check : Run following command to format clang**

```rust
bazel run //quality/clang-format -- -f 
```

**Format Source Code**

```rust
bazel run //platform/aas:clang-format -- -f 
```

**Formatting Bazel Files:**

```rust
bazel run //bazel/tools/buildifier:fix --python=3.8
```

**Format & Lints the Bazel/ Starlark files**

```powershell
buildifier -lint=fix -r score/mw/log
```

## Bazel Dependency Graph Command:

**Pre-requisite:** 

 1. Install Graphviz  and xdot :  

`sudo apt update && sudo apt install graphviz xdot`

```bash
xdot <(bazel query --notool_deps --noimplicit_deps "deps(//main:hello-world)" --output graph)
```

### 6. Bazel Code Coverage:

```rust
bazel coverage --config=spp_host_gcc //platform/aas/lib/json:json_writer_unit_test
```

### 7. Bazel Code Coverity:

```rust
bazel run //platform/aas/quality/coverity -- -m platform/aas/lib/json/metrics.yaml
bazel run //platform/aas/quality/coverity -- -t //platform/aas/pas/sysmon/application/ipnext:sysmon #use_coverity_patch
```

### 8. Copybara Commands:

  **Most Common:** 

```rust
Copybara command for Baselibs: 
------------------------------
bazel run //copybara/workflows/spp_to_baselibs:to_oss_mirror -- --init-history  --force   --git-destination-url=git@cc-github.bmwgroup.net:sonu1/eclipse-score-baselibs.git

Copybara command for Logging: 
------------------------------
bazel run //copybara/workflows/spp_to_logging:to_oss_mirror --   --init-history   --force   --git-destination-url=git@cc-github.bmwgroup.net:sonu1/eclipse-score-logging.git

Copybara command for communication:
bazel run //copybara/workflows/spp_to_com:to_oss_mirror --   --init-history   --force   --git-destination-url=git@cc-github.bmwgroup.net:sonu1/eclipse-score-communication.git
```

**Another command if you are running copybara FIRST TIME:** 

```rust
bazel run --python=3.9 //copybara/workflows/spp_to_baselibs:to_oss_mirror – --squash --git-origin-fetch-depth=10 --git-origin-log-batch=10
```

Above command helps you to **avoid Java Memory Buffer Error** if you are facing that.

**Check Locally Copybara changes** with following command: 

```rust
bazel run //copybara/workflows/spp_to_logging:to_local_directory -- --folder-dir=/home/bti-001080@Bmwtechworks.local/SCORE/eclipse-score-logging --verbose
```

Use above command to generate the repo content locally to test, No need to push it to the remote branch everytime. And this is much faster.

# Github Commands :

1. Rebase your branch : 

```cpp
1. Go to master
2. git pull origin
3. Go to your branch 
4. git rebase -i origin/master
5. Resolve Conflicts.
6. git rebase --continue 
7. If you see "successfully rebased" message in terminal, You are good to go !
```

1. On Master Branch :  ( **To get lastest Master + Submodules**)

```rust
git checkout master
git pull origin 
git pull --recurse-submodules

```

1. On My Branch : (**Rebase**)

```rust
git checkout sonu_resolve_visibilities_mw_log
git rebase master

```

To check How many commits behind and ahead my branch is : 

```cpp
# To see both behind and ahead counts
git rev-list --left-right --count origin/main...HEAD
```

1. Reset / Restore : 

```powershell
git log
git reset --soft e1a59523cb6c034b7e57575560ee35d4eba50973
git status
git restore system_description
git status
git restore .
git status
git restore --staged system_description
git restore --staged ecu/ipnext
git status
git log
git status
git commit
git push origin sonu_resolve_visibilities_mw_log -f
```

1. rebase/ merge branch : sonu_branch and dev_branch

```powershell
Question: 
        
        I have one monorepo assume Platform_X repo.
        I created a branch named " sonu_branch" from master branch. 
        also I have my friends on another branch named "dev_branch" . 
        but dev_branch is not merged yet. 
        I want to compare the two branches content and
        want to use dev_branch content.
        
        
        Command: git checkout sonu_branch
        
        If you want to compare the branches: 
        
        command: 
        
        git diff origin/sonu_branch..origin/dev_branch
        git diff --name-status origin/sonu_branch..origin/dev_branch
        
        Merge or rebase to use dev_branch content

------------------------------------------------------------------------------
Now you have two main choices, depending on your workflow:

✅ Option A: Merge dev_branch into your branch
------------------------------------------------
If you want your branch to include everything from dev_branch:

git merge origin/dev_branch

This will bring all dev_branch changes into sonu_branch.

If there are conflicts, Git will ask you to resolve them.

Then commit the merge and push:

git push origin sonu_branch
--------------------------------------------------------

✅ Option B: Rebase your branch on top of dev_branch

If you want to replay your work on top of dev_branch (cleaner history):
--------------------------------------------------------
git rebase origin/dev_branch

If conflicts occur, resolve them, then:

git rebase --continue

Once done:

git push --force origin sonu_branch

(Use --force only if you know it’s safe — e.g., your branch isn’t shared.)
        
  
```

## Create a Local branch and then upstream to remote :

```powershell
git checkout master
git pull origin master  # Make sure you're up to date
git checkout -b <your-new-feature-branch>
git push --set-upstream origin <your-new-feature-branch>
```

## Your branch is 1 commit ahead and 436 commits behind master

This message indicates that your current branch is:

- **1 commit ahead** of master - you have 1 local commit that master doesn't have
- **436 commits behind** master - master has 436 commits that your branch doesn't have

This means your branch is significantly out of date with master.

**What you should do depends on your situation:**

## **Option 1: If you want to keep your 1 commit and update your branch**

```powershell
# First, make sure you're on your branch
git status

# Fetch the latest changes from remote
git fetch origin

# Rebase your commit on top of the latest master
git rebase origin/master

# If there are conflicts, resolve them, then:
# git add <resolved-files>
# git rebase --continue

# Force push (since rebase rewrites history)
git push --force-with-lease origin <your-branch-name>
```

## How to rename “Branch”

![image.png](attachment:d336fbdb-82e5-4bc3-b47f-ef273d410404:image.png)

---

## How to Edit previous commit ??

**Question :** I made a commit 10 days back, when there is not icts compliance check. Now they added ICTS compliance check on PR, Now I make another commit on that PR Now. I have two commits - previous one commit passing without ICTS compliance and latest commit failing due to previous commit non-compliance with ICTS. 

```cpp
git log --oneline -3
```

```cpp
git rebase -i HEAD~2
```

```cpp
git push -f
```

---

# Analysis/tracing - IPC folder regarding Commands

---

Mandatory Commands Before Every Git Add : 

```cpp
format source code:
bazel run //platform/aas:clang-format -- -f
 
formatting bazel files:
bazel run //bazel/tools/buildifier:fix --python=3.8
```

1. For building any target in analysis/tracing use: —config=ipn10_qnx  [I don’t know why]

```powershell
bazel build --config=ipn10_qnx7 //platform/aas/analysis/tracing/common/memory_validator/any:any
```

To Run : qnx-tests 

```cpp
bazel test --config=x86-64-qnx-unit-tests //platform/aas/analysis/tracing/daemon/code/common/mbuf_provider/bpf/test/unit_test:unit_tests_qnx
```

## Coverage of Unit Tests:

```cpp
Step 1:
Check coverage of unit test files:
    bazel coverage --config=spp_host_gcc --combined_report=lcov //platform/aas/analysis/tracing/common/shared_memory_creator/test:sysram_shared_memory_creator_test

Step 2:
Generate Html report from .dat file:
    genhtml bazel-out/_coverage/_coverage_report.dat -o coverage_html --ignore-errors source --synthesize-missing
    
Step 3:
Check report in browser on this path: safe-posix-platform/coverage_html/index.html
```

# Coverity Command :

# Note: Don’t Run coverity for single target, run for it’s parent folder’s all targets .

You can also refer this doc : **platform/aas/quality/coverity/README.md** 

You can directly also see Coverity findings on the dashboard here : 

https://coverity.swf.bmwgroup.net/

```cpp
bazel run //platform/aas/quality/coverity -- -m  /home/bti-001006/Desktop/safe-posix-platform/platform/aas/analysis/tracing/metrics.yaml
```

Another command to pass optional to store Findings.txt

```cpp
 
bazel run //platform/aas/quality/coverity -- -m  /home/bti-001006/Desktop/safe-posix-platform/platform/aas/analysis/tracing/metrics.yaml  -o  "Absolute_path_to_store_findings"
```

In above command this is metrics.yaml file : /home/bti-001006/Desktop/safe-posix-platform/platform/aas/analysis/tracing/metrics.yaml

Sample metrics.yaml file : 

path : /home/bti-001006/Desktop/safe-posix-platform/platform/aas/analysis/tracing/metrics.yaml

```cpp
# yamllint disable rule:indentation

metrics:
  values:
    targets: !flatten &targets
      - //platform/aas/analysis/tracing/common/memory_validator_/...
    exclude_targets: !flatten &exclude_targets
      - //platform/aas/analysis/tracing:unit_tests_qnx
    exclusion_patterns: !flatten &exclusion_patterns
        - ".*/test/.*"
        - ".*/mock/.*"
        - ".*/mocks/.*"
    parent: platform/aas/analysis/metrics_analysis.yaml
  tools:
    coverity:
      targets: *targets
      exclude_targets: *exclude_targets
      export_html: False
      inclusion_patterns:
        - "./platform/aas/analysis/tracing/common/memory_validator_/.*"
    clang_tidy:
      targets: *targets
      exclude_targets: *exclude_targets
      exclusion_patterns: *exclusion_patterns

```

## Bazel Code - Coverage Command :

For QNX Coverage : 

```cpp
bazel run   //platform/aas/coverage:qemu --python=3.8 -- -t //platform/aas/analysis/tracing/common/memory_validator_/test:unit_tests_qnx -b /home/bti-001006/Desktop/Sonu_new_feb_3 -o /home/bti-001006/Desktop/Sonu_new_feb_3/
```

For Host GCC 

```cpp
bazel coverage --config=spp_host_gcc --combined_report=lcov //platform/aas/analysis/tracing/common/shared_memory_creator/test:sysram_shared_memory_creator_test
```