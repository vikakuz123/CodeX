# Bitrix24 to Qdrant Sync

Этот проект выгружает `компании` и `сделки` из Bitrix24, превращает их в текстовые документы, создает эмбеддинги через OpenAI и сохраняет в Qdrant.

## Что делает

- читает компании через `crm.company.list`
- читает сделки через `crm.deal.list`
- собирает нормализованный текст для векторизации
- создает embeddings через OpenAI
- загружает точки в одну коллекцию Qdrant

## Подготовка

1. Создай виртуальное окружение:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

2. Установи зависимости:

```powershell
pip install -r requirements.txt
```

3. Создай `.env` на основе примера:

```powershell
Copy-Item .env.example .env
```

4. Заполни переменные:

- `BITRIX_WEBHOOK_BASE`
- `QDRANT_URL`
- `QDRANT_API_KEY` если нужен
- `QDRANT_COLLECTION`
- `OPENAI_API_KEY`
- `OPENAI_EMBEDDING_MODEL`

## Запуск

```powershell
python .\sync_bitrix_to_qdrant.py
```

## Что попадет в Qdrant

Каждая точка содержит:

- `entity_type`: `company` или `deal`
- `entity_id`: id сущности в Bitrix24
- `title`: название компании или сделки
- `document`: текст, который ушел в embeddings
- `source`: `bitrix24`
- `raw`: исходные поля записи

## GitHub

После заполнения файлов и проверки можно выполнить:

```powershell
git add .
git commit -m "Add Bitrix24 to Qdrant sync"
git push -u origin main
```

## Что еще нужно для полной автоматизации

- GitHub token для push или GitHub Actions
- доступный Qdrant сервер
- секреты в `.env` локально или в secrets на сервере/GitHub
