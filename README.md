# UserDefaultsPropertyWrapperCodable

```swift 
@propertyWrapper
struct UserDefaultsStore<T: Codable> {
    
    private let key: String
    private let defaultValue: T
    private let container: UserDefaults
    
    init(key: String, defaultValue: T) {
        self.key = key
        self.defaultValue = defaultValue
        self.container = UserDefaults.standard
    }
    
    init(key: String, defaultValue: T, suiteName: String) {
        self.key = key
        self.defaultValue = defaultValue
        guard let container = UserDefaults(suiteName: suiteName) else {
            fatalError("No suite")
        }
        self.container = container
    }
    
    var wrappedValue: T {
        get {
            guard let data = self.container.object(forKey: self.key) as? Data else {
                return self.defaultValue
            }
            let value = try? JSONDecoder().decode(T.self, from: data)
            return value ?? self.defaultValue
        }
        set{
            let data = try? JSONEncoder().encode(newValue)
            self.container.set(data, forKey: self.key)
        }
    }
    
}

extension UserDefaults {
    
    @UserDefaultsStore(key: "value", defaultValue: "") static var value: String
//    @UbiquitousStore(key: "setting", defaultValue: "") static var settings: Settings
    
}

```
