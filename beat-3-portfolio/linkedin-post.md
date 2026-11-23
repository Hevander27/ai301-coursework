I built an AI-assisted tool called Issue Scout to help developers find meaningful open-source contributions. Along the way, I discovered that a small mistake in its filtering rules was eliminating legitimate opportunities before they could even be evaluated.

During my first real test, the tool excluded 21 issues, even though only two deserved to be filtered out. After revising the rules and replaying the results, it excluded only those two.

I also improved how the tool evaluates repository participation and checks whether issues are already being addressed.

I then used Issue Scout to evaluate documentation opportunities in ChurchCRM, an open-source platform for church management. That led to a contribution updating its email documentation, which I revised after discovering overlapping work already completed by a maintainer.

My pull request is now open and awaiting review.

This experience reinforced something I value in engineering: AI tools should be evaluated against real evidence, and their recommendations should remain open to scrutiny and improvement.

https://github.com/ChurchCRM/docs.churchcrm.io/pull/252