# Harbor Cart: IMPACT example PR

Harbor Cart is a synthetic Python checkout service used to demonstrate
[IMPACT](https://github.com/Omar8345/IMPACT). Its `main` branch contains an
intentional bug: a coupon can exceed the cart subtotal and produce a negative
charge. The example pull request fixes that line.

The change starts in `shop/pricing.py::apply_coupon`. Pricing, checkout, and
receipt functions call through it, so the repo has tests for both normal and
oversized coupons. On `main`, two oversized-coupon tests fail. On the fix branch,
all three tests pass.

Run the tests from this repository's root:

```bash
python -m unittest discover -s tests -v
```

`coupon_bug.patch` records the earlier one-line regression. `company.json` and
the Markdown files in `company/` are fictional ownership and decision records
for IMPACT's Company Brain example. No private data or third-party packages are
needed to run this repository's tests.

## GitHub Actions review

The [Review PR with IMPACT](.github/workflows/impact-review.yml) workflow runs
when a Python pull request opens or changes. You can also run it manually from
**Actions → Review PR with IMPACT → Run workflow** with PR number `1`. It starts
a temporary FalkorDB service, runs IMPACT on the PR, compares graph-selected
tests on the base and head commits, saves the Markdown and interactive HTML
report as an artifact, and posts or updates a Markdown comment on the PR.

The analysis job has read-only repository access. Only the separate comment
job can post to the PR. This example accepts PR branches from this repository;
it does not run tests from forks. It needs no FalkorDB Cloud or AI API key.
