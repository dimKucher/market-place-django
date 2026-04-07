<div align="left">
  
# 🛍️ Интернет-магазин "MEGANO"

**Полнофункциональная платформа для онлайн-торговли с ролевой моделью (покупатель, продавец, администратор), каталогом, корзиной, заказами и асинхронной обработкой задач.**

[![Django](https://img.shields.io/badge/Django-5.0-092e20?logo=django&logoColor=white)](https://www.djangoproject.com/)
[![Django REST](https://img.shields.io/badge/DRF-3.14-a30000?logo=django&logoColor=white)](https://www.django-rest-framework.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169e1?logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-7.0-dc382d?logo=redis&logoColor=white)](https://redis.io/)
[![Celery](https://img.shields.io/badge/Celery-5.3-37814a?logo=celery&logoColor=white)](https://docs.celeryq.dev/)
[![Flower](https://img.shields.io/badge/Flower-1.2-ff69b4?logo=python&logoColor=white)](https://flower.readthedocs.io/)
[![Nginx](https://img.shields.io/badge/Nginx-1.24-009639?logo=nginx&logoColor=white)](https://nginx.org/)
[![Docker](https://img.shields.io/badge/Docker-24.0-2496ed?logo=docker&logoColor=white)](https://www.docker.com/)
[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)](https://www.python.org/)

</div>

---

## 📋 Описание проекта

Приложение разработано для интернет-магазина "MEGANO" — многофункциональная платформа с разграничением ролей (покупатель, продавец, администратор), каталогом товаров, корзиной, системой заказов, избранным, сравнением товаров и панелями управления для продавцов и администраторов.

---

## 👥 Роли на сайте

| Роль | Доступные возможности |
|------|----------------------|
| **Незарегистрированный пользователь** | Просмотр каталога, детальных страниц товаров |
| **Авторизованный покупатель** | Корзина, заказы, избранное, сравнение, личный кабинет, комментарии, адреса доставки |
| **Продавец** | Панель управления, магазины, заказы, отзывы, личный кабинет |
| **Администратор** | Полное управление: пользователи, магазины, товары, заказы, категории, теги, модерация отзывов |

---

## 🗂️ Структура сайта

<details>
<summary><b>🛒 Для покупателя</b></summary>

- **Каталог товаров** – по категориям, тегам, просмотрам, новизне, продажам
- **Детальная страница товара**
- **Корзина**
- **Личный кабинет**:
  - профиль
  - заказы
  - комментарии
  - адреса доставки
  - квитанции
- **Избранные товары**
- **Товары для сравнения**

</details>

<details>
<summary><b>🏪 Для продавца</b></summary>

- Панель управления
- Список магазинов
- Детальная страница магазина
- Список заказов
- Отзывы
- Личный кабинет

</details>

<details>
<summary><b>⚙️ Для администратора</b></summary>

- Панель управления
- Управление:
  - покупателями
  - продавцами
  - магазинами
  - товарами
  - заказами
- Управление категориями и тегами
- Модерация отзывов

</details>

---

## 🧩 Приложения в проекте

| Приложение | Назначение |
|------------|------------|
| **Django** | Основной фреймворк для разработки |
| **Celery** | Асинхронные задачи (отправка писем, фоновые процессы) |
| **Flower** | Мониторинг и управление Celery-задачами |
| **PostgreSQL** | База данных |
| **Redis** | Брокер сообщений для Celery |
| **Nginx** | Раздача статических файлов |

---
## 🐳 Деплой через Docker (первый способ)

```bash
# Клонирование репозитория
git clone https://...
cd проект

# Сборка и запуск контейнеров
docker-compose build
docker-compose up
```

---

## 🖥️ Деплой на сервер (второй способ)

### 1. Установка виртуального окружения
```bash
pip install virtualenv
python -m venv venv
# Windows:
venv\Scripts\activate.bat
# Linux/MacOS:
source venv/bin/activate
```

### 2. Установка зависимостей
```bash
pip install -r requirements.txt
```

### 3. Клонирование репозитория
```bash
git clone https://...
```

### 4. Создание моделей и базы данных
```bash
python manage.py makemigrations
python manage.py migrate
```

### 5. Очистка кэша ContentType
```bash
python manage.py shell < installations/contenttype.py
```

### 6. Создание супер-пользователя
```bash
python manage.py shell < installations/superusercreator.py
# или
python manage.py createsuperuser
```

### 7. Загрузка фикстур
```bash
python manage.py loaddata fixtures/db.json
```

### 8. Сбор статических файлов
```bash
python manage.py collectstatic --noinput
```

### 9. Запуск сервера
```bash
python manage.py runserver
# Остановка: Ctrl + C
```

### 10. Запуск Celery и Flower
```bash
celery -A shop worker --pool=solo -l INFO
celery -A shop flower
```

### 11. Запуск Redis
```bash
# Linux
redis-server        # старт
redis-server stop   # стоп

# Windows: запустите redis-server.exe
```

---

## 📊 Модели базы данных

### Товар
| Модель | Описание | Связь |
|--------|----------|-------|
| `Item` | Товар | — |
| `Category` | Категория | FK → Item |
| `Tag` | Тег | M2M → Item |
| `Feature` | Характеристика | FK → Category |
| `FeatureValue` | Значение характеристики | M2M → Item |
| `Image` | Изображение | M2M → Item |
| `Comment` | Комментарий | FK → Item, User |
| `IpAddress` | IP-адрес | FK → User |

### Корзина
| Модель | Описание | Связь |
|--------|----------|-------|
| `Cart` | Корзина пользователя | FK → User |
| `CartItem` | Товар в корзине | FK → Cart |

### Квитанция
| Модель | Описание | Связь |
|--------|----------|-------|
| `Invoice` | Квитанция оплаты | FK → Order |

### Заказ
| Модель | Описание | Связь |
|--------|----------|-------|
| `Order` | Заказ | FK → User |
| `OrderItem` | Оплаченный товар | FK → Order |
| `Address` | Адрес доставки | FK → User |

### Магазин
| Модель | Описание | Связь |
|--------|----------|-------|
| `Store` | Магазин | FK → User |

### Пользователь
| Модель | Описание | Связь |
|--------|----------|-------|
| `Profile` | Профиль пользователя | O2O → User |

### Настройки
| Модель | Описание |
|--------|----------|
| `SiteSettings` | Настройки сайта |
