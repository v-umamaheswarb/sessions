# Speaker Notes: Agent Skills and End-to-End Tests

**Total time: 20 minutes**  
**Module 1: Agent Skills, 10 minutes**  
**Module 2: End-to-End Tests with MCP Playwright, 10 minutes**

Use these notes with [Agent-Skills-Playwright-Add-Browser-Testing.html](Agent-Skills-Playwright-Add-Browser-Testing.html). The talk track is written to be spoken, with demo actions and time checks included.

## Before the session

- Open the presentation on the title slide.
- Prepare a small project or demo repository where you can create a project skill and show it being invoked.
- For the browser demo, use a local app with a contact form and a test branch. Confirm the app starts and identify the existing test command.
- Confirm MCP Playwright is configured and available in the Copilot host. If it is not, use the configuration example in the deck as a brief explanation; do not spend the demo installing or troubleshooting tools.
- Use synthetic form data, no production services, and no real credentials. Have screenshots or a recorded result ready as a fallback.

## 0:00-1:00 | Welcome and map

**Say:**

> Today has two separate modules. First, “Customize Copilot with skills”: we will create a reusable Agent Skill that gives Copilot a repeatable team workflow. Second, “Add browser testing”: we will use Playwright MCP to inspect a local application and add an end-to-end test. The first is about teaching Copilot a task; the second is about verifying what a user experiences in a browser.

> They can work together, but neither depends on the other. We will finish each scenario with something concrete: a skill that changes how Copilot approaches a repeated task, and a browser test we can run again.

**Do:** Show the module table on the title slide. Point to the exact scenario and key feature for each row.

## Module 1: Customize Copilot with skills | 1:00-10:00

### 1:00-3:00 | Define the skill and the problem

**Say:**

> An Agent Skill is a small, task-focused package of instructions and supporting material. Its center is a `SKILL.md` file. The frontmatter gives the skill a name and a description that says what it does and when it should be used. The body provides the actual workflow. A skill can also include templates, examples, or scripts.

> The problem it solves is repeated explanation. If every person has to tell Copilot the same release-note format, source material, and rules for uncertain changes, we get inconsistent prompts and inconsistent output. A skill lets the team capture that process once and make it discoverable.

> A useful distinction: instructions are standards Copilot should follow, such as naming or formatting. A skill is a playbook for a particular task, such as preparing release notes. The description is important because it helps the agent decide when the skill is relevant.

**Do:** Show the definition and problem cards. Then advance to the skill layout and frontmatter example.

**Ask briefly:**

> What is one task where your team repeats the same Copilot guidance today?

Take one or two quick responses. Tie them back to reusable workflows, not general rules.

### 3:00-8:30 | Live demo: create and invoke a release-notes skill

**Say:**

> I’ll use release notes as the example. The particular task is less important than the pattern: name one repeated job, describe when it applies, and write down the steps and standards that produce a useful result.

**Demo actions and narration:**

1. **3:00-4:00, choose the task.** Open the demo repository and say:

   > Our repeated task is drafting release notes from merged pull requests. We want Copilot to group changes by user impact, follow our template, and flag ambiguity instead of making up details.

2. **4:00-5:30, create the skill.** Show or create `.github/skills/release-notes/SKILL.md`. Explain that the directory name and `name` value should match. Walk through the description:

   > The description says both what the skill does and when to use it: draft release notes from merged PRs using our team format, when someone asks to prepare or update release notes.

3. **5:30-6:30, add the workflow.** Show the four example steps from the deck: review merged PR titles and labels, group by user impact, use the team template, and flag unclear changes. Say:

   > The workflow is intentionally short. The skill is not a giant policy manual. We want the useful steps and references close to the task, with extra templates in separate files if the workflow needs them.

4. **6:30-7:30, invoke the skill.** In a fresh Copilot chat, ask:

   > Draft release notes from the merged PRs for this release.

   If skill discovery is delayed in this host, refresh or use its supported skill picker or slash command. Avoid implying every Copilot host activates skills in exactly the same way.

5. **7:30-8:30, review the result.** Compare the output to the template and source PRs. Ask:

   > Did it group changes in a way that helps a user? Did it leave uncertain information as a question rather than inventing a feature?

   If needed, improve the description or workflow and try again.

**Expected observation:** Copilot follows the reusable task guidance, and the presenter can point to where the team’s format and uncertainty rules came from.

**Fallback if activation fails:** Show the skill file, explain the metadata-to-workflow loading model, and use the contents as a direct prompt once. Be candid that the fallback demonstrates the instructions, not automatic discovery.

### 8:30-10:00 | Close module 1 and transition

**Say:**

> The result is a project-specific playbook, not a guarantee that every answer is correct. We still inspect the source PRs and review the generated notes. The win is that the same process is reusable and easier to improve than a prompt copied from someone’s chat history.

