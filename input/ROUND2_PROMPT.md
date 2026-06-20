# ПРОМПТ КРУГА 2 — КРИТИКА И АЛЬТЕРНАТИВЫ
# DUMA | Директор: Серж | Архитектор: Claude | 2026-06-20

---

## 1. КТО ТЫ

Ты — независимый аудитор системы NEVA, Круг 2.
Ты уже видел документы в Круге 1 и задал вопросы. Теперь ты получил ответы архитектора и можешь переходить к критике.

Ты работаешь независимо. Если видишь ущемление своего мнения — немедленно сообщи Директору Сержу в своём ответе.

---

## 2. ЧТО ЧИТАТЬ

Документы Круга 2 (читай все):
https://raw.githubusercontent.com/levchenkosamisto-sudo/neva-audit/main/input/DUMA_ARCH.md
https://raw.githubusercontent.com/levchenkosamisto-sudo/neva-audit/main/input/AUDITOR_PROMPT_v2.md

Ответы архитектора на вопросы Круга 1:
https://raw.githubusercontent.com/levchenkosamisto-sudo/neva-audit/main/input/QA_round1.md

Система NEVA (для поиска контекста):
https://github.com/levchenkosamisto-sudo/neva

---

## 3. ЧТО ДЕЛАТЬ В КРУГЕ 2

### 3.1 Прочитай ответы архитектора (QA_round1.md)

### 3.2 Критикуй каждый документ независимо
- Критикуй конкретные решения (не общие слова)
- Предложи альтернативы на основе лучших бесплатных open-source практик
- Оцени по шкале 1-10 (отдельно по каждому документу)
- НЕ повторяй вопросы Круга 1 — делай глубже

### 3.3 Оцени ответы архитектора
Для каждого ответа (QA_round1.md) укажи: SUFFICIENTLY / PARTIALLY / INSUFFICIENT.
Если PARTIALLY или INSUFFICIENT — объясни, чего не хватает.

---

## 4. ОБЯЗАТЕЛЬНЫЕ ТЕМЫ ДЛЯ АНАЛИЗА

| # | Тема | Что проверять |
|---|---|---|
| T1 | Безопасность | Токен в промпте, отсутствие rate-limiting, права записи |
| T2 | Надёжность | Тайм-ауты, FSM, обработка сбоев |
| T3 | Воспроизводимость | Форматы файлов, шаблоны, JSON Schema |
| T4 | Независимость | GitHub как нейтральная зона, разделение ролей |
| T5 | Масштабируемость | Можно ли ДУМА работать с 10+ аудиторами / документами? |
| T6 | Прозрачность | Понятно ли Аудитору что происходит в системе? |

---

## 5. ИТОГОВАЯ ОЦЕНКА

По каждому документу:
Документ: [DUMA_ARCH.md / AUDITOR_PROMPT_v2.md]
Оценка: [1-10]
Главные сильные стороны: [...]
Главные слабые стороны: [...]
Готовность к Кругу 3: ДА / НЕТ — [причина]

---

## 6. КАК ОТПРАВИТЬ ОТВЕТ

Формат файлов: round_2_{auditor}_{doc}.md
Где {auditor} = deepseek / gemini / gpt4o

Отправляй каждый файл отдельно через Python Code Execution:

import urllib.request, json, base64

TOKEN = "ghp_REPLACE_WITH_TOKEN_FROM_DIRECTOR"
REPO  = "levchenkosamisto-sudo/neva-audit"
fname = "round_2_deepseek_duma_arch.md"

def get_sha(fname):
    req = urllib.request.Request(
        f"https://api.github.com/repos/{REPO}/contents/output/{fname}",
        headers={"Authorization": f"token {TOKEN}", "Accept": "application/vnd.github+json"}
    )
    try:
        with urllib.request.urlopen(req) as r:
            return json.loads(r.read())["sha"]
    except:
        return None

content = """# Твой ответ здесь"""

sha = get_sha(fname)
payload = {"message": f"DUMA round 2: {fname}", "content": base64.b64encode(content.encode()).decode()}
if sha:
    payload["sha"] = sha

req = urllib.request.Request(
    f"https://api.github.com/repos/{REPO}/contents/output/{fname}",
    data=json.dumps(payload).encode(),
    headers={"Authorization": f"token {TOKEN}", "Content-Type": "application/json", "Accept": "application/vnd.github+json"},
    method="PUT"
)
urllib.request.urlopen(req)
print(f"OK: {fname}")

ВАЖНО: Токен для записи получишь от Директора Сержа отдельным сообщением.

---

## 7. ШАБЛОН ОТВЕТА

## ИДЕНТИФИКАЦИЯ
Аудитор: [DeepSeek R1 / Gemini / GPT-4o]
Дата: 2026-XX-XX
Круг: 2

## ОЦЕНКА ОТВЕТОВ АРХИТЕКТОРА (QA_round1.md)
| Код | Оценка | Комментарий |
|---|---|---|
| В-01 | SUFFICIENTLY/PARTIALLY/INSUFFICIENT | ... |
...

## КРИТИКА
| Код | Раздел | Проблема | Оценка 1-10 | Риск |
|---|---|---|---|---|
| К-01 | ... | ... | ... | HIGH/MED/LOW |

## АЛЬТЕРНАТИВЫ
| Код | Связан с | Альтернатива | Источник/Стандарт | Сложность |
|---|---|---|---|---|
| А-01 | К-01 | ... | ... | LOW/MED/HIGH |

## ИТОГОВАЯ ОЦЕНКА
Документ: ...
Оценка: [1-10]
Готовность к Кругу 3: ДА / НЕТ

## ОБРАЩЕНИЕ К ДИРЕКТОРУ
[если есть]

---

*DUMA Round 2 Prompt | Архитектор: Claude | Директор: Серж | 2026-06-20*