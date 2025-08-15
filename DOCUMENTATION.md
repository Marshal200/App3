## App3 iOS Application Documentation

### Overview
App3 is a minimal iOS application written in Swift. It contains two primary components:

1. **`AppDelegate`** – Handles global application lifecycle events.
2. **`ViewController`** – Manages the initial user interface presented at launch.

This document provides detailed information on the public APIs exposed by these components, along with usage examples to help you extend the application.

---

## 1. `AppDelegate`
Located at `App3/AppDelegate.swift`

### Public Properties
| Property | Type | Description |
|----------|------|-------------|
| `window` | `UIWindow?` | Main application window. Automatically provided by Xcode template; you can replace or customize it if your app uses scenes or custom windowing logic. |

### Lifecycle Methods (Public APIs)
| Method | Called When | Typical Use-cases |
|--------|-------------|--------------------|
| `application(_:didFinishLaunchingWithOptions:)` | Just after the app has launched. | • App-wide configuration (e.g., analytics initialisation)<br>• Setting root view controller programmatically |
| `applicationWillResignActive(_)` | App is moving from active to inactive (e.g., incoming call). | • Pause ongoing tasks<br>• Disable timers |
| `applicationDidEnterBackground(_)` | App entered background. | • Release shared resources<br>• Persist user data |
| `applicationWillEnterForeground(_)` | App is transitioning from background to foreground. | • Undo changes made when entering background |
| `applicationDidBecomeActive(_)` | App became active again. | • Restart paused tasks<br>• Refresh UI |
| `applicationWillTerminate(_)` | App is about to terminate. | • Final cleanup and data saving |

### Example – Customising Launch Logic
```swift
func application(_ application: UIApplication,
                 didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // 1️⃣ Configure analytics
    AnalyticsService.shared.start()

    // 2️⃣ Inject dependencies
    let rootVC = ViewController()
    window = UIWindow(frame: UIScreen.main.bounds)
    window?.rootViewController = rootVC
    window?.makeKeyAndVisible()

    return true
}
```

---

## 2. `ViewController`
Located at `App3/ViewController.swift`

`ViewController` is the default root view controller of the application. It inherits from `UIViewController` and overrides the following lifecycle method:

| Method | Purpose |
|--------|---------|
| `viewDidLoad()` | Called after the controller’s view is loaded into memory. Ideal place to perform additional setup, network calls, or view configuration. |

### Example – Adding a Label Programmatically
```swift
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .systemBackground

        let label = UILabel()
        label.text = "Hello, App3!"
        label.font = .systemFont(ofSize: 24, weight: .semibold)
        label.translatesAutoresizingMaskIntoConstraints = false
        view.addSubview(label)

        NSLayoutConstraint.activate([
            label.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            label.centerYAnchor.constraint(equalTo: view.centerYAnchor)
        ])
    }
}
```

---

## 3. Project Assets & Storyboards
The project includes default Xcode template resources:

- **`Assets.xcassets`** – Stores app icons and image assets.
- **`Base.lproj/Main.storyboard`** – Interface Builder storyboard (currently unused if you build UI programmatically).
- **`Info.plist`** – Application-specific configuration (bundle identifier, version, permissions, etc.).

You can safely remove the storyboard and switch to a pure SwiftUI or programmatic UIKit approach by deleting `Main.storyboard` and removing the `Main storyboard file base name` entry in `Info.plist`. Then, set the root view controller in `AppDelegate` as shown above.

---

## 4. Building & Running
1. Open `App3.xcodeproj` in Xcode.
2. Ensure the **App3** target is selected.
3. Choose an iOS simulator/device.
4. Press **⌘R** (Run) to build and launch.

---

## 5. Extending the App
Here are common extension points:

1. **Adding More Screens** – Create new `UIViewController` subclasses and push/present them from `ViewController`.
2. **Networking Layer** – Introduce URLSession‐based services or third-party libraries (e.g., Alamofire).
3. **Persistence** – Integrate Core Data or UserDefaults in `applicationDidEnterBackground(_)` to persist state.

---

## 6. Contributing
1. Fork the repository and create a feature branch.
2. Ensure all Swift files include documentation comments (`///`).
3. Submit a pull request with descriptive title and description.

---

© 2019 Cognizant. Licensed under the MIT License (see `LICENSE` for details).