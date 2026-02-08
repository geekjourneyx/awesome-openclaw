# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| Latest  | [PASS]        |
| Older   | [FAIL]        |

## Reporting a Vulnerability

This is a curated list of resources and does not execute code. However, if you discover a security vulnerability in this repository:

1. **Do NOT open a public issue**
2. Send an email to the repository maintainers
3. Include:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if known)

## Security Best Practices for OpenClaw

While this repository is a resource list, the OpenClaw project itself has specific security considerations:

### Critical Security Principles

1. **Never expose API keys**
   - Use environment variables
   - Never print keys in responses
   - Rotate keys regularly

2. **Implement Access Control**
   - Whitelist allowed users
   - Use webhook verification tokens
   - Enable rate limiting

3. **Sanitize All Inputs**
   - Validate user input
   - Escape special characters
   - Check for command injection

4. **Keep Dependencies Updated**
   - Regular security audits
   - Update OpenClaw regularly
   - Monitor security advisories

### Official Security Resources

- [OpenClaw Security Documentation](https://docs.openclaw.ai/security)
- [Security Best Practices](https://docs.openclaw.ai/security/best-practices)
- [Report OpenClaw Vulnerabilities](https://github.com/openclaw/openclaw/security/advisories)

### Prompt Injection Awareness

[WARN] **Important**: Prompt injection is an industry-wide unsolved problem.

**Mitigation strategies:**
- Use strong models (Claude 3.5 Sonnet, GPT-4)
- Implement input validation
- Use system prompts carefully
- Monitor for suspicious patterns
- Educate users about risks

## Security Checklist

Before deploying OpenClaw:

- [ ] API keys in environment variables
- [ ] Webhook verification enabled
- [ ] Access control configured
- [ ] Rate limiting active
- [ ] HTTPS/TLS enabled
- [ ] Input sanitization implemented
- [ ] Dependencies updated
- [ ] Monitoring/logging configured

## Disclosure Policy

We follow responsible disclosure:

1. Acknowledge receipt within 48 hours
2. Investigate within 7 days
3. Fix within 30 days (critical: 7 days)
4. Public disclosure after fix is deployed

## Contact

For security concerns:
- Email: security at openclaw dot ai
- GitHub Security Advisory: [Report Vulnerability](https://github.com/openclaw/openclaw/security/advisories/new)
