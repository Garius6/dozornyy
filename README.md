# дозорный

OAuth2/OIDC identity provider для [panos](https://github.com/Garius6/panos)
— упрощённый аналог [authentik](https://goauthentik.io/): Authorization
Code + PKCE flow, admin REST/JSON API (без веб-интерфейса). Строится
над [быстряга](https://github.com/Garius6/bystryaga).

Скоуп v1, открытые предпосылки (новые `крипто.*`-builtin'ы, нужные
раньше реализации) — см. [spec.md](spec.md).

**Статус: US1 (Authorization Code + PKCE) + US2 (admin API) + US3
(refresh-ротация) реализованы и проверены вживую (curl, не только
юнит-тесты)** — см. `модель.pns`/`пароли.pns`/`oauth.pns`/`admin.pns`/
`старт.pns`.

## Лицензия

MIT, см. [LICENSE](LICENSE).
