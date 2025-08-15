# App3 - Usage Guide

## Table of Contents

1. [Getting Started](#getting-started)
2. [Basic Usage Examples](#basic-usage-examples)
3. [Common Use Cases](#common-use-cases)
4. [Advanced Patterns](#advanced-patterns)
5. [Troubleshooting](#troubleshooting)
6. [Best Practices](#best-practices)

---

## Getting Started

### Prerequisites

- Xcode 10.0 or later
- iOS 8.0+ deployment target
- Basic knowledge of Swift and iOS development

### Project Setup

1. **Clone or Download the Project**
   ```bash
   git clone <repository-url>
   cd App3
   ```

2. **Open in Xcode**
   ```bash
   open App3.xcodeproj
   ```

3. **Configure Your Team**
   - Select the project in the navigator
   - Choose your target
   - In "Signing & Capabilities", select your development team

4. **Build and Run**
   - Select your target device or simulator
   - Press `Cmd+R` to build and run

---

## Basic Usage Examples

### 1. Customizing AppDelegate

#### Basic Configuration
```swift
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    var window: UIWindow?
    
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Configure the main window
        window = UIWindow(frame: UIScreen.main.bounds)
        
        // Create and configure the root view controller
        let viewController = ViewController()
        let navigationController = UINavigationController(rootViewController: viewController)
        
        // Set the root view controller
        window?.rootViewController = navigationController
        
        // Make the window visible
        window?.makeKeyAndVisible()
        
        return true
    }
}
```

#### Adding Third-Party Services
```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // Configure Firebase
    FirebaseApp.configure()
    
    // Configure Crashlytics
    Fabric.with([Crashlytics.self])
    
    // Configure analytics
    Analytics.setAnalyticsCollectionEnabled(true)
    
    // Set up the main window
    setupMainWindow()
    
    return true
}

private func setupMainWindow() {
    window = UIWindow(frame: UIScreen.main.bounds)
    let viewController = ViewController()
    window?.rootViewController = viewController
    window?.makeKeyAndVisible()
}
```

### 2. Customizing ViewController

#### Basic UI Setup
```swift
import UIKit

class ViewController: UIViewController {
    
    // MARK: - UI Elements
    private let titleLabel: UILabel = {
        let label = UILabel()
        label.text = "Welcome to App3"
        label.font = UIFont.systemFont(ofSize: 24, weight: .bold)
        label.textAlignment = .center
        label.translatesAutoresizingMaskIntoConstraints = false
        return label
    }()
    
    private let actionButton: UIButton = {
        let button = UIButton(type: .system)
        button.setTitle("Tap Me", for: .normal)
        button.titleLabel?.font = UIFont.systemFont(ofSize: 18)
        button.backgroundColor = .systemBlue
        button.setTitleColor(.white, for: .normal)
        button.layer.cornerRadius = 8
        button.translatesAutoresizingMaskIntoConstraints = false
        return button
    }()
    
    // MARK: - Lifecycle
    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
        setupConstraints()
        setupActions()
    }
    
    // MARK: - Setup
    private func setupUI() {
        view.backgroundColor = .systemBackground
        title = "App3"
        
        view.addSubview(titleLabel)
        view.addSubview(actionButton)
    }
    
    private func setupConstraints() {
        NSLayoutConstraint.activate([
            titleLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            titleLabel.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor, constant: 50),
            
            actionButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            actionButton.topAnchor.constraint(equalTo: titleLabel.bottomAnchor, constant: 30),
            actionButton.widthAnchor.constraint(equalToConstant: 200),
            actionButton.heightAnchor.constraint(equalToConstant: 50)
        ])
    }
    
    private func setupActions() {
        actionButton.addTarget(self, action: #selector(actionButtonTapped), for: .touchUpInside)
    }
    
    // MARK: - Actions
    @objc private func actionButtonTapped() {
        let alert = UIAlertController(title: "Button Tapped", message: "You tapped the button!", preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
}
```

#### Table View Implementation
```swift
import UIKit

class ViewController: UIViewController {
    
    // MARK: - Properties
    private let tableView = UITableView()
    private let dataSource = ["Item 1", "Item 2", "Item 3", "Item 4", "Item 5"]
    
    // MARK: - Lifecycle
    override func viewDidLoad() {
        super.viewDidLoad()
        setupTableView()
    }
    
    // MARK: - Setup
    private func setupTableView() {
        view.addSubview(tableView)
        tableView.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            tableView.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor),
            tableView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            tableView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            tableView.bottomAnchor.constraint(equalTo: view.bottomAnchor)
        ])
        
        tableView.delegate = self
        tableView.dataSource = self
        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "Cell")
    }
}

// MARK: - UITableViewDataSource
extension ViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return dataSource.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
        cell.textLabel?.text = dataSource[indexPath.row]
        return cell
    }
}

// MARK: - UITableViewDelegate
extension ViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        tableView.deselectRow(at: indexPath, animated: true)
        
        let alert = UIAlertController(title: "Selected", message: "You selected: \(dataSource[indexPath.row])", preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
}
```

---

## Common Use Cases

### 1. Navigation and Multiple Screens

#### Creating a Navigation Flow
```swift
import UIKit

class ViewController: UIViewController {
    
    private let pushButton: UIButton = {
        let button = UIButton(type: .system)
        button.setTitle("Push to Detail", for: .normal)
        button.translatesAutoresizingMaskIntoConstraints = false
        return button
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
        setupActions()
    }
    
    private func setupUI() {
        view.backgroundColor = .systemBackground
        title = "Main Screen"
        
        view.addSubview(pushButton)
        NSLayoutConstraint.activate([
            pushButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            pushButton.centerYAnchor.constraint(equalTo: view.centerYAnchor)
        ])
    }
    
    private func setupActions() {
        pushButton.addTarget(self, action: #selector(pushButtonTapped), for: .touchUpInside)
    }
    
    @objc private func pushButtonTapped() {
        let detailVC = DetailViewController()
        navigationController?.pushViewController(detailVC, animated: true)
    }
}

class DetailViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        view.backgroundColor = .systemBackground
        title = "Detail Screen"
        
        // Add a back button
        navigationItem.leftBarButtonItem = UIBarButtonItem(
            title: "Back",
            style: .plain,
            target: self,
            action: #selector(backButtonTapped)
        )
    }
    
    @objc private func backButtonTapped() {
        navigationController?.popViewController(animated: true)
    }
}
```

### 2. Data Persistence

#### UserDefaults Implementation
```swift
import UIKit

class ViewController: UIViewController {
    
    private let saveButton: UIButton = {
        let button = UIButton(type: .system)
        button.setTitle("Save Data", for: .normal)
        button.translatesAutoresizingMaskIntoConstraints = false
        return button
    }()
    
    private let loadButton: UIButton = {
        let button = UIButton(type: .system)
        button.setTitle("Load Data", for: .normal)
        button.translatesAutoresizingMaskIntoConstraints = false
        return button
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
        setupActions()
    }
    
    private func setupUI() {
        view.backgroundColor = .systemBackground
        title = "Data Persistence"
        
        let stackView = UIStackView(arrangedSubviews: [saveButton, loadButton])
        stackView.axis = .vertical
        stackView.spacing = 20
        stackView.translatesAutoresizingMaskIntoConstraints = false
        
        view.addSubview(stackView)
        NSLayoutConstraint.activate([
            stackView.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            stackView.centerYAnchor.constraint(equalTo: view.centerYAnchor)
        ])
    }
    
    private func setupActions() {
        saveButton.addTarget(self, action: #selector(saveData), for: .touchUpInside)
        loadButton.addTarget(self, action: #selector(loadData), for: .touchUpInside)
    }
    
    @objc private func saveData() {
        let userDefaults = UserDefaults.standard
        userDefaults.set("Hello World", forKey: "savedText")
        userDefaults.set(Date(), forKey: "savedDate")
        userDefaults.synchronize()
        
        let alert = UIAlertController(title: "Success", message: "Data saved successfully!", preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
    
    @objc private func loadData() {
        let userDefaults = UserDefaults.standard
        let savedText = userDefaults.string(forKey: "savedText") ?? "No data"
        let savedDate = userDefaults.object(forKey: "savedDate") as? Date ?? Date()
        
        let dateFormatter = DateFormatter()
        dateFormatter.dateStyle = .medium
        dateFormatter.timeStyle = .medium
        
        let message = "Text: \(savedText)\nDate: \(dateFormatter.string(from: savedDate))"
        
        let alert = UIAlertController(title: "Loaded Data", message: message, preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
}
```

### 3. Network Requests

#### Basic URLSession Implementation
```swift
import UIKit

class ViewController: UIViewController {
    
    private let fetchButton: UIButton = {
        let button = UIButton(type: .system)
        button.setTitle("Fetch Data", for: .normal)
        button.translatesAutoresizingMaskIntoConstraints = false
        return button
    }()
    
    private let resultLabel: UILabel = {
        let label = UILabel()
        label.text = "No data fetched yet"
        label.numberOfLines = 0
        label.textAlignment = .center
        label.translatesAutoresizingMaskIntoConstraints = false
        return label
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
        setupActions()
    }
    
    private func setupUI() {
        view.backgroundColor = .systemBackground
        title = "Network Example"
        
        view.addSubview(fetchButton)
        view.addSubview(resultLabel)
        
        NSLayoutConstraint.activate([
            fetchButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            fetchButton.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor, constant: 50),
            
            resultLabel.topAnchor.constraint(equalTo: fetchButton.bottomAnchor, constant: 30),
            resultLabel.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 20),
            resultLabel.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -20)
        ])
    }
    
    private func setupActions() {
        fetchButton.addTarget(self, action: #selector(fetchData), for: .touchUpInside)
    }
    
    @objc private func fetchData() {
        fetchButton.isEnabled = false
        fetchButton.setTitle("Fetching...", for: .normal)
        
        guard let url = URL(string: "https://jsonplaceholder.typicode.com/posts/1") else {
            showError("Invalid URL")
            return
        }
        
        let task = URLSession.shared.dataTask(with: url) { [weak self] data, response, error in
            DispatchQueue.main.async {
                self?.fetchButton.isEnabled = true
                self?.fetchButton.setTitle("Fetch Data", for: .normal)
                
                if let error = error {
                    self?.showError("Network error: \(error.localizedDescription)")
                    return
                }
                
                guard let data = data else {
                    self?.showError("No data received")
                    return
                }
                
                do {
                    let json = try JSONSerialization.jsonObject(with: data, options: [])
                    if let dict = json as? [String: Any], let title = dict["title"] as? String {
                        self?.resultLabel.text = "Fetched: \(title)"
                    } else {
                        self?.resultLabel.text = "Data received: \(String(data: data, encoding: .utf8) ?? "Unknown")"
                    }
                } catch {
                    self?.showError("JSON parsing error: \(error.localizedDescription)")
                }
            }
        }
        
        task.resume()
    }
    
    private func showError(_ message: String) {
        let alert = UIAlertController(title: "Error", message: message, preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
}
```

---

## Advanced Patterns

### 1. MVVM Architecture

#### Model
```swift
struct User {
    let id: Int
    let name: String
    let email: String
}

struct UserViewModel {
    let user: User
    
    var displayName: String {
        return user.name
    }
    
    var displayEmail: String {
        return user.email
    }
    
    var isEmailValid: Bool {
        return user.email.contains("@")
    }
}
```

#### ViewModel
```swift
class UserListViewModel {
    private var users: [User] = []
    
    var numberOfUsers: Int {
        return users.count
    }
    
    func user(at index: Int) -> UserViewModel? {
        guard index < users.count else { return nil }
        return UserViewModel(user: users[index])
    }
    
    func fetchUsers(completion: @escaping (Result<Void, Error>) -> Void) {
        // Simulate network request
        DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
            self.users = [
                User(id: 1, name: "John Doe", email: "john@example.com"),
                User(id: 2, name: "Jane Smith", email: "jane@example.com"),
                User(id: 3, name: "Bob Johnson", email: "bob@example.com")
            ]
            completion(.success(()))
        }
    }
}
```

#### View
```swift
class UserListViewController: UIViewController {
    
    private let tableView = UITableView()
    private let viewModel = UserListViewModel()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupTableView()
        fetchUsers()
    }
    
    private func setupTableView() {
        view.addSubview(tableView)
        tableView.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            tableView.topAnchor.constraint(equalTo: view.topAnchor),
            tableView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            tableView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            tableView.bottomAnchor.constraint(equalTo: view.bottomAnchor)
        ])
        
        tableView.delegate = self
        tableView.dataSource = self
        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "UserCell")
    }
    
    private func fetchUsers() {
        viewModel.fetchUsers { [weak self] result in
            switch result {
            case .success:
                self?.tableView.reloadData()
            case .failure(let error):
                self?.showError("Failed to fetch users: \(error.localizedDescription)")
            }
        }
    }
    
    private func showError(_ message: String) {
        let alert = UIAlertController(title: "Error", message: message, preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
}

extension UserListViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return viewModel.numberOfUsers
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "UserCell", for: indexPath)
        
        if let userViewModel = viewModel.user(at: indexPath.row) {
            cell.textLabel?.text = userViewModel.displayName
            cell.detailTextLabel?.text = userViewModel.displayEmail
            cell.accessoryType = userViewModel.isEmailValid ? .checkmark : .none
        }
        
        return cell
    }
}

extension UserListViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        tableView.deselectRow(at: indexPath, animated: true)
        
        if let userViewModel = viewModel.user(at: indexPath.row) {
            let alert = UIAlertController(title: "User Selected", message: "Name: \(userViewModel.displayName)\nEmail: \(userViewModel.displayEmail)", preferredStyle: .alert)
            alert.addAction(UIAlertAction(title: "OK", style: .default))
            present(alert, animated: true)
        }
    }
}
```

### 2. Protocol-Oriented Programming

#### Defining Protocols
```swift
protocol DataFetchable {
    func fetchData(completion: @escaping (Result<[String], Error>) -> Void)
}

protocol DataDisplayable {
    func displayData(_ data: [String])
}

protocol DataStorable {
    func saveData(_ data: [String])
    func loadData() -> [String]
}
```

#### Implementing Protocols
```swift
class NetworkDataFetcher: DataFetchable {
    func fetchData(completion: @escaping (Result<[String], Error>) -> Void) {
        // Simulate network request
        DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
            completion(.success(["Item 1", "Item 2", "Item 3"]))
        }
    }
}

class LocalDataStorage: DataStorable {
    private let userDefaults = UserDefaults.standard
    private let key = "savedData"
    
    func saveData(_ data: [String]) {
        userDefaults.set(data, forKey: key)
    }
    
    func loadData() -> [String] {
        return userDefaults.stringArray(forKey: key) ?? []
    }
}

class DataViewController: UIViewController, DataDisplayable {
    
    private let dataFetcher: DataFetchable
    private let dataStorage: DataStorable
    
    init(dataFetcher: DataFetchable, dataStorage: DataStorable) {
        self.dataFetcher = dataFetcher
        self.dataStorage = dataStorage
        super.init(nibName: nil, bundle: nil)
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
        loadSavedData()
    }
    
    private func setupUI() {
        view.backgroundColor = .systemBackground
        title = "Protocol Example"
        
        let fetchButton = UIButton(type: .system)
        fetchButton.setTitle("Fetch New Data", for: .normal)
        fetchButton.addTarget(self, action: #selector(fetchNewData), for: .touchUpInside)
        fetchButton.translatesAutoresizingMaskIntoConstraints = false
        
        view.addSubview(fetchButton)
        NSLayoutConstraint.activate([
            fetchButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            fetchButton.centerYAnchor.constraint(equalTo: view.centerYAnchor)
        ])
    }
    
    private func loadSavedData() {
        let savedData = dataStorage.loadData()
        if !savedData.isEmpty {
            displayData(savedData)
        }
    }
    
    @objc private func fetchNewData() {
        dataFetcher.fetchData { [weak self] result in
            switch result {
            case .success(let data):
                self?.dataStorage.saveData(data)
                self?.displayData(data)
            case .failure(let error):
                self?.showError("Failed to fetch data: \(error.localizedDescription)")
            }
        }
    }
    
    func displayData(_ data: [String]) {
        let message = data.joined(separator: "\n")
        let alert = UIAlertController(title: "Data", message: message, preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
    
    private func showError(_ message: String) {
        let alert = UIAlertController(title: "Error", message: message, preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
}
```

---

## Troubleshooting

### Common Issues and Solutions

#### 1. Build Errors

**Issue:** "No such module 'UIKit'"
**Solution:** Ensure you're building for iOS target, not macOS

**Issue:** "Use of unresolved identifier"
**Solution:** Check import statements and ensure proper inheritance

#### 2. Runtime Crashes

**Issue:** App crashes on launch
**Solution:** Check AppDelegate implementation and ensure proper window setup

**Issue:** View not displaying
**Solution:** Verify Auto Layout constraints and view hierarchy

#### 3. Memory Issues

**Issue:** Memory warnings
**Solution:** Implement proper cleanup in lifecycle methods

**Issue:** Retain cycles
**Solution:** Use weak references in closures and delegates

### Debug Tips

1. **Use Breakpoints**
   - Set breakpoints in key methods
   - Use conditional breakpoints for specific scenarios

2. **Console Logging**
   ```swift
   print("Debug: \(variable)")
   NSLog("Debug: %@", variable)
   ```

3. **Instruments**
   - Use Instruments for performance analysis
   - Monitor memory usage and leaks

---

## Best Practices

### 1. Code Organization

- Use MARK comments to organize code sections
- Group related properties and methods together
- Follow consistent naming conventions

### 2. Memory Management

- Use weak references in closures
- Implement proper cleanup in deinit
- Avoid retain cycles

### 3. Error Handling

- Always handle errors gracefully
- Provide meaningful error messages
- Log errors for debugging

### 4. Performance

- Keep viewDidLoad lightweight
- Use background queues for heavy operations
- Implement proper caching strategies

### 5. User Experience

- Provide loading indicators
- Handle edge cases gracefully
- Follow iOS design guidelines

---

*This usage guide provides comprehensive examples and patterns for using the App3 iOS application. For additional information, refer to the API Reference and README documentation.*