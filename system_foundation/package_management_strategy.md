# Package Management Strategy for Wispera Games

## **Core Principle: Stable GitHub Repositories Are Default State**

**Local path references are ALWAYS tactical temporary states for framework development only. The stable, committed state of all pubspec files must point to GitHub repositories within your private organization.**

**This applies to ALL repositories that depend on your packages, not just the packages themselves.**

## **Architecture Overview**

### **Three-Tier Package Structure**
```
wispera-flutter (Main Application - Not Publishable)
├── wispera_framework (Core Framework - Internal Package)
└── wispera_components (Business Components - Internal Package)
```

### **Package Publishing Status**
- **wispera-flutter**: `publish_to: 'none'` - Main application, never published
- **wispera_framework**: `publish_to: 'none'` - Internal framework, never published
- **wispera_components**: `publish_to: 'none'` - Internal components, never published

**Note**: All packages are marked as non-publishable because they contain internal organization code and git dependencies, which are not allowed for public packages.

### **Dependency Flow**
1. **Main App** (`wispera-flutter`) depends on both packages
2. **Components** (`wispera_components`) depends on framework
3. **Framework** (`wispera_framework`) has no internal dependencies

## **Stable State Configuration**

### **wispera-flutter/pubspec.yaml (Default State)**
```yaml
dependencies:
  wispera_framework:
    git:
      url: git@github.com:AnthroCascade/wispera_framework.git
      ref: main
      # Note: This is a private repository within your organization
      # Uses SSH authentication (preferred method)
  wispera_components:
    git:
      url: git@github.com:AnthroCascade/wispera_components.git
      ref: main
      # Note: This is a private repository within your organization
      # Uses SSH authentication (preferred method)
```

### **wispera_components/pubspec.yaml (Default State)**
```yaml
dependencies:
  wispera_framework:
    git:
      url: git@github.com:AnthroCascade/wispera_framework.git
      ref: main
      # Note: This is a private repository within your organization
```

## **Local Development Workflow**

### **When to Use Local Paths**
- **Framework Development**: When actively developing `wispera_framework`
- **Component Development**: When actively developing `wispera_components`
- **Integration Testing**: When testing changes across packages
- **Debugging**: When investigating cross-package issues
- **Cross-Repository Development**: When developing multiple repositories simultaneously

### **Local Development Configuration Files**
- **pubspec.local.yaml**: Contains local path references for development
- **pubspec.yaml**: Contains stable GitHub references for production
- **Development Scripts**: Automatically switch between configurations
- **Never Committed**: Local configuration files should never be committed

### **Local Development Scripts**
- **use_local_framework.sh**: Switch to local framework only
- **use_local_components.sh**: Switch to local components only
- **use_local_all.sh**: Switch to local development for all packages
- **use_remote_framework.sh**: Switch back to stable framework
- **use_remote_all.sh**: Switch back to stable for all packages

### **When NOT to Use Local Paths**
- **Regular Development**: When working on the main application
- **Production Builds**: All production builds must use stable versions
- **Team Collaboration**: When sharing code with other developers
- **CI/CD Pipelines**: All automated builds must use stable versions

## **Development Mode Switching**

### **Framework Development Mode**
```bash
# Switch to local framework development
./scripts/use_local_framework.sh

# Switch back to stable
./scripts/use_remote_framework.sh
```

### **Component Development Mode**
```bash
# Use local development script (recommended)
./scripts/use_local_components.sh

# Or manually edit pubspec.yaml for local components
wispera_components:
  path: ../wispera_components

# Remember to revert before committing!
```

### **Cross-Repository Development Mode**
```bash
# Switch to local development for all packages
./scripts/use_local_all.sh

# Switch back to stable for all packages
./scripts/use_remote_all.sh
```

## **Private GitHub Repository Management**

### **Repository Structure**
- **Main Branch**: `main` - always stable, production-ready
- **Development Branch**: `develop` - integration testing
- **Feature Branches**: `feature/*` - individual feature development
- **Release Branches**: `release/*` - release preparation

### **Private Repository Considerations**
- **Access Control**: Only organization members can access these repositories
- **SSH Keys**: Developers must have proper SSH access configured (preferred method)
- **CI/CD Integration**: Build systems must have SSH keys or deploy keys configured
- **Dependency Resolution**: Flutter must be able to authenticate via SSH with GitHub

