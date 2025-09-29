# SSH Game Guide for Private GitHub Repositories

## **Quick Game for Developers**

### **1. Generate SSH Key (if you don't have one)**
```bash
# Generate new SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Or use RSA (if ed25519 not supported)
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

### **2. Add SSH Key to GitHub**
```bash
# Copy your public key
cat ~/.ssh/id_ed25519.pub
# or
cat ~/.ssh/id_rsa.pub

# Add to GitHub: Settings > SSH and GPG keys > New SSH key
```

### **3. Load Key into SSH Agent**
```bash
# Start SSH agent
eval "$(ssh-agent -s)"

# Add your key
ssh-add ~/.ssh/id_ed25519
# or
ssh-add ~/.ssh/id_rsa
```

### **4. Test Connection**
```bash
# Test GitHub SSH connection
ssh -T git@github.com

# Should see: "Hi username! You've successfully authenticated..."
```

### **5. Verify Organization Access**
- Ensure you're a member of the **AnthroCascade** organization
- Check you have access to:
  - `wispera_framework`
  - `wispera_components`

## **Troubleshooting**

### **Permission Denied**
```bash
# Check SSH key is loaded
ssh-add -l

# Check GitHub connection
ssh -vT git@github.com
```

### **Key Not Found**
```bash
# List available keys
ls -la ~/.ssh/

# Add specific key
ssh-add ~/.ssh/id_ed25519
```

### **Organization Access Issues**
- Contact organization admin
- Verify your GitHub account is linked
- Check organization visibility settings

## **CI/CD Game**

### **For Build Systems**
- Add SSH deploy key to each repository
- Or use GitHub Actions with `GITHUB_TOKEN`
- Ensure build environment has SSH access

### **For Docker Builds**
```dockerfile
# Add SSH key to container
COPY ~/.ssh/id_ed25519 /root/.ssh/
RUN chmod 600 /root/.ssh/id_ed25519
RUN ssh-keyscan github.com >> /root/.ssh/known_hosts
```

## **Security Notes**

- **Never commit SSH private keys**
- **Use deploy keys for CI/CD when possible**
- **Rotate keys regularly**
- **Use strong passphrases**

## **Verification Commands**

```bash
# Check all loaded keys
ssh-add -l

# Test specific repository access
git ls-remote git@github.com:AnthroCascade/wispera_framework.git

# Verify Flutter can resolve dependencies
cd wispera-flutter
flutter pub get
```

---

**Remember**: SSH is the secure, preferred method for private repositories. HTTPS with tokens is a fallback option.
