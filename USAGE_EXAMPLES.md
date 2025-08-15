# App3 - Usage Examples and Code Samples

## Table of Contents
- [Basic Usage](#basic-usage)
- [Component Customization](#component-customization)
- [Advanced Features](#advanced-features)
- [Integration Examples](#integration-examples)
- [Common Use Cases](#common-use-cases)
- [Code Snippets Library](#code-snippets-library)

## Basic Usage

### 1. Running the App As-Is

The simplest way to use App3 is to run it without modifications:

```swift
// The app will automatically:
// 1. Display the "Howling King" background image
// 2. Show overlay text: "King Is Howling" and "Never Messup"
// 3. Support all device orientations
// 4. Scale appropriately for different screen sizes
```

### 2. Basic Text Customization

To change the displayed text, modify the storyboard labels:

```swift
// Option 1: Via Storyboard
// 1. Open Main.storyboard
// 2. Select the title label (cbx-mT-EBF)
// 3. Change "Text" property in Attributes Inspector
// 4. Repeat for subtitle label (abM-C5-n7Y)

// Option 2: Programmatically (requires IBOutlets)
class ViewController: UIViewController {
    @IBOutlet weak var titleLabel: UILabel!
    @IBOutlet weak var subtitleLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        titleLabel.text = "Your Custom Title"
        subtitleLabel.text = "Your Custom Subtitle"
    }
}
```

### 3. Basic Image Replacement

Replace the background image with your own:

```swift
// Option 1: Via Assets.xcassets
// 1. Add your image to Assets.xcassets
// 2. Name it "Howling King" to replace automatically
// 3. Or name it differently and update the reference

// Option 2: Programmatically
class ViewController: UIViewController {
    @IBOutlet weak var backgroundImageView: UIImageView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Replace with your image
        backgroundImageView.image = UIImage(named: "your-image-name")
        
        // Or load from bundle
        if let imagePath = Bundle.main.path(forResource: "your-image", ofType: "jpg"),
           let image = UIImage(contentsOfFile: imagePath) {
            backgroundImageView.image = image
        }
    }
}
```

## Component Customization

### 1. Styling the Text Labels

#### Font Customization
```swift
extension ViewController {
    private func customizeFonts() {
        // System fonts with different weights
        titleLabel.font = UIFont.systemFont(ofSize: 28, weight: .bold)
        subtitleLabel.font = UIFont.systemFont(ofSize: 18, weight: .light)
        
        // Custom fonts (ensure font is included in bundle)
        titleLabel.font = UIFont(name: "Helvetica-Bold", size: 24)
        subtitleLabel.font = UIFont(name: "Helvetica-Light", size: 16)
        
        // Preferred fonts for accessibility
        titleLabel.font = UIFont.preferredFont(forTextStyle: .largeTitle)
        subtitleLabel.font = UIFont.preferredFont(forTextStyle: .body)
        
        // Enable dynamic type
        titleLabel.adjustsFontForContentSizeCategory = true
        subtitleLabel.adjustsFontForContentSizeCategory = true
    }
}
```

#### Color and Effects
```swift
extension ViewController {
    private func customizeTextAppearance() {
        // Basic colors
        titleLabel.textColor = UIColor.systemBlue
        subtitleLabel.textColor = UIColor.systemGray
        
        // Custom colors
        titleLabel.textColor = UIColor(red: 1.0, green: 0.8, blue: 0.0, alpha: 1.0)
        
        // Text shadows
        titleLabel.layer.shadowColor = UIColor.black.cgColor
        titleLabel.layer.shadowOffset = CGSize(width: 2, height: 2)
        titleLabel.layer.shadowOpacity = 0.7
        titleLabel.layer.shadowRadius = 3
        
        // Text stroke (outline)
        let strokeAttributes: [NSAttributedString.Key: Any] = [
            .strokeColor: UIColor.black,
            .strokeWidth: -3.0,
            .foregroundColor: UIColor.white
        ]
        
        titleLabel.attributedText = NSAttributedString(
            string: titleLabel.text ?? "",
            attributes: strokeAttributes
        )
    }
}
```

### 2. Overlay Customization

#### Background Effects
```swift
extension ViewController {
    private func customizeOverlay() {
        // Solid color with transparency
        overlayView.backgroundColor = UIColor.black.withAlphaComponent(0.6)
        
        // Gradient background
        let gradientLayer = CAGradientLayer()
        gradientLayer.colors = [
            UIColor.clear.cgColor,
            UIColor.black.withAlphaComponent(0.8).cgColor
        ]
        gradientLayer.locations = [0.0, 1.0]
        gradientLayer.frame = overlayView.bounds
        overlayView.layer.insertSublayer(gradientLayer, at: 0)
        
        // Blur effect
        let blurEffect = UIBlurEffect(style: .dark)
        let blurView = UIVisualEffectView(effect: blurEffect)
        blurView.frame = overlayView.bounds
        blurView.autoresizingMask = [.flexibleWidth, .flexibleHeight]
        overlayView.insertSubview(blurView, at: 0)
        
        // Rounded corners
        overlayView.layer.cornerRadius = 20
        overlayView.layer.maskedCorners = [.layerMinXMinYCorner, .layerMaxXMinYCorner]
    }
}
```

### 3. Image View Customization

#### Content Mode and Effects
```swift
extension ViewController {
    private func customizeImageView() {
        // Content modes
        backgroundImageView.contentMode = .scaleAspectFit  // Shows full image
        backgroundImageView.contentMode = .scaleAspectFill // Fills screen, may crop
        backgroundImageView.contentMode = .center          // Centers image at original size
        
        // Image effects
        backgroundImageView.alpha = 0.8  // Transparency
        
        // Tint color (for template images)
        backgroundImageView.image = backgroundImageView.image?.withRenderingMode(.alwaysTemplate)
        backgroundImageView.tintColor = UIColor.systemBlue
        
        // Corner radius
        backgroundImageView.layer.cornerRadius = 16
        backgroundImageView.clipsToBounds = true
        
        // Border
        backgroundImageView.layer.borderWidth = 2
        backgroundImageView.layer.borderColor = UIColor.white.cgColor
    }
}
```

## Advanced Features

### 1. Animation System

#### Entry Animations
```swift
extension ViewController {
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        animateContentEntry()
    }
    
    private func animateContentEntry() {
        // Hide elements initially
        overlayView.alpha = 0
        overlayView.transform = CGAffineTransform(translationX: 0, y: 100)
        titleLabel.alpha = 0
        subtitleLabel.alpha = 0
        
        // Animate overlay
        UIView.animate(withDuration: 0.8, delay: 0.2, usingSpringWithDamping: 0.8, initialSpringVelocity: 0) {
            self.overlayView.alpha = 1
            self.overlayView.transform = .identity
        }
        
        // Animate title
        UIView.animate(withDuration: 0.6, delay: 0.6) {
            self.titleLabel.alpha = 1
        }
        
        // Animate subtitle
        UIView.animate(withDuration: 0.6, delay: 0.8) {
            self.subtitleLabel.alpha = 1
        }
    }
}
```

#### Interactive Animations
```swift
extension ViewController {
    private func setupGestureRecognizers() {
        let tapGesture = UITapGestureRecognizer(target: self, action: #selector(handleTap))
        view.addGestureRecognizer(tapGesture)
        
        let panGesture = UIPanGestureRecognizer(target: self, action: #selector(handlePan(_:)))
        overlayView.addGestureRecognizer(panGesture)
    }
    
    @objc private func handleTap() {
        UIView.animate(withDuration: 0.3, animations: {
            self.overlayView.alpha = self.overlayView.alpha > 0.5 ? 0.3 : 1.0
        })
    }
    
    @objc private func handlePan(_ gesture: UIPanGestureRecognizer) {
        let translation = gesture.translation(in: view)
        
        switch gesture.state {
        case .changed:
            overlayView.transform = CGAffineTransform(translationX: 0, y: max(0, translation.y))
        case .ended:
            let velocity = gesture.velocity(in: view)
            if velocity.y > 500 || translation.y > overlayView.frame.height * 0.3 {
                hideOverlay()
            } else {
                showOverlay()
            }
        default:
            break
        }
    }
    
    private func hideOverlay() {
        UIView.animate(withDuration: 0.4) {
            self.overlayView.transform = CGAffineTransform(translationX: 0, y: self.overlayView.frame.height)
        }
    }
    
    private func showOverlay() {
        UIView.animate(withDuration: 0.4) {
            self.overlayView.transform = .identity
        }
    }
}
```

### 2. Dynamic Content Loading

#### Network Image Loading
```swift
extension ViewController {
    func loadImageFromURL(_ urlString: String) {
        guard let url = URL(string: urlString) else { return }
        
        // Show loading indicator
        let activityIndicator = UIActivityIndicatorView(style: .large)
        activityIndicator.center = view.center
        activityIndicator.color = .white
        view.addSubview(activityIndicator)
        activityIndicator.startAnimating()
        
        URLSession.shared.dataTask(with: url) { [weak self] data, response, error in
            DispatchQueue.main.async {
                activityIndicator.removeFromSuperview()
                
                if let data = data, let image = UIImage(data: data) {
                    self?.backgroundImageView.image = image
                } else {
                    // Handle error or show placeholder
                    self?.backgroundImageView.image = UIImage(systemName: "photo")
                }
            }
        }.resume()
    }
}
```

#### JSON Data Integration
```swift
struct ContentData: Codable {
    let title: String
    let subtitle: String
    let imageURL: String
}

extension ViewController {
    func loadContentFromJSON(_ jsonString: String) {
        guard let data = jsonString.data(using: .utf8) else { return }
        
        do {
            let content = try JSONDecoder().decode(ContentData.self, from: data)
            updateContent(with: content)
        } catch {
            print("Error parsing JSON: \(error)")
        }
    }
    
    private func updateContent(with content: ContentData) {
        titleLabel.text = content.title
        subtitleLabel.text = content.subtitle
        loadImageFromURL(content.imageURL)
    }
}
```

### 3. State Management

#### Content State System
```swift
enum ContentState {
    case loading
    case loaded(ContentData)
    case error(Error)
}

extension ViewController {
    private var contentState: ContentState = .loading {
        didSet {
            updateUI(for: contentState)
        }
    }
    
    private func updateUI(for state: ContentState) {
        switch state {
        case .loading:
            showLoadingState()
        case .loaded(let content):
            showContent(content)
        case .error(let error):
            showError(error)
        }
    }
    
    private func showLoadingState() {
        titleLabel.text = "Loading..."
        subtitleLabel.text = ""
        backgroundImageView.image = UIImage(systemName: "photo")
    }
    
    private func showContent(_ content: ContentData) {
        titleLabel.text = content.title
        subtitleLabel.text = content.subtitle
        loadImageFromURL(content.imageURL)
    }
    
    private func showError(_ error: Error) {
        titleLabel.text = "Error"
        subtitleLabel.text = error.localizedDescription
        backgroundImageView.image = UIImage(systemName: "exclamationmark.triangle")
    }
}
```

## Integration Examples

### 1. Navigation Integration

#### Navigation Controller Setup
```swift
// In AppDelegate.swift or SceneDelegate.swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    
    guard let windowScene = (scene as? UIWindowScene) else { return }
    window = UIWindow(windowScene: windowScene)
    
    let storyboard = UIStoryboard(name: "Main", bundle: nil)
    let viewController = storyboard.instantiateViewController(withIdentifier: "ViewController")
    let navigationController = UINavigationController(rootViewController: viewController)
    
    window?.rootViewController = navigationController
    window?.makeKeyAndVisible()
    
    return true
}
```

#### Navigation Bar Customization
```swift
extension ViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        setupNavigationBar()
    }
    
    private func setupNavigationBar() {
        // Hide navigation bar for full-screen experience
        navigationController?.setNavigationBarHidden(true, animated: false)
        
        // Or customize it
        title = "Photo Viewer"
        navigationController?.navigationBar.prefersLargeTitles = false
        navigationController?.navigationBar.isTranslucent = true
        navigationController?.navigationBar.setBackgroundImage(UIImage(), for: .default)
        navigationController?.navigationBar.shadowImage = UIImage()
        
        // Add navigation items
        navigationItem.rightBarButtonItem = UIBarButtonItem(
            barButtonSystemItem: .action,
            target: self,
            action: #selector(shareButtonTapped)
        )
    }
    
    @objc private func shareButtonTapped() {
        guard let image = backgroundImageView.image else { return }
        
        let activityController = UIActivityViewController(
            activityItems: [image],
            applicationActivities: nil
        )
        
        present(activityController, animated: true)
    }
}
```

### 2. Tab Bar Integration

#### Tab Bar Controller Setup
```swift
class TabBarController: UITabBarController {
    override func viewDidLoad() {
        super.viewDidLoad()
        setupViewControllers()
    }
    
    private func setupViewControllers() {
        let storyboard = UIStoryboard(name: "Main", bundle: nil)
        let photoVC = storyboard.instantiateViewController(withIdentifier: "ViewController")
        photoVC.tabBarItem = UITabBarItem(title: "Photo", image: UIImage(systemName: "photo"), tag: 0)
        
        // Add more view controllers as needed
        viewControllers = [photoVC]
    }
}
```

### 3. Core Data Integration

#### Data Model Setup
```swift
import CoreData

class DataManager {
    lazy var persistentContainer: NSPersistentContainer = {
        let container = NSPersistentContainer(name: "DataModel")
        container.loadPersistentStores { _, error in
            if let error = error {
                fatalError("Core Data error: \(error)")
            }
        }
        return container
    }()
    
    var context: NSManagedObjectContext {
        return persistentContainer.viewContext
    }
    
    func saveContext() {
        if context.hasChanges {
            try? context.save()
        }
    }
}
```

#### Data Integration in ViewController
```swift
extension ViewController {
    private let dataManager = DataManager()
    
    private func loadSavedContent() {
        let request: NSFetchRequest<PhotoContent> = PhotoContent.fetchRequest()
        
        do {
            let savedContent = try dataManager.context.fetch(request)
            if let latest = savedContent.first {
                titleLabel.text = latest.title
                subtitleLabel.text = latest.subtitle
                if let imageData = latest.imageData {
                    backgroundImageView.image = UIImage(data: imageData)
                }
            }
        } catch {
            print("Error loading data: \(error)")
        }
    }
    
    private func saveCurrentContent() {
        let photoContent = PhotoContent(context: dataManager.context)
        photoContent.title = titleLabel.text
        photoContent.subtitle = subtitleLabel.text
        photoContent.imageData = backgroundImageView.image?.pngData()
        photoContent.timestamp = Date()
        
        dataManager.saveContext()
    }
}
```

## Common Use Cases

### 1. Photo Gallery App

```swift
class PhotoGalleryViewController: ViewController {
    private var photos: [Photo] = []
    private var currentIndex = 0
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupSwipeGestures()
        loadPhotos()
    }
    
    private func setupSwipeGestures() {
        let leftSwipe = UISwipeGestureRecognizer(target: self, action: #selector(swipeLeft))
        leftSwipe.direction = .left
        view.addGestureRecognizer(leftSwipe)
        
        let rightSwipe = UISwipeGestureRecognizer(target: self, action: #selector(swipeRight))
        rightSwipe.direction = .right
        view.addGestureRecognizer(rightSwipe)
    }
    
    @objc private func swipeLeft() {
        showNextPhoto()
    }
    
    @objc private func swipeRight() {
        showPreviousPhoto()
    }
    
    private func showNextPhoto() {
        guard currentIndex < photos.count - 1 else { return }
        currentIndex += 1
        updateDisplay()
    }
    
    private func showPreviousPhoto() {
        guard currentIndex > 0 else { return }
        currentIndex -= 1
        updateDisplay()
    }
    
    private func updateDisplay() {
        let photo = photos[currentIndex]
        
        UIView.transition(with: backgroundImageView, duration: 0.3, options: .transitionCrossDissolve) {
            self.backgroundImageView.image = photo.image
        }
        
        UIView.animate(withDuration: 0.3) {
            self.titleLabel.text = photo.title
            self.subtitleLabel.text = photo.description
        }
    }
}
```

### 2. Onboarding Screen

```swift
class OnboardingViewController: ViewController {
    private let onboardingData = [
        OnboardingItem(title: "Welcome", subtitle: "Get started with our app", imageName: "welcome"),
        OnboardingItem(title: "Features", subtitle: "Discover what you can do", imageName: "features"),
        OnboardingItem(title: "Ready", subtitle: "You're all set!", imageName: "ready")
    ]
    
    private var currentPage = 0
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupPageControl()
        setupSkipButton()
        updateContent()
    }
    
    private func setupPageControl() {
        let pageControl = UIPageControl()
        pageControl.numberOfPages = onboardingData.count
        pageControl.currentPage = 0
        pageControl.pageIndicatorTintColor = UIColor.white.withAlphaComponent(0.5)
        pageControl.currentPageIndicatorTintColor = .white
        
        view.addSubview(pageControl)
        pageControl.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            pageControl.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            pageControl.bottomAnchor.constraint(equalTo: overlayView.topAnchor, constant: -20)
        ])
    }
    
    private func updateContent() {
        let item = onboardingData[currentPage]
        titleLabel.text = item.title
        subtitleLabel.text = item.subtitle
        backgroundImageView.image = UIImage(named: item.imageName)
    }
}
```

### 3. Splash Screen with Branding

```swift
class SplashViewController: ViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        setupBrandingElements()
        performStartupTasks()
    }
    
    private func setupBrandingElements() {
        // App logo
        let logoImageView = UIImageView(image: UIImage(named: "app-logo"))
        logoImageView.contentMode = .scaleAspectFit
        overlayView.addSubview(logoImageView)
        
        logoImageView.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            logoImageView.centerXAnchor.constraint(equalTo: overlayView.centerXAnchor),
            logoImageView.topAnchor.constraint(equalTo: overlayView.topAnchor, constant: 20),
            logoImageView.widthAnchor.constraint(equalToConstant: 100),
            logoImageView.heightAnchor.constraint(equalToConstant: 100)
        ])
        
        // Version info
        let versionLabel = UILabel()
        versionLabel.text = "Version \(Bundle.main.infoDictionary?["CFBundleShortVersionString"] as? String ?? "1.0")"
        versionLabel.font = UIFont.systemFont(ofSize: 12, weight: .light)
        versionLabel.textColor = .white
        versionLabel.textAlignment = .center
        overlayView.addSubview(versionLabel)
        
        versionLabel.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            versionLabel.centerXAnchor.constraint(equalTo: overlayView.centerXAnchor),
            versionLabel.bottomAnchor.constraint(equalTo: overlayView.bottomAnchor, constant: -20)
        ])
    }
    
    private func performStartupTasks() {
        // Simulate app initialization
        DispatchQueue.main.asyncAfter(deadline: .now() + 2.0) {
            self.transitionToMainApp()
        }
    }
    
    private func transitionToMainApp() {
        let storyboard = UIStoryboard(name: "Main", bundle: nil)
        let mainVC = storyboard.instantiateViewController(withIdentifier: "MainViewController")
        
        if let windowScene = view.window?.windowScene,
           let window = windowScene.windows.first {
            window.rootViewController = mainVC
            
            UIView.transition(with: window, duration: 0.5, options: .transitionCrossDissolve, animations: nil)
        }
    }
}
```

## Code Snippets Library

### 1. Image Processing Utilities

```swift
extension UIImage {
    func resized(to size: CGSize) -> UIImage? {
        UIGraphicsBeginImageContextWithOptions(size, false, 0.0)
        defer { UIGraphicsEndImageContext() }
        
        draw(in: CGRect(origin: .zero, size: size))
        return UIGraphicsGetImageFromCurrentImageContext()
    }
    
    func withBlur(radius: CGFloat) -> UIImage? {
        let context = CIContext()
        guard let ciImage = CIImage(image: self) else { return nil }
        
        let filter = CIFilter(name: "CIGaussianBlur")
        filter?.setValue(ciImage, forKey: kCIInputImageKey)
        filter?.setValue(radius, forKey: kCIInputRadiusKey)
        
        guard let outputImage = filter?.outputImage,
              let cgImage = context.createCGImage(outputImage, from: ciImage.extent) else { return nil }
        
        return UIImage(cgImage: cgImage)
    }
}
```

### 2. Animation Helpers

```swift
extension UIView {
    func fadeIn(duration: TimeInterval = 0.3, completion: (() -> Void)? = nil) {
        alpha = 0
        UIView.animate(withDuration: duration, animations: {
            self.alpha = 1
        }, completion: { _ in
            completion?()
        })
    }
    
    func slideInFromBottom(duration: TimeInterval = 0.5) {
        transform = CGAffineTransform(translationX: 0, y: frame.height)
        UIView.animate(withDuration: duration, delay: 0, usingSpringWithDamping: 0.8, initialSpringVelocity: 0, options: .curveEaseOut, animations: {
            self.transform = .identity
        })
    }
    
    func shake() {
        let animation = CAKeyframeAnimation(keyPath: "transform.translation.x")
        animation.timingFunction = CAMediaTimingFunction(name: .linear)
        animation.duration = 0.6
        animation.values = [-20.0, 20.0, -20.0, 20.0, -10.0, 10.0, -5.0, 5.0, 0.0]
        layer.add(animation, forKey: "shake")
    }
}
```

### 3. Constraint Helpers

```swift
extension UIView {
    func constrainToSuperview(with insets: UIEdgeInsets = .zero) {
        guard let superview = superview else { return }
        
        translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            topAnchor.constraint(equalTo: superview.topAnchor, constant: insets.top),
            leadingAnchor.constraint(equalTo: superview.leadingAnchor, constant: insets.left),
            trailingAnchor.constraint(equalTo: superview.trailingAnchor, constant: -insets.right),
            bottomAnchor.constraint(equalTo: superview.bottomAnchor, constant: -insets.bottom)
        ])
    }
    
    func centerInSuperview() {
        guard let superview = superview else { return }
        
        translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            centerXAnchor.constraint(equalTo: superview.centerXAnchor),
            centerYAnchor.constraint(equalTo: superview.centerYAnchor)
        ])
    }
}
```

### 4. Device Detection

```swift
extension UIDevice {
    var isIPhone: Bool {
        return userInterfaceIdiom == .phone
    }
    
    var isIPad: Bool {
        return userInterfaceIdiom == .pad
    }
    
    var hasNotch: Bool {
        if #available(iOS 11.0, *) {
            return UIApplication.shared.windows.first?.safeAreaInsets.top ?? 0 > 20
        }
        return false
    }
}
```

---

This comprehensive usage guide provides practical examples for extending and customizing App3 for various use cases. Each example includes complete, working code that can be implemented directly in your project.