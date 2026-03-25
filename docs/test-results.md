# FSL API Test Results

**Date:** 2026-03-25
**Test Suite:** `/opt/encrypthealth/test-suite.js`
**Environment:** VPS `74.208.202.239:4001` — PM2 `encrypthealth-api`

## Results: 10/10 PASS ✓

| Endpoint | Method | Result | Status |
|---|---|---|---|
| `/api/status` | GET | ✅ PASS | 200 |
| `/api/game/complete` | POST | ✅ PASS | 200 |
| `/api/wellness-score/:wallet` | GET | ✅ PASS | 200 |
| `/api/avatar/room` | POST | ✅ PASS | 200 |
| `/api/provider/list` | GET | ✅ PASS | 200 |
| `/api/groups/open` | GET | ✅ PASS | 200 |
| `/api/biometrics/:wallet/summary` | GET | ✅ PASS | 200 |
| `/api/ai/recommend-nt/:wallet` | POST | ✅ PASS | 200 |
| `/api/recommend/:wallet` | GET | ✅ PASS | 200 |
| `/api/operator/register` | POST | ✅ PASS | 200 |

## Notes

- `/api/ai/recommend-nt/:wallet` returns graceful fallback when `ANTHROPIC_API_KEY` is not active. Set key to enable live Claude recommendations.
- All endpoints respond within expected latency bounds.
- Health check script (`health-check.js`) also confirms 7/7 green.

## Security Scan (same session)

- PM2: online, 2 restarts (1 intentional restart during sprint)
- Fail2ban: 83 IPs currently banned, 0 current failed attempts
- Iptables: 45 rules active, saved to `/etc/iptables/rules.v4`
- No hardcoded secrets found in `index.js`
- Port 4001 bound only to PM2 process (pid 233100)
