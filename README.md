#  CRM System  

**CRM System** — это настольное **WPF-приложение** для управления клиентами, сделками и взаимодействиями.  
Оно разработано на **C# (.NET 8)** с использованием **Entity Framework Core** и **MVVM-архитектуры**,  
а также поддерживает отправку email-подтверждений при регистрации пользователей.

---

##  Начало работы  

Эти инструкции помогут вам **установить и запустить проект** на локальном компьютере для разработки и тестирования.

---

###  Необходимые условия  

Перед началом убедитесь, что у вас установлено:  

1. **Visual Studio 2022** с поддержкой .NET 8  
2. **SQL Server Express / LocalDB**  
3. **.NET SDK 8.0**  
4. Подключение к интернету для отправки email через SMTP  

```
# Проверка версии .NET SDK
dotnet --version
```

---

###  Установка  

1. **Клонируйте репозиторий:**  
```
git clone https://github.com/Mamblz/CRMSystem.git
```

2. **Откройте проект** в Visual Studio:  
   - `File → Open → Project/Solution → CRMSystem.sln`  

3. **Настройте строку подключения** в `appsettings.json`:  
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\MSSQLLocalDB;Database=CRMSystem;Trusted_Connection=True;"
  }
}
```

4. **Создайте базу данных:**  
```
Update-Database
```

5. **Запустите проект**:  
   - Нажмите **F5** в Visual Studio или выберите **Start Debugging**  

---

##  Основные возможности  

-  **Регистрация и авторизация пользователей**
  - Проверка пароля, валидация данных  
  - Отправка email с кодом подтверждения  
-  **Управление клиентами**
  - Добавление, редактирование, удаление  
  - История взаимодействий  
-  **Работа со сделками**
  - Этапы: _Лид → Предложение → В работе → Завершено / Отменено_  
  - Назначение ответственного менеджера  
  - Аналитика и статистика  
-  **Отправка писем через SMTP (Mail.ru)**  
-  **Entity Framework Core** для работы с БД  
-  **MVVM-паттерн** для разделения логики и интерфейса  

---

##  Пример кода  

Пример **регистрации пользователя** с хешированием пароля и подтверждением email:

```csharp
public bool Register(string username, string email, string password, DateTime registeredAt, out string errorMessage)
{
    errorMessage = string.Empty;

    if (_context.Users.Any(u => u.Username == username))
    {
        errorMessage = "Пользователь с таким именем уже существует.";
        return false;
    }

    var confirmationCode = GenerateConfirmationCode();
    var user = new User
    {
        Username = username,
        Email = email,
        PasswordHash = HashPassword(password),
        RegisteredAt = registeredAt,
        IsEmailVerified = false,
        EmailConfirmationCode = confirmationCode
    };

    _context.Users.Add(user);
    _context.SaveChanges();
    SendConfirmationEmail(email, confirmationCode);
    return true;
}
```

---

##  Интерфейс приложения  

<table>
  <tr>
    <td align="center">
      <img src="https://github.com/Mamblz/CRM/blob/master/Auth.jpg" height="400"/><br/>
      <b>Экран авторизации</b>
    </td>
    <td align="center">
      <img src="https://github.com/Mamblz/CRM/blob/master/Reg.jpg" height="400"/><br/>
      <b>Экран регистрации</b>
    </td>
    <td align="center">
      <img src="https://github.com/Mamblz/CRM/blob/master/Main.jpg" height="400"/><br/>
      <b>Экран главной страницы</b>
    </td>
  </tr>
</table>

---

##  Структура проекта  

| Папка / Файл | Описание |
|:--------------|:----------|
| **Models/** | Модели данных (User, Client, Deal) |
| **Services/** | Логика работы с данными (AuthService, ClientService, DealService) |
| **Data/** | Контекст БД (ApplicationDbContext) |
| **ViewModels/** | Логика интерфейса (MVVM) |
| **Views/** | WPF-интерфейс (XAML) |
| **EmailSend/** | Модуль для отправки писем |

---

##  Таблица стадий сделки  

| Этап | Описание |
|------|-----------|
| Лид | Начальный этап, клиент проявил интерес |
| Предложение | Отправлено коммерческое предложение |
| В работе | Работа над сделкой активна |
| Завершено | Сделка успешно завершена |
| Отменено | Сделка закрыта с отказом |

---

>  *"CRM — это не просто база данных клиентов, это сердце вашего бизнеса."*  

---

##  To-Do  

- [x] Реализовать регистрацию и подтверждение email  
- [x] Добавить CRUD для клиентов  
- [x] Реализовать управление сделками  
- [ ] Добавить отчёты и графики продаж  
- [ ] Подключить REST API для интеграции с вебом  

---

##  Используемые технологии  

-  **C# / .NET 8**
-  **WPF (Windows Presentation Foundation)**
-  **Entity Framework Core**
-  **System.Net.Mail**
-  **MVVM-паттерн**
-  **SQL Server / LocalDB**

---

## Полезные ссылки  

- [Документация .NET](https://learn.microsoft.com/dotnet/)  
- [WPF Guide](https://learn.microsoft.com/dotnet/desktop/wpf/)  
- [Entity Framework Core Docs](https://learn.microsoft.com/ef/core/)  

---

## Авторы  

* **Кобзарь Кирилл** – *Основная разработка* – [Mamblz](https://github.com/mamblz)  




