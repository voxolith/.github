# Security policy

## Reporting a vulnerability

Please **don't open a public issue**. Report it privately through GitHub instead: on the
affected repository, open the **Security** tab and choose **Report a vulnerability**. Only the
maintainer sees the report, and the fix can be prepared in a private advisory.

Include what you can of:

- the repository, and the version or commit;
- what an attacker could do, and what they need to do it;
- a reproduction: a crafted file, a page, or steps.

Voxolith is maintained by one person, so this is best effort: expect an acknowledgement within
a week, and a fix or a plan soon after that.

## What counts

Voxolith runs in the browser and reads files users give it. Things worth reporting:

- a `.vox`, `.mca` or share code that makes the parser or a generator run out of memory, loop
  forever, or read or write outside its buffers;
- input that hangs or crashes the GPU process (a device loss the page can't recover from);
- script injection in an app (viewer, editor, examples, demolition-shot);
- anything in the GitHub workflows or the published npm packages that could let someone push or
  publish code.

A page that is slow on an unusually large but legitimate world is a performance bug; open a
normal issue.

## Supported versions

Fixes go into `main` and the next npm release of the affected package. Older releases are not
patched.
