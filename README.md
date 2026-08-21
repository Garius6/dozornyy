# дозорный

OAuth2/OIDC identity provider для [panos](https://github.com/Garius6/panos)
— упрощённый аналог [authentik](https://goauthentik.io/): Authorization
Code + PKCE flow, admin REST/JSON API + admin SPA, упрощённый
flow-движок. Строится над [быстряга](https://github.com/Garius6/bystryaga).

Полная спека, включая скоуп v1/v1.1 и итоги реализации — см.
[spec.md](spec.md).

**Статус: US1 (Authorization Code + PKCE) + US2 (admin REST/JSON API,
полный CRUD) + US3 (refresh-ротация) реализованы.** Все FR-001..FR-012
и SC-001..SC-006 закрыты. Плюс v1.1: упрощённый flow-движок (стадии в
БД, `consent` отключаема через API/SPA) и admin SPA (WASM/`DOM.*`,
полный CRUD, без JS-фреймворка). Проверено 71 e2e-проверкой на реальном
сокете (`тест_дозорный.pns`), живым `curl` и реальным headless-браузером
(Playwright) для SPA.

## Структура

- `модель.pns` — SQLite-схема (`бд.*`) и репозитории: Пользователь/
  Группа/Клиент/КодАвторизации/AccessТокен/RefreshТокен/admin-токен/
  Стадия.
- `пароли.pns` — PBKDF2-хэширование паролей.
- `oauth.pns` — `/authorize` → `/login` → `/consent` → `/token` →
  `/userinfo` → `/.well-known/openid-configuration`; учитывает
  `стадия_включена(consent)` — выключена -> редирект с `code` сразу
  после логина, минуя экран согласия.
- `admin.pns` — REST/JSON admin API поверх `быстряга.Приложение`:
  create/read/update/delete для пользователей, групп, клиентов, стадий.
- `frontend/` — admin SPA, panos → WASM (`panos build --target=wasm`),
  `DOM.*`/`состояние.*`, отдаётся на `/admin`.
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
Admin SPA — `http://<базовый_url>/admin`, логин тем же паролем.

## Тесты

```
panos тест_дозорный.pns
```

Пересборка admin SPA после правок `frontend/main.pns`:

```
panos build --target=wasm frontend/main.pns -o frontend/main.wasm
```

## Лицензия

MIT, см. [LICENSE](LICENSE).
