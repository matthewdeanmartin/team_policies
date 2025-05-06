# team_policies
Generic software development team policies.

These are policies suitable for closed source, commercial software with well-defined roles. Please read
[Working in Public](https://www.amazon.com/Working-Public-Making-Maintenance-Software/dp/0578675862) for
the special conditions of open source software.

## Pull Requests
- All developers must work on branches
- Two people need to approve a merge request
- The build should pass basic quality checks
- Merges to main should squash commits
- Don't rebase unless the whole team is advanced git users.
- It is the pull requester's responsibility to handle merge conflicts
- Code achieves the goal of the ticket and the acceptance criteria
- (If per branch deployment is possible) Code deploys successfully and works in deployment environment

## Build Quality Gates
The build script should be in a reasonably common script like Makefile, Justfile, Bash that can be run locally. The CI 
pipeline scripts as much as possible should call these scripts instead of relying on build steps that can only be run
on the build server.

Automatic steps should be run by a precommit hook.
- Autoformat
- Autofix

The CI may detect that precommits were not run and reject the build.

Manual steps
- Type annotations are present and pass the type checker.
- Code compiles with compiler warnings all on.
- Linting passes at some level of strictness (see below for exceptions)
- Unit tests pass
- Security scan passes
- Other language specific quality gates pass

Linting is not a good code gate for 
- stupid rules
- when there is too much lint
- the lint's rules should instead be governed by autoformatter
- An autofixer is available for the specific rule

Optional Strictness. The reviewer will have to use professional judgement
- High unit test coverage
- Unit test quality (assertions)
- High unit test coverage of new code

Unit tests are not cost free. Not having unit tests is its own cost.

Periodic Checks that would be nice to run every build, but would be impractical
- Integration tests
- End to End tests
- Performance tests
- Documentation presence checks (API docs)
- Spellcheck
- Mutation testing, hypothesis testing, other esoteric styles

## Use of AI
Your client or employer may have AI policies already. Additive to those...

AI usage is on a spectrum of completely save to completely reckless.

## Safe(r)
- Asking knowledge questions. Possibly no more unreliable than StackOverflow or random blogs.
- Generating simple code in a language you understand.
- Generating medium complexity code along with comprehensive unit tests.
- Two screen generation. Generate code on one computer, hand copy it to the other computer. Guarantees no proprietary code leaks.
- Copying and pasting code into a local LLM. However, most LLMs you can run locally are not smart enough to not make catastrophically stupid mistakes.
- Non-production code, e.g. one off scripts, unit tests, build scripts
- Transformation tasks, convert this bash to python, upgrade this python from v2 to v3
- Use frontier models

## Think about it
- Copying pasting non-public code into non-local LLMs. LLM hosts vary in their privacy policies.
- Line completion. It is sometimes worse than ordinary intellisense, especially at completing methods or properties.
- Generating code that is in a language you are not familiar with
- Whole document editing. LLMs can make changes to your document that 
- Unit tests. LLMs test basic situations beautifully. They will also tend to mock too much, monkey patch code under test to fix a bug to make a test pass and engage in other shenanigans.
- Using libraries suggested by LLMs. These often do not exist and malicious users will "slopsquat" malicious libraries at the names that the LLM commonly hallucinates.

## Reckless
- Agentic coding where the tool edits, commits and runs code.
- Untested code, generated blind, copy-pasted and committed.
- Complex code you don't understand
- Code that relies on math you don't understand
- Don't use small models. They may be free, but they're more likely to make catestrophic errors.

## How to recognize AI
AI code is highly recognizable. If your client bans LLM use, they will know by:

- Perfect spelling and grammar.
- Extremely regular commenting style
- Lack of commitment to dependencies, e.g. using unittest mock one second and using pytest-mock the next
- Left over conversation
- Instruction in comments to do further editing

## All in on AI
If the team is all in on AI, some practices

- Try to commit LLM code separate from handwritten code. Agentic coding does this automatically, but it is up to us to try to keep LLM commits separate.
- Add a link to the conversation
- Include prompt in the comments