# Руководство по настройке Deep Research Assistant

## Пошаговая установка

### 1. Установка зависимостей
```bash
# Создание виртуального окружения
python3 -m venv .venv
source .venv/bin/activate

# Установка LangGraph CLI
pip install -U "langgraph-cli[inmem]"

# Установка проекта
pip install -e .
```

### 2. Настройка окружения
Создайте файл `.env` в корне проекта:
```env
TAVILY_API_KEY=your_tavily_key_here
OPENAI_API_KEY=your_api_key_here  
OPENAI_API_BASE=https://your-vllm-server/v1/chat/completions
OPENAI_MODEL=your_model_name
```

### 3. Запуск (для zsh)
```bash
# В новом терминале активируйте окружение
source .venv/bin/activate

# Загрузите переменные окружения и запустите сервер
set -a
source .env
set +a
langgraph dev
```

### 4. Запуск (альтернативный способ)
```bash
# Запуск с явной загрузкой переменных
env $(cat .env | grep -v '^#' | xargs) .venv/bin/langgraph dev
```

## Диагностика проблем

### Проверка переменных окружения
```bash
# Убедитесь, что переменные загружены
echo $OPENAI_API_KEY
echo $OPENAI_API_BASE
echo $OPENAI_MODEL
```

### Проверка файла .env
```bash
# Проверьте формат файла (не должно быть лишних символов)
cat -vet .env
```

### Очистка кешей Python
```bash
# При странных ошибках очистите кеши
find . -name "*.pyc" -delete
find . -name "__pycache__" -type d -exec rm -r {} +
```

## Доступ к интерфейсу
После успешного запуска откройте:
https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024

## Troubleshooting

### "LLM API Key is required"
1. Проверьте правильность переменных в `.env` (см. пример выше)
2. Перезапустите сервер в новом терминале

### "Address already in use"
```bash
# Завершите предыдущие процессы
pkill -f langgraph
# Затем запустите заново
```

### Переменные не загружаются
1. Убедитесь, что файл называется именно `.env` (а не `.env.txt`)
2. Проверьте права доступа: `ls -la .env`
3. Используйте альтернативный способ запуска с `env $(cat .env...)` 