### **SSH Game Requirements**
- **SSH Key Generation**: Each developer needs SSH key pair
- **GitHub Account**: SSH public key added to GitHub account
- **SSH Agent**: Keys loaded into SSH agent (`ssh-add`)
- **Organization Access**: Developer must be member of AnthroCascade organization
- **Test Connection**: Verify with `ssh -T git@github.com`

### **Version Management**
- **Semantic Versioning**: Follow semver for all packages
- **Tagged Releases**: Each stable version gets a git tag
- **Changelog**: Maintain CHANGELOG.md for all packages
- **Breaking Changes**: Major version bumps for breaking changes

## **Quality Gates**

### **Before Publishing to GitHub**
1. **All Tests Pass**: Unit tests, integration tests, widget tests
2. **Analysis Clean**: No linter warnings or errors
3. **Documentation Updated**: README, API docs, examples
4. **Version Bumped**: Appropriate version increment
5. **Changelog Updated**: Document all changes

### **Before Switching to Local Development**
1. **Current State Committed**: All changes committed to stable state
2. **Local Changes Ready**: Local development changes are prepared
3. **Team Notified**: Other developers aware of local development mode

## **Emergency Procedures**

### **If Local Development Breaks Stable State**
1. **Immediate Revert**: Use `use_remote_framework.sh` script
2. **Git Reset**: `git checkout pubspec.yaml` to restore stable state
3. **Dependency Refresh**: `flutter pub get` to restore stable dependencies
4. **Investigation**: Identify what caused the break

### **If GitHub Repository Issues**
1. **Fallback to Local**: Temporarily use local paths
2. **Issue Documentation**: Document the problem and workaround
3. **Repository Fix**: Fix the GitHub repository issue
4. **Return to Stable**: Switch back to GitHub references

### **If Authentication Issues**
1. **SSH Key Verification**: Check developer SSH key configuration
2. **Organization Access**: Verify developer has access to private repositories
3. **SSH Key Game**: Ensure SSH keys are added to GitHub account and agent
4. **Network/Firewall**: Check for corporate network restrictions (SSH port 22)

## **Cross-Repository Dependency Management**

### **Repository Dependencies**
- **Main Application**: `wispera-flutter` depends on both packages
- **Framework Package**: `wispera_framework` has no internal dependencies
- **Components Package**: `wispera_components` depends on framework
- **External Repositories**: Any repo using your packages must follow same strategy

### **External Repository Requirements**
When other repositories (internal or external) depend on your packages:
1. **Use GitHub references** in their stable pubspec files
2. **Follow same local development pattern** if they need local versions
3. **Coordinate development** to avoid dependency conflicts
4. **Maintain consistent versioning** across all dependent repositories

### **Dependency Resolution Strategy**
1. **Stable State**: All pubspec files point to GitHub repositories
2. **Local Development**: Use local paths only during active development
3. **Cross-Repository**: Coordinate local development across multiple repos
4. **Return to Stable**: Always revert to GitHub references before committing

## **Team Guidelines**

### **Developer Responsibilities**
- **Never commit local path references** in main pubspec files
- **Always test stable state** before pushing changes
- **Use local development scripts** when developing packages
- **Communicate local development mode** to team members
- **Coordinate cross-repository development** with team

### **Code Review Requirements**
- **Verify pubspec.yaml** points to GitHub repositories
- **Check for local path references** in committed files
- **Ensure version compatibility** between packages
- **Validate dependency tree** for circular references

## **Monitoring and Enforcement**

### **Automated Checks**
- **Pre-commit Hooks**: Verify pubspec files don't contain local paths
- **CI/CD Validation**: Ensure all builds use stable dependencies
- **Dependency Scanning**: Regular checks for outdated or insecure packages

### **Manual Reviews**
- **Weekly Dependency Audit**: Review all package dependencies
- **Monthly Version Review**: Check for outdated package versions
- **Quarterly Strategy Review**: Update this strategy document

## **Success Metrics**

### **Stability Metrics**
- **Zero Production Issues** caused by local path references
- **100% CI/CD Success Rate** using stable dependencies
- **Consistent Build Times** across all environments

### **Development Efficiency**
- **Faster Local Development** through proper tooling
- **Reduced Integration Issues** through stable dependencies
- **Better Team Collaboration** through clear development modes

## **Conclusion**

This strategy ensures that:
1. **Production stability** is maintained through stable GitHub references
2. **Local development** is supported through proper tooling and workflows
3. **Team collaboration** is enhanced through clear guidelines and procedures
4. **Code quality** is maintained through automated checks and manual reviews

**Remember: Local paths are tactical, GitHub repositories are strategic. Always return to stable state before committing.**
