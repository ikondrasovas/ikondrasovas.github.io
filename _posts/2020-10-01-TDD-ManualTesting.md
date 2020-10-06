---
layout: post
title: Can TDD be Used in Manual Testing?
published: true
lang: en
ref: tddmanual
---

Sure. But I still think any manual testing script is a technical debt. Please tell me I am wrong.

Still, I think it is better live with manual testing than without any testing.

Usually, when we hear about TDD (Test-Driven Development) we automatically liste the terms "mock", "dependency injection", "SUT" and so on so on.

I get it. Test automation is main stream (at least it should be). An there are discussions if prefer [classical or mocking testing](https://martinfowler.com/articles/mocksArentStubs.html#ClassicalAndMockistTesting).

This is not the point in this article.

My point is that we may be missing a good opportunity to insert a good team practice before jumping in to automate everything as the unique alternative to better software. TDD can also happen in places where manual testing is also present.

Can we still count on traditional manual testers and apply TDD principles?

![Tester, I found a defect today.](../images/tddTesters.jpg)

I think we definitely can. What if, as soon a user story starts (or an accelerator), developer and tester work in parallel?

While the new feature is implemented, why not create a manual test script and make sure it fails?

Isn't it what would be expected during a full fledge automated test? We write the test and make sure it fails before you add the expected functionality? So why can't we apply this principle when creating a manual test plan?

If test cases are already written before developer finishes the implementation, congratulations! But how do you know if the tests themselves are actually testing what do you think you are testing, if you do not execute them first and make sure them fail?

In the old way, testers where usually under pressure to finish their works before the end of the sprint. Some team have a disfunction to push the testing phase of the current user stories to the next sprint, with the excuse testers do not have anything to do until code is ready.

Why not change that and incorporate testing as soon sprint starts?
