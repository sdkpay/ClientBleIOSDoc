# [ClientBleIOSDoc](https://sdkpay.github.io/ClientBleIOSDoc/)

#### [Настройки проекта](https://sdkpay.github.io/ClientBleIOSDoc/clientble_scenario) | [Сущности и классы](https://sdkpay.github.io/ClientBleIOSDoc/clientble_classes) | [Актуальная версия SDK](https://sdkpay.github.io/ClientBleIOSDoc/clientble_version)

<br>

# Настройки проекта

<br>

В info.plist необходимо добавить:
* deeplink, зарегистрированный в Сбер, для успешной авторизации
* Пермиссия на использование BLE
* Пермиссия на использование локальной биометрии
* Пермиссия на доступ к геопозиции
* Доверенные домены для работы sdk

```swift

<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>yourapp</string> <!-- Схема, зарегистрированная в системе SberID -->
        </array>
    </dict>
</array>
<key>NSBluetoothAlwaysUsageDescription</key>
<string>Bluetooth usage description</string>
<key>NSFaceIDUsageDescription</key>
<string>FaceID usage description</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Need location</string>
<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSExceptionDomains</key>
		<dict>
			<key>gate1.spaymentsplus.ru</key>
			<dict>
				<key>NSExceptionAllowsInsecureHTTPLoads</key>
				<true/>
			</dict>
			<key>safepayonline.ru</key>
			<dict>
				<key>NSExceptionAllowsInsecureHTTPLoads</key>
				<true/>
			</dict>
		</dict>
	</dict>
```

##  Настройка AppDelegate

Реализуйте обработку deep links в вашем AppDelegate:

```swift
import UIKit
import BluePaySDK

@main
class AppDelegate: UIResponder, UIApplicationDelegate {
    // ... другие методы
    
    func application(
        _ app: UIApplication,
        open url: URL,
        options: [UIApplication.OpenURLOptionsKey: Any] = [:]
    ) -> Bool {
        // Передаем URL в SDK для обработки
        BleSDK.getAuthURL(url)
        return true
    }
}
```
Для iOS 13+ с SceneDelegate:

```swift
import BluePaySDK

func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
    guard let url = URLContexts.first?.url else { return }
    BleSDK.getAuthURL(url)
}
```