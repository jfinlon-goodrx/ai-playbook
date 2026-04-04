# Task: Jira Story to Delivery Plan

## When to Use
You want to turn a Jira story, epic, or bug into a concrete engineering plan the team can execute.

## The Prompt

```text
Read Jira ticket [PROJ-1234] and summarize it for delivery planning.

Return:
1. The business goal in plain language
2. The acceptance criteria in testable language
3. Missing information or ambiguities
4. Dependencies, blockers, and linked work
5. A proposed implementation breakdown for:
   - Product / story clarification
   - Development
   - QA
   - Release / rollout
   - Documentation

Then produce a suggested sub-task list with:
- Title
- Owner role
- Definition of done
- Dependencies
- Estimated complexity

If the story is not implementation-ready, say so clearly and list the questions that should be resolved before coding starts.
```

## Follow-Up Prompt: Ready for Build?

```text
Based on the Jira story and the repo context, tell me whether this is ready for implementation.

If not ready, rewrite the acceptance criteria and technical notes so the development team, QA team, and reviewer all know what done looks like.
```

## Tips

- This works best when the story description, comments, and linked docs are all included.
- Ask the AI to separate missing product decisions from missing technical details.
