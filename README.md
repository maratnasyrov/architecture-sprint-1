# Task 1. Архитектура микрофронтендов для приложения MESTO

В этом проекте фронтенд-часть приложения была разделена на четыре ключевых микрофронтенда: **host**, **photo**, **profile** и **auth**. Такой подход позволяет нам создать модульную, масштабируемую и легко поддерживаемую архитектуру.

## Технологический стек

Для реализации микрофронтендов выбрал **vite-module-federation** - современную интерпретацию webpack-module-federation, адаптированную для использования с Vite. Этот выбор обусловлен несколькими ключевыми преимуществами Vite:

1. **Cкорость разработки**: Vite использует нативные ES-модули, что значительно ускоряет процесс запуска и перезагрузки проекта.

2. **Минимальная конфигурация**: Vite предлагает интуитивно понятную настройку и работает эффективно "из коробки" для большинства проектов.

3. **Поддержка современных технологий**: Встроенная поддержка TypeScript, JSX, CSS-модулей и других технологий без необходимости установки дополнительных плагинов.

Для обеспечения согласованности и эффективности разработки, во всех микрофронтендах будет использоваться **React** в качестве основной библиотеки для построения пользовательских интерфейсов.

## Структура микрофронтендов

### Auth

Этот микрофронтенд отвечает за аутентификацию и авторизацию пользователей. Он включает в себя:

- Интерфейс регистрации новых пользователей
- Форму входа в систему
- Логику управления сессиями пользователей

### Photo

Данный микрофронтенд фокусируется на работе с фотографиями и включает:

- Отображение ленты фотографий (мест)
- Интерфейс для загрузки новых фотографий
- Функционал лайков и комментариев
- Систему фильтрации и поиска фотографий

### Profile

Микрофронтенд профиля пользователя предоставляет:

- Отображение информации о текущем пользователе
- Формы для редактирования данных профиля
- Интерфейс для обновления аватара пользователя
- Возможность просмотра статистики активности пользователя

### Host

Host выступает в роли основного приложения, которое объединяет все остальные микрофронтенды. Особенности:

- Использование Server-Side Rendering (SSR) для улучшения SEO. Предлагается использовать Vike (аналог Next.js для Vite) для обеспечения совместимости с выбранным сборщиком.
- Реализация общей навигации и футера непосредственно в Host-приложении, с возможностью их выделения в отдельные микрофронтенды в будущем при усложнении функционала.
- Управление маршрутизацией между различными микрофронтендами

## Взаимодействие между микрофронтендами

Ключевым механизмом для обеспечения эффективного взаимодействия между микрофронтендами в нашей архитектуре является событийная система, реализованная по паттерну publish/subscribe (pub/sub). Этот подход позволяет микрофронтендам обмениваться данными и уведомлениями, сохраняя при этом слабую связанность.

### Преимущества использования событийной системы:

1. **Слабая связанность**: Микрофронтенды могут взаимодействовать друг с другом, не имея прямых зависимостей.
2. **Гибкость**: Легко добавлять новые обработчики событий или удалять существующие без изменения кода отправителя.
3. **Масштабируемость**: Система легко расширяется для поддержки новых типов событий при росте приложения.
4. **Асинхронность**: События могут обрабатываться асинхронно, что улучшает производительность.
5. **Централизованное управление**: Единая точка для управления межкомпонентными коммуникациями.

# Task 2. Декомпозиция веб приложения на Django

Ссылка
https://disk.yandex.ru/i/iIVQiwqn8xD9ow

Не совсем понятно как работают уведомления в исходной системе, поэтому пусть это будет способ уведомлений через почту.

- Непрерывной линией указаны синхронные запросы.
- Пунктирной асинхронные.

## Описание

### Основные сервисы

- Сервис авторизации и регистрации: Управляет регистрацией пользователей и их аутентификацией.
- Сервис товаров: Отвечает за управление каталогом товаров.
- Сервис заказов: Обрабатывает создание и управление заказами.
- Сервис техподдержки: Отвечает за работу с заявками в саппорт.
- Сервис услуг: Обрабатывает предоставление услуг.
- Сервис оплаты: Управляет платежными операциями.
- Сервис уведомлений: Отправляет уведомления пользователям.
- Сервис отчеты: Генерирует различные отчеты.
- Сервис аукцион: Отвечает за создание и управление аукционом

### Другие компоненты:

- User Client, Admin Client: Интерфейсы для пользователей и администраторов.
- Api gateway: Обеспечивает единую точку входа для клиентских запросов.
- Распределенная система управления событиями: Координирует взаимодействие между сервисами.
- Адаптер для платежных провайдеров: Интегрирует внешние платежные системы.

Каждый микросервис имеет свою базу данных (postgres). Система использует событийно-ориентированную архитектуру для обмена данными между сервисами.

События расписаны глобально, их нужно будет декомпозировать на мелкие события для каждых отдельных случаев
