---
type: bug
status: todo
priority: high
created: 2025-01-20
updated: 2025-01-20
severity: high
---

# Bug: Login Request Times Out After 30 Seconds

## Summary

User login fails with timeout error after waiting 30 seconds.

## Environment

- OS: macOS 14.2
- Browser: Chrome 120
- App Version: 2.1.0
- Environment: Production

## Expected Behavior

Login should complete within 2-3 seconds.

## Actual Behavior

Login request hangs for 30 seconds, then fails with timeout error.

## Steps to Reproduce

1. Open app at https://app.example.com
2. Enter username: test@example.com
3. Enter password: [any valid password]
4. Click "Login" button
5. **Result:** Request hangs, timeout after 30s

**Reproducibility:** Always (100%)

## Error Logs

```
Error: timeout of 30000ms exceeded
  at createError (createError.js:16)
  at XMLHttpRequest.handleTimeout (xhr.js:99)

Network tab:
POST /api/auth/login - Status: failed
Request took: 30.04s
```

## Root Cause Analysis

**Suspected Cause:**
Database connection pool exhausted or network issue.

**Files Involved:**

- `src/services/auth.ts` - Login service
- `src/api/auth.controller.ts` - Auth controller
- Database connection settings

## Solution Approach

1. **Increase timeout temporarily** (quick fix)

   - Change timeout from 30s to 60s in axios config

2. **Investigate database connection** (proper fix)

   - Check connection pool size
   - Add connection retry logic
   - Add better error handling

3. **Add logging**
   - Log request start/end times
   - Log database query times

**Files to Modify:**

- `src/config/axios.ts` - Increase timeout
- `src/services/auth.ts` - Add retry logic
- `src/config/database.ts` - Optimize connection pool

## Testing Plan

**Verification Steps:**

1. Clear browser cache
2. Attempt login
3. Verify completes within 3 seconds
4. Check error logs for issues

**Regression Testing:**

- [ ] Other API calls still work
- [ ] Sign up flow not affected
- [ ] Password reset works

## Success Criteria

- [ ] Login completes in < 3 seconds
- [ ] No timeout errors
- [ ] Proper error messages if issues occur
- [ ] Logs show request timings

## References

- Similar issue: #245
- Database docs: [link]
