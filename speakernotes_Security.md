# Speaker Notes: Security Review, Code Quality, CodeQL, and Secret Scanning

## Suggested Run of Show

Allow about 15 minutes, plus questions:

- Opening and framing: 1 minute
- Security Review: 3 minutes
- Code Quality: 3 minutes
- CodeQL: 4 minutes
- Secret Scanning: 3 minutes
- Wrap-up: 1 minute

The walkthroughs below assume a GitHub repository and a prepared demo branch. Product availability and organization settings vary, so confirm access before the session and avoid promising a feature or license based on this presentation alone.

## Opening

**Say:**

> Today I’ll show four complementary ways to improve security and code health. Security Review helps us inspect proposed changes. Code Quality highlights maintainability and coverage signals. CodeQL looks for risky code patterns and data flows. Secret Scanning helps prevent credentials from being committed and find exposure that already happened. These tools help developers make better decisions; they do not replace testing or human review.

**Transition:**

> Let’s start at the point where a change is already being discussed: the pull request.

## 1. Security Review

### Definition

AI-assisted code review analyzes a pull request or local changes and comments on likely bugs, security concerns, missing tests, and inconsistencies with project guidance. Repository instructions can tell it which conventions and risks matter to the team.

### Problem it solves

Human reviewers have limited time, and routine issues can be missed or handled inconsistently. An additional review pass can surface those issues while the change is still small. It cannot determine product intent or guarantee that a change is safe.

### Demo walkthrough

**Prepare:** Use a small demo pull request with one deliberate, non-production issue. Add or use repository review instructions if available. Do not put credentials or sensitive data in the example.

1. Open the pull request and point out the changed files and the normal review controls.
2. Request a Copilot review from the reviewers panel, or show the result of an already-completed review. If your organization has automatic reviews enabled, explain that this is organization configuration rather than a default assumption.
3. Open one inline comment. Explain what code it refers to and what risk or quality concern it is raising.
4. Ask the team: “Is this finding valid in this context? What would we check before accepting a fix?”
5. Inspect the proposed change, run the relevant test or check, and resolve or dismiss the comment with a reason.

**Point out:** Instructions can improve relevance. Human reviewers still decide whether the finding is real and whether the fix is correct.

**Say:**

> Think of this as another reviewer who is useful for mechanical checks. A clean AI review is not a security sign-off; we still need to understand the change and validate it.

**Transition:**

> Pull request review looks at a particular change. Next, let’s look at signals that help us improve the health of a codebase over time.

## 2. Code Quality

### Definition

GitHub Code Quality brings together code-quality analysis and coverage reporting. The training material describes CodeQL analysis for supported languages, model-based detection for issues static rules may miss, and suggested fixes for some findings. Confirm current language and feature availability for the repository before presenting.

### Problem it solves

Quality findings and test coverage are often scattered across alerts, CI logs, and dashboards. Bringing them closer to the code makes it easier to identify maintainability problems and see where tests are missing, then prioritize improvements.

### Demo walkthrough

**Prepare:** Select a repository in a supported language. Enable the feature if your organization has access, or use a repository where analysis and coverage data are already available. If it is unavailable in your environment, show the concept using prepared screenshots or an existing finding rather than implying it is enabled.

1. Open the repository’s Code Quality view or the prepared findings page.
2. Select one finding and show its explanation, affected file, and suggested remediation if present.
3. Open the coverage information and identify an area with low coverage.
4. Relate the finding to recent development: “Would a test here help us catch regressions in code that changes frequently?”
5. On a demo branch, review a suggested change or add a focused test, then run the checks and discuss the result.

**Point out:** Prioritize areas with both low coverage and frequent changes. A suggested fix is a starting point: review it in context and run tests.

**Say:**

> The value is not a score by itself. It is having quality findings and coverage signals visible enough to help us choose the next useful improvement.

**Transition:**

> Code Quality gives us broader health signals. CodeQL is the analysis engine behind many code-scanning alerts, and its data-flow view helps explain why an alert matters.

## 3. CodeQL

### Definition

CodeQL analyzes source code by creating a queryable representation of it, then running queries that identify patterns associated with vulnerabilities and other defects. An alert can include the rule, the affected code, and a path showing how data flows through the program.

### Problem it solves