> Now we are changing the question. A skill customizes how Copilot handles a task. For the second module, we want evidence that an actual web interface behaves correctly for a user.

**Do:** State the Module 1 takeaway: “A skill packages a repeatable task with a clear trigger and an explicit workflow.” Advance to End-to-End Tests / MCP Playwright.

## Module 2: Add browser testing | 10:00-20:00

### 10:00-12:00 | Define browser testing and MCP Playwright

**Say:**

> End-to-end testing checks a user journey through the running application, rather than testing only a function or component in isolation. Playwright MCP gives Copilot tools to inspect and interact with a real browser: navigate, read page structure, click, fill fields, and inspect what happens next.

> This addresses a gap in build and unit-test results. A build can pass while a form is missing a label, a button does nothing, or a successful submission never shows confirmation. Browser interaction lets us exercise the interface the way a user does.

> There are two related outcomes here. MCP interaction lets an agent explore and verify a page during the session. A committed Playwright test gives the team a repeatable check that can run again, including in CI. A browser walkthrough is useful evidence, but it is not itself regression coverage or approval to release.

**Do:** Show the MCP definition and the “Observe, Act, Verify” sequence. Briefly point to the `.vscode/mcp.json` example only if the server is already configured or the audience asks how it connects.

### 12:00-18:00 | Live demo: add browser testing

**Say:**

> The scenario is “Add browser testing.” I’m going to use a local contact form, synthetic data, and a demo branch. I’ll ask Copilot to inspect the project first so it follows the existing framework instead of inventing a second test setup.

**Demo actions and narration:**

1. **12:00-13:00, inspect the project.** Show the project test files and the local run/test commands. Start the app using its existing workflow. Say:

   > Before changing anything, we need to know how this project starts, what it already uses for tests, and whether the form can be submitted locally without an external side effect.

2. **13:00-14:00, confirm the browser tools.** Verify that Playwright MCP is available in Copilot. Ask Copilot to inspect the local page and identify the form controls. Say:

   > The agent should discover controls through their accessible roles and labels where possible. That is more robust than selecting a button by its current color or layout position.

3. **14:00-15:30, request the test.** Use this prompt, adapting the URL and command to the repository:

   > Inspect this project’s existing test setup. Add a Playwright end-to-end test for the contact form: submit valid synthetic data and verify the success state. Use the local app only; do not send external requests.

   Review the proposed file and any package or configuration changes before accepting them.

4. **15:30-16:30, run the test.** Have Copilot execute the project’s existing test command. If the test fails, use MCP to inspect the current browser state and identify whether the problem is app setup, selector choice, validation, or the expected success state.

5. **16:30-18:00, verify the evidence.** Show the test result and the visible confirmation state. Say:

   > We have two pieces of evidence: the browser reached the expected success state, and the repeatable test passed. If either one is missing, we should not call the scenario verified. We should inspect the failure and rerun after fixing it.

**Expected observation:** A Playwright test follows the form journey with synthetic local data and asserts a visible success result. The test can be rerun independently of the live agent conversation.

**Fallback if the app or MCP stalls:** Stop after a short attempt. Show a previously captured run or test result, explain what it proves, and identify what would still need validation. Do not spend the presentation window debugging the environment or switch to production data.

### 18:00-20:00 | Wrap-up and discussion

**Say:**

> Let’s separate the takeaways. Agent Skills customize Copilot for a repeated task by packaging a trigger and workflow. End-to-end tests exercise a user journey in a running app. MCP Playwright gives an agent a browser to inspect and interact with; a saved Playwright test makes the check repeatable.

> In both cases, the model can help perform the work, but we are responsible for reviewing its output. We check that the release notes match their source, and that browser tests assert the behavior users actually need. Then normal CI and human approval remain in place.

**Ask the team:**

> Which repeated Copilot task should we turn into a skill first? And which user journey should we protect with an end-to-end test?

Take one or two responses. Close with the next action: choose an owner and a small pilot for each module.

## Timing at a glance

| Time | Slide / activity | Outcome |
|---|---|---|
| 0:00-1:00 | Opening and module map | Establish the two independent scenarios |
| 1:00-3:00 | Agent Skills definition and problem | Explain reusable task guidance |
| 3:00-8:30 | Create and invoke release-notes skill | Show Copilot customized by a team workflow |
| 8:30-10:00 | Module 1 recap | Summarize the skill pattern |
| 10:00-12:00 | End-to-End Tests and MCP Playwright | Distinguish browser interaction from saved tests |
| 12:00-18:00 | Add a contact-form browser test | Inspect, create, run, and verify |
| 18:00-20:00 | Wrap-up and discussion | Agree on pilot tasks |
