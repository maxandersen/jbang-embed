# JBang Embed - Cross-Platform Binary Builder

This project automatically builds cross-platform native executables from JBang scripts using [caxa](https://github.com/leafac/caxa). Simply configure your script reference in a `.env` file, open a pull request, and GitHub Actions will build binaries for Linux, macOS (ARM and Intel), and Windows.

## 🚀 How It Works

1. **Configure**: Set your JBang script reference and output name in a `.env` file
2. **Open PR**: Create a pull request with your changes
3. **Build**: GitHub Actions automatically builds binaries for all platforms
4. **Download**: Get your platform-specific binaries from the workflow artifacts

## 📋 Prerequisites

- A GitHub account
- A JBang script reference (e.g., `myapp@latest` or `myapp@group:artifact:version`)

## 🛠️ Setup

### 1. Edit the `.env` file

Edit the `.env` file in the root of this repository with the following variables:

```bash
OUTPUT_NAME=myapp
SCRIPT_REF=myapp@latest
```

**Variables:**
- `OUTPUT_NAME`: The name of the output executable (without extension)
- `SCRIPT_REF`: The JBang script reference (e.g., `jvminsight@mcp-java`)

### 2. Open a Pull Request

1. Fork this repository
2. Create a new branch
3. Add or modify the `.env` file with your configuration
4. Commit and push your changes
5. Open a pull request targeting the `main` branch

### 3. Wait for Build

Once your PR is opened, GitHub Actions will automatically:

1. **Prepare** (Linux runner):
   - Checkout the repository
   - Setup Java 21
   - Setup JBang
   - Install the JBang wrapper
   - Prepare the build content

2. **Build** (Matrix: Ubuntu, macOS ARM, macOS Intel, Windows):
   - Read your `.env` configuration
   - Setup Node.js
   - Download the prepared content
   - Build platform-specific native executables using caxa
   - Upload artifacts for each platform

### 4. Download Binaries

After the workflow completes:

1. Go to the **Checks** tab in your pull request
2. Click on the workflow run
3. Scroll down to the **Artifacts** section
4. Download the binary for your platform:
   - `myapp-Linux-x64` - Linux executable
   - `myapp-macOS-arm64` - macOS Apple Silicon
   - `myapp-macOS-x64` - macOS Intel
   - `myapp-Windows-x64` - Windows executable (.exe)

## 📦 Supported Platforms

| Platform | Runner | Architecture | Output |
|----------|--------|--------------|--------|
| Linux | `ubuntu-latest` | x64 | `myapp` |
| macOS | `macos-latest` | ARM64 (Apple Silicon) | `myapp` |
| macOS | `macos-13` | x64 (Intel) | `myapp` |
| Windows | `windows-latest` | x64 | `myapp.exe` |


## 🏗️ Architecture

The workflow uses a two-stage build process:

1. **Prepare Job** (runs once on Linux):
   - Sets up the build environment
   - Installs JBang wrapper dependencies
   - Uploads prepared content as an artifact

2. **Build Job** (runs in parallel on all platforms):
   - Downloads the prepared content
   - Reads configuration from `.env`
   - Builds platform-specific native executables
   - Uploads platform-specific artifacts

This approach ensures:
- ✅ Consistent build environment
- ✅ Faster builds (preparation happens once)
- ✅ Parallel platform builds
- ✅ Platform-specific optimizations

## 🐛 Troubleshooting

### Build fails with "OUTPUT_NAME not found"
- Ensure your `.env` file contains `OUTPUT_NAME=yourname`
- Check that the file is committed to your branch

### Build fails with "SCRIPT_REF not found"
- Ensure your `.env` file contains `SCRIPT_REF=yourscript@version`
- Verify the JBang script reference is valid

### Artifact not found
- Check that the workflow completed successfully
- Ensure you're looking at the correct workflow run
- Artifacts are available for 90 days after the workflow run

## 📚 Learn More

- [JBang Documentation](https://www.jbang.dev/)
- [caxa Documentation](https://github.com/leafac/caxa)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

---

**Happy Building! 🎉**
