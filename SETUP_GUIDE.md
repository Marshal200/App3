# App3 - Setup and Installation Guide

## Table of Contents
- [System Requirements](#system-requirements)
- [Installation Steps](#installation-steps)
- [Project Configuration](#project-configuration)
- [Building and Running](#building-and-running)
- [Troubleshooting](#troubleshooting)
- [Development Environment Setup](#development-environment-setup)
- [Deployment](#deployment)

## System Requirements

### Development Environment
- **macOS:** 10.14.4 (Mojave) or later
- **Xcode:** 10.1 or later (recommended: latest stable version)
- **iOS Deployment Target:** 12.0 or later
- **Swift:** 5.0 or later
- **Hardware:** Mac with Intel or Apple Silicon processor

### Target Devices
- **iPhone:** iOS 12.0+, all screen sizes supported
- **iPad:** iOS 12.0+, all orientations supported
- **Device Capabilities:** ARM7 processor (minimum)

### Apple Developer Account
- **For Simulator:** Not required
- **For Device Testing:** Apple Developer Account (free or paid)
- **For App Store:** Paid Apple Developer Program membership ($99/year)

## Installation Steps

### 1. Prerequisites Installation

#### Install Xcode
1. **From App Store (Recommended):**
   ```bash
   # Open App Store and search for "Xcode"
   # Click "Get" or "Install"
   ```

2. **From Apple Developer Portal:**
   - Visit [developer.apple.com](https://developer.apple.com)
   - Download Xcode from Downloads section
   - Install the .xip file

3. **Verify Installation:**
   ```bash
   xcode-select --version
   # Should output: xcode-select version 2XXX or later
   ```

#### Install Command Line Tools
```bash
xcode-select --install
```

### 2. Project Setup

#### Clone/Download Project
```bash
# If using Git
git clone [repository-url]
cd App3

# Or download and extract ZIP file
# Navigate to extracted folder in Terminal
cd path/to/App3
```

#### Verify Project Structure
```bash
ls -la
# Expected output:
# App3/
# App3.xcodeproj/
# .git/ (if cloned from Git)
```

### 3. Asset Configuration

#### Required Assets
The project requires the following assets to be present:

1. **Howling King Image:**
   - **Location:** `App3/Assets.xcassets/`
   - **Dimensions:** 1920x1080 (recommended)
   - **Format:** PNG, JPEG, or HEIF
   - **Name:** "Howling King"

#### Adding Missing Assets
If the "Howling King" image is missing:

1. **Open Assets.xcassets in Xcode:**
   ```bash
   open App3.xcodeproj
   # Navigate to App3 > Assets.xcassets in Project Navigator
   ```

2. **Add Image Asset:**
   - Right-click in Assets.xcassets
   - Select "New Image Set"
   - Name it "Howling King"
   - Drag your image files into the appropriate slots (1x, 2x, 3x)

3. **Alternative: Replace with System Image:**
   ```swift
   // In ViewController.swift or storyboard
   // Replace "Howling King" with system image
   backgroundImageView.image = UIImage(systemName: "photo.fill")
   ```

## Project Configuration

### 1. Bundle Identifier Setup

#### For Simulator Testing (Basic Setup)
1. Open `App3.xcodeproj`
2. Select project root in Navigator
3. Under "General" tab:
   - **Display Name:** App3 (or your preferred name)
   - **Bundle Identifier:** `com.yourname.App3`
   - **Version:** 1.0
   - **Build:** 1

#### For Device Testing
1. **Apple Developer Account Required**
2. **Team Selection:**
   - Under "Signing & Capabilities"
   - Select your development team
   - Enable "Automatically manage signing"

3. **Bundle Identifier:**
   - Must be unique (e.g., `com.yourname.app3.unique`)
   - Cannot conflict with existing apps

### 2. Deployment Target Configuration

```swift
// Minimum supported iOS version
iOS Deployment Target: 12.0

// Supported device families
Device Family: iPhone/iPad (Universal)

// Supported orientations
iPhone: Portrait, Landscape Left, Landscape Right
iPad: All orientations
```

### 3. Build Settings Verification

#### Key Settings to Verify:
```
Base SDK: iOS (latest)
iOS Deployment Target: 12.0
Supported Platforms: iOS
Valid Architectures: arm64, armv7
Build Active Architecture Only: No (for Release)
Swift Language Version: 5.0
```

## Building and Running

### 1. Simulator Testing

#### Select Target Device
```bash
# Open Xcode
open App3.xcodeproj

# In Xcode:
# 1. Select scheme: App3
# 2. Select destination: Choose iOS simulator
#    - iPhone 14 Pro (recommended for testing)
#    - iPad Air (for tablet testing)
```

#### Build and Run
```bash
# Keyboard shortcut in Xcode
Cmd + R

# Or from Product menu
Product > Run
```

#### Alternative: Command Line Build
```bash
# Navigate to project directory
cd /path/to/App3

# Build for simulator
xcodebuild -project App3.xcodeproj -scheme App3 -sdk iphonesimulator -destination 'platform=iOS Simulator,name=iPhone 14 Pro,OS=latest'

# Run in simulator (requires additional setup)
xcrun simctl boot "iPhone 14 Pro"
xcrun simctl install booted /path/to/App3.app
xcrun simctl launch booted com.yourname.App3
```

### 2. Device Testing

#### Prerequisites
- Apple Developer Account
- Device registered in Developer Portal
- Valid provisioning profile

#### Device Configuration
1. **Connect Device via USB**
2. **Trust Computer:**
   - On device: Settings > General > Device Management
   - Trust the developer certificate

3. **Enable Developer Mode (iOS 16+):**
   - Settings > Privacy & Security > Developer Mode
   - Toggle on and restart device

#### Build for Device
```bash
# In Xcode:
# 1. Select your connected device as destination
# 2. Ensure signing is configured
# 3. Build and Run (Cmd + R)
```

### 3. Build Configurations

#### Debug Build (Default)
- Optimizations disabled
- Debug symbols included
- Faster build times
- Larger app size

#### Release Build
```bash
# In Xcode:
# Product > Scheme > Edit Scheme
# Set "Build Configuration" to "Release"
# Or use Archive for distribution
```

## Troubleshooting

### Common Issues and Solutions

#### 1. Build Errors

**Issue:** "No such file or directory: Howling King"
```bash
# Solution: Add the missing image asset
# See "Asset Configuration" section above
```

**Issue:** "Code signing error"
```bash
# Solution: Configure signing
# 1. Xcode > Preferences > Accounts
# 2. Add Apple ID
# 3. Project settings > Signing & Capabilities
# 4. Select team and enable automatic signing
```

**Issue:** "Swift Compiler Error"
```bash
# Solution: Clean build folder
# Product > Clean Build Folder (Cmd + Shift + K)
# Then rebuild (Cmd + B)
```

#### 2. Runtime Issues

**Issue:** App crashes on launch
```bash
# Debug steps:
# 1. Check console for error messages
# 2. Verify all IBOutlets are properly connected
# 3. Ensure images exist in bundle
# 4. Check for force unwrapping of nil values
```

**Issue:** Interface not displaying correctly
```bash
# Solution: Verify storyboard constraints
# 1. Open Main.storyboard
# 2. Check for constraint conflicts (red/orange warnings)
# 3. Update frames if needed
```

#### 3. Device-Specific Issues

**Issue:** "App installation failed"
```bash
# Solutions:
# 1. Verify device is unlocked
# 2. Trust developer certificate on device
# 3. Check available storage space
# 4. Restart device and Xcode
```

**Issue:** Performance issues on older devices
```bash
# Optimization steps:
# 1. Reduce image resolution for older devices
# 2. Test on minimum supported iOS version
# 3. Profile using Instruments
```

### Debug Tools

#### Xcode Debugger
```bash
# Set breakpoints in code
# Run with debugger attached
# Inspect variables and call stack
```

#### Console Logging
```swift
// Add debug logging
print("Debug: viewDidLoad called")
NSLog("Error loading image: %@", error.localizedDescription)
```

#### Instruments Profiling
```bash
# Product > Profile (Cmd + I)
# Choose appropriate instrument:
# - Time Profiler (for performance)
# - Leaks (for memory issues)
# - Energy Log (for battery usage)
```

## Development Environment Setup

### 1. Xcode Configuration

#### Recommended Preferences
```bash
# Xcode > Preferences > Text Editing
# Enable: Line numbers, Code folding, Page guide at column 80
# Indentation: 4 spaces, Convert tabs to spaces

# Xcode > Preferences > Behaviors
# Configure behaviors for build success/failure
# Set up breakpoint behavior
```

#### Useful Keyboard Shortcuts
```bash
Cmd + R          # Run
Cmd + B          # Build
Cmd + Shift + K  # Clean
Cmd + Shift + O  # Open Quickly
Cmd + /          # Toggle Comment
Cmd + [          # Shift Left
Cmd + ]          # Shift Right
```

### 2. Version Control Setup

#### Git Configuration
```bash
# Initialize Git repository (if not already done)
git init
git add .
git commit -m "Initial commit"

# Add remote repository
git remote add origin [your-repo-url]
git push -u origin main
```

#### .gitignore Setup
```bash
# Create .gitignore file
cat > .gitignore << EOF
# Xcode
.DS_Store
*/build/*
*.pbxuser
!default.pbxuser
*.mode1v3
!default.mode1v3
*.mode2v3
!default.mode2v3
*.perspectivev3
!default.perspectivev3
xcuserdata/
profile
*.moved-aside
DerivedData
.idea/
*.hmap
*.xccheckout
*.xcworkspace
!default.xcworkspace

# CocoaPods (if used in future)
Pods/
Podfile.lock
EOF
```

### 3. Documentation Setup

#### Code Documentation
```swift
/// Main view controller for the app
/// Displays a full-screen image with overlay text
class ViewController: UIViewController {
    
    /// Called after the view loads
    /// - Note: Override to perform additional setup
    override func viewDidLoad() {
        super.viewDidLoad()
        // Implementation
    }
}
```

#### Quick Help Documentation
```bash
# In Xcode:
# Option + Click on any symbol for Quick Help
# Cmd + Option + 2 for Quick Help Inspector
```

## Deployment

### 1. App Store Preparation

#### Archive Build
```bash
# In Xcode:
# 1. Select "Any iOS Device" as destination
# 2. Product > Archive
# 3. Wait for build to complete
# 4. Organizer window will open
```

#### App Store Connect Setup
1. **Create App Record:**
   - Visit [appstoreconnect.apple.com](https://appstoreconnect.apple.com)
   - Create new app with unique bundle identifier

2. **Upload Build:**
   - From Xcode Organizer
   - Click "Distribute App"
   - Choose "App Store Connect"

#### Required Assets for App Store
```bash
# App Icons (required sizes):
20x20, 29x29, 40x40, 58x58, 60x60, 76x76, 80x80, 87x87, 120x120, 152x152, 167x167, 180x180, 1024x1024

# Screenshots (required):
iPhone: 6.7", 6.5", 5.5", 4.7"
iPad: 12.9", 11"
```

### 2. TestFlight Beta Testing

#### Internal Testing
```bash
# Upload build to App Store Connect
# Add internal testers (up to 100)
# Testers receive email invitation
# No review required
```

#### External Testing
```bash
# Requires Beta App Review
# Up to 10,000 external testers
# Public link or email invitations
# 1-3 day review process
```

### 3. Enterprise/Ad Hoc Distribution

#### Enterprise Distribution
```bash
# Requires Enterprise Developer Account ($299/year)
# Internal distribution only
# No App Store review required
```

#### Ad Hoc Distribution
```bash
# Up to 100 registered devices
# Archive > Distribute App > Ad Hoc
# Generate .ipa file for direct installation
```

---

This setup guide provides comprehensive instructions for getting App3 running on both simulators and devices, from initial installation through App Store deployment. Follow the sections relevant to your needs, and refer to the troubleshooting section if you encounter any issues.