# Security policy

## Reporting a vulnerability

Please report security issues privately to **security@straitlink.in**.
Do not open a public GitHub issue, pull request or discussion for a suspected
vulnerability.

Our contact details and disclosure policy are published at
https://straitlink.in/.well-known/security.txt (policy: https://straitlink.in/security/).

Please include:

- the affected package and version (`strait-sample-app`, with the commit you used),
- steps to reproduce or a proof of concept,
- the impact you observed or expect.

We acknowledge reports as soon as we can, keep you updated while we work on a
fix, and credit you in the release notes if you would like.

## Supported versions

This sample app is kept on its latest commit on `main`; fixes land there.

## Keys

This sample app only uses a workspace's **publishable** key (`st_pub_live_…` /
`st_pub_test_…`), which is safe to ship in apps and web pages. Never put a
secret key (`st_live_…` / `st_test_…`) in client code or in a fork of this
repo. If a secret key leaks, revoke it in the dashboard (Settings → Secret
keys) right away and create a new one.
