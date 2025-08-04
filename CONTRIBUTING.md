# Contributing to Hackintosh EFI Configuration

Thank you for your interest in contributing to this Hackintosh EFI configuration! This document provides guidelines for contributing to the project.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Contribute](#how-to-contribute)
- [Reporting Issues](#reporting-issues)
- [Submitting Changes](#submitting-changes)
- [Configuration Guidelines](#configuration-guidelines)
- [Testing Requirements](#testing-requirements)
- [Documentation Standards](#documentation-standards)

## 🤝 Code of Conduct

By participating in this project, you agree to abide by our code of conduct:

- Be respectful and inclusive
- Provide constructive feedback
- Focus on what is best for the community
- Show empathy towards other community members

## 🛠 How to Contribute

### Types of Contributions

- **Bug Reports**: Report issues with the current configuration
- **Feature Requests**: Suggest improvements or new features
- **Documentation**: Improve or add documentation
- **Configuration Updates**: Update kexts, drivers, or OpenCore versions
- **Hardware Support**: Add support for new hardware configurations

## 🐛 Reporting Issues

When reporting issues, please include:

1. **Hardware Information**:
   - CPU model and generation
   - Motherboard model and BIOS version
   - GPU model(s)
   - RAM configuration
   - Storage devices

2. **Software Information**:
   - macOS version
   - OpenCore version
   - Kext versions

3. **Problem Description**:
   - Clear description of the issue
   - Steps to reproduce
   - Expected vs actual behavior
   - Screenshots or logs (if applicable)

4. **Configuration Details**:
   - Any modifications made to the EFI
   - BIOS/UEFI settings used

## 📝 Submitting Changes

### Before Submitting

1. **Test Thoroughly**: Ensure your changes work on the target hardware
2. **Update Documentation**: Modify relevant documentation files
3. **Follow Naming Conventions**: Use clear, descriptive names for files
4. **Sanitize Personal Data**: Remove any personal information from configs

### Pull Request Process

1. **Fork the Repository**: Create a personal fork of the project
2. **Create a Branch**: Use a descriptive branch name (e.g., `fix-audio-issue` or `add-gpu-support`)
3. **Make Changes**: Implement your changes with clear commit messages
4. **Test Changes**: Verify everything works as expected
5. **Update Changelog**: Add your changes to `CHANGELOG.md`
6. **Submit PR**: Create a pull request with a clear description

### Commit Message Guidelines

Use clear, descriptive commit messages:

```
feat: add support for Intel AX200 WiFi
fix: resolve audio issues on ALC892 codec
docs: update installation guide for Big Sur
chore: update OpenCore to version 0.8.5
```

## ⚙️ Configuration Guidelines

### OpenCore Configuration

- **Use Latest Stable Version**: Keep OpenCore updated to the latest stable release
- **Minimal Configuration**: Only include necessary kexts and settings
- **Document Changes**: Explain why specific settings are used
- **Test Compatibility**: Ensure changes work across supported macOS versions

### Kext Management

- **Official Sources**: Use kexts from official repositories when possible
- **Version Compatibility**: Ensure kext versions are compatible with OpenCore
- **Minimal Set**: Only include kexts that are actually needed
- **Documentation**: Document the purpose of each kext

### ACPI Patches

- **Hardware Specific**: Clearly document which hardware requires specific patches
- **Source Code**: Provide source files for custom SSDT patches when possible
- **Testing**: Thoroughly test ACPI changes across sleep/wake cycles

## 🧪 Testing Requirements

Before submitting changes, test the following:

### Basic Functionality
- [ ] System boots successfully
- [ ] All hardware components are recognized
- [ ] Sleep/wake cycle works properly
- [ ] Audio input/output functions correctly
- [ ] Network connectivity (Ethernet/WiFi) works
- [ ] USB ports function properly

### macOS Versions
- Test on supported macOS versions
- Verify compatibility with macOS updates
- Document any version-specific requirements

### Performance Testing
- [ ] Boot time is reasonable
- [ ] System stability under load
- [ ] No kernel panics during normal usage
- [ ] Graphics acceleration works properly

## 📚 Documentation Standards

### File Organization
- Keep documentation in the `docs/` folder
- Use clear, descriptive filenames
- Maintain consistent formatting

### Writing Style
- Use clear, concise language
- Include step-by-step instructions
- Provide screenshots for complex procedures
- Use proper markdown formatting

### Required Documentation Updates
- Update `HARDWARE.md` for new hardware support
- Modify `INSTALLATION.md` for process changes
- Update `README.md` for feature additions
- Add entries to `CHANGELOG.md` for all changes

## 🔒 Security and Privacy

- **Remove Personal Data**: Never include personal information in configurations
- **Sanitize Serials**: Use generic or placeholder serial numbers
- **Clean Logs**: Remove sensitive information from log files
- **Review Carefully**: Double-check all files before submission

## 💡 Getting Help

If you need help contributing:

1. **Check Documentation**: Review existing documentation first
2. **Search Issues**: Look for similar issues or questions
3. **Ask Questions**: Open an issue with the `question` label
4. **Join Community**: Participate in Hackintosh forums and communities

## 🏷️ Labels and Tags

We use the following labels for issues and PRs:

- `bug`: Something isn't working
- `enhancement`: New feature or request
- `documentation`: Improvements or additions to documentation
- `good first issue`: Good for newcomers
- `help wanted`: Extra attention is needed
- `question`: Further information is requested
- `hardware-specific`: Related to specific hardware configurations

## 📄 License

By contributing to this project, you agree that your contributions will be licensed under the same license as the project.

---

Thank you for contributing to the Hackintosh community! 🍎