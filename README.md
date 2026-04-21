# Px6Api
.NET клиент для работы с сервисом px6.me.
Библиотека предоставляет удобный интерфейс для управления прокси: покупки, продления, проверки статуса и получения информации о балансе.

# 🚀 Возможности
- 🌐 Покупка IPv4 и IPv6 прокси
- 💰 Получение информации о ценах и доступности прокси по странам
- 📋 Управление списком прокси (фильтрация по статусу, пагинация)
- 📝 Редактирование комментариев к заказам
- 🔄 Продление и удаление прокси
- ✅ Проверка валидности (чекер)
- 🔒 Управление авторизацией по IP
- ⚡ Полностью асинхронный API (async/await)

# 📦 Установка
На данный момент библиотека доступна в виде исходного кода. Скомпилируйте проект и добавьте ссылку на DLL в ваш основной проект.

# 🔑 Аутентификация
Для работы с API вам понадобится API Key.
Его можно найти в личном кабинете на сайте px6.me: Аккаунт -> Разработчикам (API).

# ⚡ Быстрый старт
``` C#
using Px6Api;
using Px6Api.DTOModels;

// Инициализация клиента
using var client = new Px6Client("ВАШ_API_КЛЮЧ");

// 1. Проверка баланса
var (user, _) = await client.GetCountriesAsync();
Console.WriteLine($"Ваш баланс: {user.Balance} {user.Currency}");

// 2. Получение списка активных прокси
var (_, proxies) = await client.GetProxiesAsync(ProxyState.Active);
foreach (var proxy in proxies.Values)
{
    Console.WriteLine($"{proxy.Ip}:{proxy.Port} - до {proxy.DateEnd}");
}

// 3. Покупка прокси
var (u, buyInfo, newList) = await client.BuyProxy(
    count: 1, 
    period: 3, 
    country: "ru", 
    description: "Тестовая покупка", 
    autoProlong: false, 
    nokey: false
);
```

# 📚 API
Информация и цены
- GetPriceAsync(count, period, version) — расчет стоимости заказа.
- GetProxyCountAsync(country, version) — доступное кол-во прокси для конкретной страны.
- GetCountriesAsync(version) — список стран, доступных для покупки.

Управление прокси
- GetProxiesAsync(...) — получение списка прокси с фильтрацией по состоянию (active, expired, expiring, all).
- SetProxyDescriptionAsync(...) — обновление технического комментария (descr) для группы прокси.
- BuyProxy(...) — покупка новых прокси (поддерживает HTTP/SOCKS5).
- ProlongProxyAsync(...) — продление срока действия прокси.
- DeleteProxyAsync(...) — удаление прокси из личного кабинета.

Инструменты и безопасность
- CheckProxyAsync(proxyId) — проверка работоспособности прокси по ID или строке.
- IpAuthorizationAsync(ips) — привязка авторизации к списку IP-адресов.
- DeleteIpAuthorizationAsync() — удаление привязки по IP.

# 🛠 Модели данных
Библиотека использует строго типизированные DTO:
- ProxyInfo: Полная информация о прокси (IP, Port, User, Pass, Срок действия).
- User: Информация о пользователе (ID, Баланс, Валюта).
- PriceInfo: Детализация стоимости.