Some risky behavior is difficult to identify by reading one line at a time. CodeQL can find patterns across a repository and trace relevant code paths, making it easier to locate and understand potential vulnerabilities.

### Demo walkthrough

**Prepare:** Use a training repository with CodeQL already enabled and a known, pre-generated alert. A deliberately vulnerable training sample is appropriate; do not demonstrate against production code or attempt to exploit a real service. Pre-run analysis to avoid waiting for a workflow.

1. Open the repository’s **Security** tab, then **Code scanning** (the exact navigation may vary).
2. Open one prepared alert and read its title and severity.
3. Show the affected file and line, then follow the data-flow path. Explain the source, the steps through the code, and the sensitive operation or sink identified by the alert.
4. Read the rule’s help text and remediation guidance. Ask the team what evidence they would use to verify the issue and the proposed fix.
5. On a demo branch, make a corrective change and run the relevant tests. After CodeQL analysis completes, return to the alert and show its updated status if the environment permits.

**Explain setup options:** Default setup is the managed starting point for many repositories. Advanced setup is useful when the team needs control over build steps, query packs, triggers, or analysis scope. Compiled languages may need CodeQL to observe the real build. The default query suite generally favors precision; an extended suite can find more issues but may produce more findings to triage.

**Say:**

> The important part of this alert is not just the rule name. Follow the path and understand why data reaches the flagged operation. Then validate the correction with tests and a new analysis run.

**Transition:**

> CodeQL helps find risky behavior in code. The next example is about a different kind of risk: a credential that can grant access if it gets committed.

## 4. Secret Scanning

### Definition

Secret Scanning checks repository content and history for patterns that look like credentials. Push protection blocks supported secrets before the push reaches the repository. Detection can find exposure, but it does not revoke a credential.

### Problem it solves

A token, password, or connection string committed to a repository may be copied or used before anyone notices. Deleting it from the latest version does not make it safe or remove it from history. A real exposed credential must be revoked or rotated promptly.

### Demo walkthrough

**Prepare:** Use a disposable demo repository where secret scanning and push protection are enabled. Follow GitHub’s current documentation for its test secret value and test procedure. Never paste a real token, password, or connection string into a demo. If push protection cannot be demonstrated in your environment, show a prepared alert and explain the expected block instead.

1. Show that push protection is enabled in the repository or organization settings.
2. Follow GitHub’s documented procedure using only its test secret value. Attempt the test push and show the prevention message.
3. Remove the test value from the demo change and retry, if that is part of the documented procedure.
4. Open a prepared secret-scanning alert and show its location and status. Explain that historical scanning can find a credential even if it is no longer in the current file.
5. Walk through the response: revoke or rotate the credential, investigate potential use, remove the exposed value from code and history as appropriate, and resolve the alert accurately.

**Point out:** Push protection prevents some new exposures; secret scanning detects supported patterns. Both are part of the response, and neither replaces credential rotation.

**Say:**

> If a real credential reaches the repository, treat it as compromised. Rotate or revoke it first; cleaning up Git history alone does not invalidate it.

## Wrap-Up

**Say:**

> These controls work best together: review changes before merge, use quality and coverage signals to prioritize improvements, investigate CodeQL alerts through their data flows, and prevent or quickly respond to exposed credentials. The common practice is to verify findings, validate fixes, and keep people accountable for the final decision.

**Discussion prompt:**

> Which of these checks is already part of our workflow, and what is one small gap we could close next?

## Presenter Checklist

- Confirm the demo account can access the repository and relevant security views.
- Confirm Copilot code review and Code Quality availability and settings; have a fallback screenshot if needed.
- Pre-run CodeQL so the prepared alert is available without waiting for analysis.
- Use only a disposable repository and GitHub’s documented test secret procedure.
- Do not display real credentials, customer data, or sensitive repository content.
- Use a demo branch; do not merge demonstration changes into a production branch.
- Avoid quoting prices or plan terms unless you have checked current official details.

## Reference Links

- [Copilot code review](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review)
- [GitHub Code Quality](https://docs.github.com/en/code-security/concepts/code-quality/code-quality)
- [CodeQL code scanning](https://docs.github.com/en/code-security/concepts/code-scanning/codeql/codeql-code-scanning)
- [Secret scanning](https://docs.github.com/en/code-security/concepts/secret-security/secret-scanning)
