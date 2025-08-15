## API Reference

This document describes the externally visible classes and lifecycle functions in this app, along with examples and usage guidance.

- **Platform**: iOS (UIKit)
- **Language**: Swift
- **Primary modules/files**: `App3/AppDelegate.swift`, `App3/ViewController.swift`

### AppDelegate

Entry point for the application. Conforms to `UIApplicationDelegate` and coordinates app-level lifecycle.

```swift
class AppDelegate: UIResponder, UIApplicationDelegate {
    var window: UIWindow?

    func application(_ application: UIApplication,
                     didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool

    func applicationWillResignActive(_ application: UIApplication)
    func applicationDidEnterBackground(_ application: UIApplication)
    func applicationWillEnterForeground(_ application: UIApplication)
    func applicationDidBecomeActive(_ application: UIApplication)
    func applicationWillTerminate(_ application: UIApplication)
}
```

- **window**: The main `UIWindow` hosting the app's view hierarchy.
- **application(_:didFinishLaunchingWithOptions:) -> Bool**: Called once on launch. Configure global state and the initial UI here.
- **applicationWillResignActive(_:)**: App moving from active to inactive (e.g., incoming call). Pause ongoing tasks.
- **applicationDidEnterBackground(_:)**: App moved to background. Release shared resources, save user data, and invalidate timers.
- **applicationWillEnterForeground(_:)**: Transitioning from background to active. Undo changes made entering background.
- **applicationDidBecomeActive(_:)**: App became active. Restart paused tasks and refresh UI.
- **applicationWillTerminate(_:)**: App is about to terminate. Persist necessary state.

#### Example: Configure the window and root view controller

```swift
func application(_ application: UIApplication,
                 didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    window = UIWindow(frame: UIScreen.main.bounds)
    let root = ViewController()
    let nav = UINavigationController(rootViewController: root)
    window?.rootViewController = nav
    window?.makeKeyAndVisible()

    // Global appearance or service setup can go here
    // UINavigationBar.appearance().tintColor = .systemBlue

    return true
}
```

#### Example: Respond to backgrounding/foregrounding

```swift
func applicationDidEnterBackground(_ application: UIApplication) {
    // Save transient state, flush caches, schedule background tasks, etc.
}

func applicationWillEnterForeground(_ application: UIApplication) {
    // Refresh data or UI that may have become stale in the background.
}
```

### ViewController

Base screen provided by the template. Subclass of `UIViewController` that overrides `viewDidLoad()` for initial setup.

```swift
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        // Perform initial UI setup here
    }
}
```

- **viewDidLoad()**: Called after the controller's view is loaded into memory. Initialize views, set colors, create layout constraints, and bind data.

#### Example: Basic UI setup

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    title = "Home"
    view.backgroundColor = .systemBackground

    let label = UILabel()
    label.text = "Hello, App3!"
    label.textAlignment = .center
    label.translatesAutoresizingMaskIntoConstraints = false

    view.addSubview(label)

    NSLayoutConstraint.activate([
        label.centerXAnchor.constraint(equalTo: view.centerXAnchor),
        label.centerYAnchor.constraint(equalTo: view.centerYAnchor)
    ])
}
```

#### Example: Presenting `ViewController` programmatically

```swift
let viewController = ViewController()
let navigationController = UINavigationController(rootViewController: viewController)
window?.rootViewController = navigationController
window?.makeKeyAndVisible()
```

### Usage notes and best practices

- **Lifecycle**: Keep heavy work out of `viewDidLoad()` and prefer background queues for non-UI tasks. Keep UI updates on the main thread.
- **State management**: Save transient state when entering background and restore in foreground or on next launch.
- **Navigation**: Wrap root controllers in `UINavigationController` if you need push-style navigation.
- **Extensibility**: Add new screens by subclassing `UIViewController` or using `UITableViewController`/`UICollectionViewController` as appropriate.

### Extending the app

- **Adding a new screen**:
  1. Create `NewFeatureViewController: UIViewController`.
  2. Push it from an existing controller: `navigationController?.pushViewController(NewFeatureViewController(), animated: true)`.
  3. Or present modally: `present(NewFeatureViewController(), animated: true)`.
- **Global theming**: Use `UIAppearance` proxies in `didFinishLaunchingWithOptions` for consistent styling.

### File inventory

- `App3/AppDelegate.swift`: App lifecycle and bootstrap.
- `App3/ViewController.swift`: Base example screen.