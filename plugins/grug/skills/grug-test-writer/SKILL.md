---
name: grug-test-writer
description: "Write tests the grug way: integration tests at cut points. Use when someone asks to write tests, add coverage, or asks 'how should I test this'. Helps when someone is over-testing with mocks or under-testing with nothing."
---

# Grug Test Writer

Grug love test. Test save grug many time. But grug not worship test idol.

## The Sweet Spot

**Integration tests at cut points = focus.** High enough to test correctness. Low enough to debug when broken. This is where grug puts ferocious effort.

**Unit tests = fine early on.** Help get going, but break when implementation changes. Don't get attached.

**End-to-end tests = small curated suite.** Keep working religiously. If ignored because "oh that breaks all time" — delete it.

**Mocking = only when necessary.** Coarse grain, at system boundaries only. Never mock internal collaborators.

## Writing a Test

1. **Find the cut point.** API endpoint, service interface, repository, message handler. The natural boundary with a narrow interface.

2. **Use real dependencies.** Real database, real file system. Mock only external third-party services.

3. **Arrange → Act → Assert.** One behaviour per test. Assert on outcomes, not implementation. Good name describing scenario and expected result.

4. **Bug reproduction exception.** When bug found, always write failing test first, then fix. Only time grug does test-first.

## What Grug Skips
- Testing private methods (extract to own module if it needs testing)
- Chasing coverage numbers (right question: "will a test tell grug if this breaks?")
- Test-first for code grug doesn't understand yet
- Tests with conditional logic (if test has if/else, split it)

Test that nobody runs is worse than no test. Keep tests simple. Keep tests useful. Delete the rest.
