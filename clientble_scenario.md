# [ClientBleIOSDoc](https://sdkpay.github.io/ClientBleIOSDoc/)

#### [Сценарии работы с SDK](https://sdkpay.github.io/ClientBleIOSDoc/clientble_scenario) | [Настройки проекта](https://sdkpay.github.io/ClientBleIOSDoc/clientble_classes) | [Актуальная версия SDK](https://sdkpay.github.io/ClientBleIOSDoc/clientble_version)

<br>

# Сценарии работы SDK

#### [Базовый сценарий](https://sdkpay.github.io/ClientBleIOSDoc/clientble_scenario#базовый-сценарий-для-sdk)
#### [Фоновый сценарий](https://sdkpay.github.io/ClientBleIOSDoc/clientble_scenario#фоновый-сценарий-для-sdk)
#### [Разлогирование_пользователя](https://sdkpay.github.io/ClientBleIOSDoc/clientble_scenario#разлогирование-пользователя)

<br>

## Базовый сценарий для SDK

Параметры запроса оплаты:
* apiKey: String - Уникальный ключ для авторизации в sdk
* redirectUri: String - deeplink, зарегистрированный в Сбер

```swift
import BluePaySDK

let request = SPaymentRequest(apiKey: apiKey, redirectUri: redirectUri)
           
BleSDK.pay(request: request, completion: { state, localid, info in
    DispatchQueue.main.async {
        switch state {
            case .success:
                print("Payment successful")
                // Обновите UI, покажите успех
                
            case .error:
                print("Payment error")
                // Покажите ошибку пользователю
                
            case .cancel:
                print("Payment cancelled by user")
                // Обработайте отмену
            }
    }
}
```

## Фоновый сценарий для SDK

Режим оплаты, после старта которого, sdk "ищет" терминалы для оплаты и автоматически поднимает окно sdk
```swift
BleSDK.startBackgroundPayment(
    request: SPaymentRequest(apiKey: apiKey, redirectUri: redirectUri),
                completion: { state, localId, info in
    DispatchQueue.main.async {
        switch state {
            case .success:
                print("Payment successful")
                // Обновите UI, покажите успех
                
            case .error:
                print("Payment error")
                // Покажите ошибку пользователю
                
            case .cancel:
                print("Payment cancelled by user")
                // Обработайте отмену
            }
    }
}
```

Остановка поиска терминалов

```swift
   BleSDK.stopBackgroundPayment()
```

## Разлогирование пользователя

Чтобы разлогировать пользователя нужно выполнить код:

```kotlin
  BleSDK.logout(
```

## Рекомендации по использованию

Безопасность API ключей:
* Никогда не хардкодьте API ключи в приложении
* Получайте ключи с сервера при необходимости

Обработка результатов:
* Всегда выполняйте UI обновления в главном потоке
* Предусмотрите все возможные сценарии (успех/ошибка/отмена)