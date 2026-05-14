---
name: Deviation Notice
about: Notify that your branch is deviating from Master (for transparency)
title: '[DEVIATION] Team: ___ | Solution: ___ '
labels: deviation
assignees: sachin-dehran
---

## Deviation Notice

> Note: You do NOT need approval to customise your branch.
> This notice is for transparency so other teams can benefit from your work.

**Team:** <!-- e.g. team2 -->
**Solution:** <!-- e.g. dbops_versol1 -->
**Branch:** <!-- e.g. project-team2 -->

### What have you customised?
<!-- Describe the change -->

### Why can't you use the standard Master version?
<!-- Technical reason, infra constraint, etc. -->

### Do you want this merged to Master eventually?
- [ ] Yes — I will open a Change Request when ready
- [ ] No — keeping it local only

### Update your config.json
Make sure `.dbops/config.json` has:
```json
{
  "deviation": true,
  "deviation_reason": "your reason here",
  "wants_merge_to_master": false
}
```
