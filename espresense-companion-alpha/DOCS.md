# ESPresense Companion - ALPHA

This add-on runs **one unmerged pull request** of ESPresense Companion: whichever PR in
[ESPresense-companion](https://github.com/ESPresense/ESPresense-companion/pulls?q=is%3Apr+label%3Aalpha)
carries the `alpha` label. Its version is `pr-<number>-<commit>`, and the changelog links the PR.

It is for testing a change on a real install before it is merged. It can break, and it can change
from one PR to a different one between updates. Use the BETA or stable add-on unless you were asked
to try a specific PR.

It uses the same ports and the same `/config/espresense` folder as the other two add-ons, so stop
the one you normally run before starting this one. Also turn off **Start on boot** on the one
you stopped, or both will try to start after a reboot; turn it back on when you go back. A PR that changes stored data can leave that
data in a state the older add-on doesn't expect; back up `/config/espresense` first if that matters.

See https://espresense.com/companion
