# Школьный Помощник — mobile PWA + Supabase + Gemini

Готовая мобильная PWA для Android/iOS. Внутри уже есть:
- Supabase Auth: ник + пароль;
- роли `user` / `admin`;
- расписание по классам;
- Gemini через Supabase Edge Function;
- админка с загрузкой фото расписания, редактированием и удалением классов;
- оценки с изменением/удалением, средним баллом и расчётом четвертной оценки;
- календарь в меню ☰ с заметками и цветными точками;
- светлая/тёмная тема без тёмных хвостов;
- адаптация под телефоны и safe-area.

## Supabase

1. Выполни `schema.sql` целиком в SQL Editor.
2. Authentication → Providers → Email → отключи Confirm email.
3. Зарегистрируй первый аккаунт.
4. Сделай его админом:

```sql
update public.profiles set role = 'admin' where username = 'ТВОЙ_НИК';
```

## Gemini

Edge Function: `supabase/functions/process-schedule/index.ts`.

В Supabase → Edge Functions → Secrets уже добавляются:

- `GEMINI_API_KEY` = твой Gemini API key
- `GEMINI_MODEL` = `gemini-3.6-flash`

Ключ не хранится в браузере или GitHub.

## GitHub Actions

В репозитории нужны два Actions secrets для автоматического деплоя Edge Function:

- `SUPABASE_ACCESS_TOKEN` — Personal Access Token Supabase;
- `SUPABASE_PROJECT_REF` — `igbkjkjagkhxpxezjwtj`.

После push в `main` workflow `.github/workflows/deploy-supabase.yml` сам задеплоит `process-schedule`. Workflow `.github/workflows/deploy-pages.yml` публикует PWA через GitHub Pages.

В GitHub: Settings → Pages → Source должен быть `GitHub Actions`.

## Оценки

Четвертная оценка округляется по школьному правилу: `3.50 → 4`, `3.49 → 3`, `4.50 → 5`.

## Календарь

☰ → Календарь. Нажатие на число открывает заметку. Дни с заметкой получают маленькую точку, выходные выделяются зелёным. Данные календаря сохраняются локально на устройстве.
