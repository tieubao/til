---
title: "10 modern software over-engineering mistakes"
date: 2016-10-16
captured: 2016-10-16T15:14:09Z
tags: [architecture, over-engineering, software-design]
source: "GitHub issue tieubao/til#268 + https://medium.com/@rdsubhas/10-modern-software-engineering-mistakes-bc67fbef4fc8"
aliases: []
status: refined
---

## Context

An influential article by Rajesh Dhiman on common over-engineering patterns in modern software development. The core argument: business requirements only diverge, never converge, so designing for anticipated futures creates brittle systems.

**Source:** [10 Modern Software Over-Engineering Mistakes](https://medium.com/@rdsubhas/10-modern-software-engineering-mistakes-bc67fbef4fc8)
**Attachment:** [10 Modern Software Over-Engineering Mistakes - Medium.pdf](https://github.com/tieubao/til/files/531900/10.Modern.Software.Over-Engineering.Mistakes.Medium.pdf)

## The mistakes

**1. Anticipating future requirements.** Engineers overestimate their ability to predict what business will need. Requirements expand unpredictably, making comprehensive upfront planning futile.

**2. Horizontal abstraction over vertical splitting.** Rather than creating shared reusable components, isolate business actions vertically. The author demonstrates how a user profile system grew to 13 different signup flows, making horizontal code sharing counterproductive.

**3. Premature abstraction.** Perfect abstractions are mythical. Patterns emerge naturally once you observe multiple implementations. Embrace duplication until patterns reveal themselves.

**4. Shallow wrapper layers.** Modern open-source libraries are well-designed. Wrapping them creates thin layers that become maintenance burdens and prevent leveraging library improvements directly.

**5. Metrics over correctness.** Code quality tools measure coverage and compliance, not whether you are testing the right things. Automated tools cannot distinguish meaningful tests from metric-gaming.

**6. Sandwich layers.** Splitting single operations across numerous dependency-injected layers creates unmaintainable complexity. This reflects cargo-cult application of SOLID principles without understanding their purpose.

**7. Vague "-ity" justifications.** Terms like "extensibility," "scalability," and "configurability" become unchallenged justifications for unnecessary complexity. Real scenarios expose their flaws.

**8. In-house reinvention.** Custom frameworks consume ongoing maintenance effort. Contributing to existing open-source projects offers better returns than building proprietary solutions.

## Actionable takeaways

- Estimate conservatively and plan for change, not completeness
- Keep code focused on individual actions rather than abstract reusability
- Allow duplication until patterns naturally emerge
- Use proven libraries directly without defensive wrappers
- Challenge every "-ity" claim with concrete business scenarios
- Refactor continuously rather than treating code as untouchable
- Prioritize estimation accuracy to prevent quality-destroying schedule pressure

## Related

- [[write-code-easy-to-delete]] - complementary philosophy on disposable code
- [[data-drives-code-structure]] - letting reality shape design instead of predictions
