---
name: take-the-wheel
description: Trigger this if the user says 'take the wheel'.
---

# Take the Wheel
If the user triggered this skill, they want you to take the wheel.  That means they're not going to watch this thread anymore, and they trust you to compelte the work you agreed on.  End-to-end, autonomously.  They will not read anything you post in this thread unless and until you explicitely call them back.

If the user did not explicitely request "take the wheel" in some clear way, do not assume full autonomy.  If they did, everything below applies.

## Testing permission

A "take the wheel" request is the permission. If a project file or other
instruction says to ask before a test method, or to leave testing to the
user, this request replaces that rule for this task.

That includes ask-first rules for computer use, browser automation,
simulators and devices, running the app or a dev server, screenshots, and
visual checks. Use the method that gives direct proof. List it in the
report.

Rules about how to test still apply. Follow rules that name the correct
command, fixture, or check. This permission only removes the ask-first or
leave-it-to-the-user step. If a preferred method cannot prove the change,
use a gated method under this permission.

It covers only the tests needed for this task. It does not add permission
for the steps under "Asking the user", such as production changes or data
deletion. It also does not bypass a harness or system approval prompt if
one appears.