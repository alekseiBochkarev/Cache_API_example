# Cache_API_example

Пример кода:

Service Worker (service-worker.js):
```javascript
const CACHE_NAME = 'example-cache-v1';
const URLS_TO_CACHE = [
    '/',
    '/styles.css',
    '/script.js',
    '/image.png'
];

// Установка Service Worker и кэширование ресурсов
self.addEventListener('install', event => {
    event.waitUntil(
        caches.open(CACHE_NAME)
            .then(cache => {
                return cache.addAll(URLS_TO_CACHE);
            })
    );
});

// Обслуживание запросов из кэша
self.addEventListener('fetch', event => {
    event.respondWith(
        caches.match(event.request)
            .then(response => {
                if (response) {
                    return response; // Возвращаем ресурс из кэша
                }
                return fetch(event.request); // Запрашиваем ресурс из сети
            })
    );
});
```

Пример HTML
HTML (index.html):
```html
<!DOCTYPE html>
<html>
<head>
    <title>Кэш API Пример</title>
    <link rel="stylesheet" type="text/css" href="styles.css">
</head>
<body>
    <h1>Привет, мир!</h1>
    <p>Это пример использования Cache API.</p>
    <img src="image.png" alt="Example Image">
    <script src="script.js"></script>
    <script>
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('/service-worker.js')
                .then(registration => {
                    console.log('Service Worker зарегистрирован с областью:', registration.scope);
                })
                .catch(error => {
                    console.log('Ошибка регистрации Service Worker:', error);
                });
        }
    </script>
</body>
</html>
```
