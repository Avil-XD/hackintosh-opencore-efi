# Security Policy

## Supported Versions

We currently support the following versions of this Hackintosh EFI configuration:

| Version | Supported          |
| ------- | ------------------ |
| Latest  | :white_check_mark: |
| < 1.0   | :x:                |

## Security Considerations

**IMPORTANT**: This Hackintosh EFI configuration is provided for educational and research purposes only. Users should be aware of the following security considerations:

### Boot Security
- This EFI configuration disables certain security features to enable macOS compatibility
- Secure Boot is disabled in the OpenCore configuration
- System Integrity Protection (SIP) may be partially disabled
- Users should understand the security implications before use

### Hardware Security
- Ensure your hardware firmware is up to date
- Use trusted sources for any additional kexts or drivers
- Verify checksums of downloaded files when possible

## Reporting a Vulnerability

If you discover a security vulnerability in this EFI configuration, please report it responsibly:

### Where to Report
- **GitHub Issues**: For general security concerns that don't pose immediate risk
- **Private Contact**: For critical vulnerabilities, create a private issue or contact maintainers directly

### What to Include
Please include the following information in your report:
- Description of the vulnerability
- Steps to reproduce the issue
- Potential impact and risk assessment
- Suggested fixes or mitigations (if any)
- Your testing environment details

### Response Timeline
- **Acknowledgment**: Within 48 hours
- **Initial Assessment**: Within 1 week
- **Resolution**: Varies based on severity and complexity

### Responsible Disclosure
- Please allow reasonable time for issues to be addressed before public disclosure
- We will credit researchers who report vulnerabilities responsibly
- Critical security updates will be released as soon as possible

## Security Best Practices

### For Users
1. Keep your EFI configuration updated
2. Only use kexts from trusted sources
3. Regularly update your system firmware
4. Use strong passwords and enable FileVault if needed
5. Understand the security trade-offs of running a Hackintosh

### For Contributors
1. Review code changes for potential security implications
2. Validate any new kexts or drivers before inclusion
3. Follow secure coding practices
4. Document any security-related configuration changes

## Disclaimer

This project modifies system-level components and boot processes. Users assume all risks associated with using this configuration. The maintainers are not responsible for any security issues, data loss, or hardware damage that may result from using this EFI configuration.

**Remember**: Running macOS on non-Apple hardware may violate Apple's Software License Agreement. Please ensure compliance with all applicable laws and agreements in your jurisdiction.