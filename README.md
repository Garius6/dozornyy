# дозорный

OAuth2/OIDC identity provider для [panos](https://github.com/Garius6/panos)
— упрощённый аналог [authentik](https://goauthentik.io/): Authorization
Code + PKCE flow, admin REST/JSON API (без веб-интерфейса). Строится
над [быстряга](https://github.com/Garius6/bystryaga).

Полная спека, включая скоуп v1 и итоги реализации — см.
[spec.md](spec.md).

**Статус: US1 (Authorization Code + PKCE) + US2 (admin REST/JSON API,
полный CRUD) + US3 (refresh-ротация) реализованы.** Все FR-001..FR-012
и SC-001..SC-006 закрыты. Проверено 54 e2e-проверками на реальном
сокете (`тест_дозорный.pns`) и вживую через `curl`.

## Структура

- `модель.pns` — SQLite-схема (`бд.*`) и репозитории: Пользователь/
  Группа/Клиент/КодАвторизации/AccessТокен/RefreshТокен/admin-токен.
- `пароли.pns` — PBKDF2-хэширование паролей.
- `oauth.pns` — `/authorize` → `/login` → `/consent` → `/token` →
  `/userinfo` → `/.well-known/openid-configuration`.
- `admin.pns` — REST/JSON admin API поверх `быстряга.Приложение`:
  create/read/update/delete для пользователей, групп, клиентов.
- `старт.pns` — сборка приложения, bootstrap первого admin-пользователя.
- `тест_дозорный.pns` — e2e-регрессия (реальный сокет, throwaway БД).

## Запуск

```
pan install
panos старт.pns
```

Порт/путь БД/базовый URL — константы в начале `старт.pns` (нет
CLI-флагов в v1). При пустой БД создаётся один администратор со
случайным паролем, напечатанным в лог РОВНО ОДИН РАЗ при старте.

## Тесты

```
panos тест_дозорный.pns
```

## Лицензия

MIT, см. [LICENSE](LICENSE).
