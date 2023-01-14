# UserDefaultsPropertyWrapperCodable

```swift 
@propertyWrapper
struct UserDefaultsStore<T: Codable> {
    
    private let key: String
    private let defaultValue: T
    
    init(key: String, defaultValue: T) {
        self.key = key
        self.defaultValue = defaultValue
    }
    
    var wrappedValue: T {
        get {
            guard let data = UserDefaults.standard.object(forKey: self.key) as? Data else {
                return self.defaultValue
            }
            let value = try? JSONDecoder().decode(T.self, from: data)
            return value ?? self.defaultValue
        }
        set{
            let data = try? JSONEncoder().encode(newValue)
            UserDefaults.standard.set(data, forKey: self.key)
        }
    }
    
}

extension UserDefaults {
    
    @UserDefaultsStore(key: "value", defaultValue: "") static var value: String
//    @UbiquitousStore(key: "setting", defaultValue: "") static var settings: Settings
    
}
```
