# vLLM Benchmark Setup & Usage Guide

## 🚀 Установка и настройка

### 1. Установка зависимостей

```bash
# Установка основных зависимостей
pip install -r requirements_vllm.txt

# Или установка через pip
pip install vllm torch transformers huggingface_hub psutil GPUtil requests aiohttp pydantic numpy pandas
```

### 2. Настройка окружения

```bash
# Создание директории для моделей
mkdir -p models

# Настройка токена HuggingFace (опционально, для приватных моделей)
export HF_TOKEN="your_huggingface_token_here"

# Настройка CUDA (если используется GPU)
export CUDA_VISIBLE_DEVICES=0
```

### 3. Проверка установки

```bash
# Проверка vLLM
python -c "import vllm; print('vLLM установлен успешно')"

# Проверка GPU (если есть)
python -c "import torch; print(f'CUDA доступна: {torch.cuda.is_available()}')"
```

## 📊 Запуск бенчмарка

### Базовое использование

```bash
# Запуск с предустановленными моделями (легкие модели)
python scripts/vllm_benchmark_cli.py --preset example

# Запуск с продвинутыми моделями (требуют больше ресурсов)
python scripts/vllm_benchmark_cli.py --preset advanced
```

### Тестирование одной модели

```bash
# Тестирование TinyLlama
python scripts/vllm_benchmark_cli.py --model "TinyLlama/TinyLlama-1.1B-Chat-v1.0"

# Тестирование с кастомным именем
python scripts/vllm_benchmark_cli.py --model "microsoft/DialoGPT-medium" --name "Мой DialoGPT"

# Тестирование Qwen модели
python scripts/vllm_benchmark_cli.py --model "Qwen/Qwen2-0.5B-Instruct"
```

### Расширенные параметры

```bash
# Запуск с DEBUG логированием
python scripts/vllm_benchmark_cli.py --preset example --log-level DEBUG

# Указание кастомной директории для результатов
python scripts/vllm_benchmark_cli.py --model "TinyLlama/TinyLlama-1.1B-Chat-v1.0" --results-dir custom_results
```

## 🎯 Предустановленные конфигурации

### Example Models (легкие модели)
- **TinyLlama 1.1B Chat** - Маленькая чат-модель
- **DialoGPT Medium** - Средняя диалоговая модель
- **Qwen2 0.5B Instruct** - Компактная инструкционная модель
- **DialoGPT Small** - Маленькая диалоговая модель

### Advanced Models (требуют больше ресурсов)
- **DialoGPT Large** - Большая диалоговая модель
- **Qwen2 1.5B Instruct** - Средняя инструкционная модель  
- **Qwen2 7B Instruct** - Большая инструкционная модель

## 🔧 Конфигурация модели

Вы можете настроить собственные модели, редактируя конфигурацию в `scripts/vllm_benchmark_cli.py`:

```python
custom_model = {
    "model_path": "your-org/your-model",        # HuggingFace ID модели
    "display_name": "Ваша Модель",              # Отображаемое имя
    "tensor_parallel_size": 1,                  # Параллелизм по тензорам
    "gpu_memory_utilization": 0.8,             # Использование GPU памяти (0.1-0.9)
    "max_model_len": 4096,                      # Максимальная длина контекста
    "port": 8000,                               # Порт для vLLM API
    "download_to_local": True,                  # Загружать в локальную папку
    "local_models_dir": "./models",             # Папка для моделей
    "temperature": 0.7                          # Температура генерации
}
```

## 📂 Структура результатов

После запуска бенчмарка создается следующая структура:

```
vllm_benchmark_results/
├── Model_Name/
│   ├── results_TIMESTAMP.json          # Подробные результаты
│   └── report_TIMESTAMP.md             # Человекочитаемый отчет
├── vllm_benchmark_summary_TIMESTAMP.json  # Сводка всех моделей
└── vllm_benchmark.log                     # Лог выполнения
```

## 💡 Полезные советы

### Оптимизация ресурсов

1. **Для маломощных систем:**
   ```bash
   # Используйте маленькие модели
   python scripts/vllm_benchmark_cli.py --model "microsoft/DialoGPT-small"
   ```

2. **Для систем с ограниченной GPU памятью:**
   - Уменьшите `gpu_memory_utilization` до 0.4-0.6
   - Используйте модели размером до 1.5B параметров

3. **Для быстрого тестирования:**
   - Код уже настроен на минимальное количество игр и раундов
   - `num_games=1, max_rounds=3-6` в зависимости от игры

### Мониторинг ресурсов

```bash
# Мониторинг GPU во время выполнения
watch -n 1 nvidia-smi

# Мониторинг системных ресурсов
htop
```

### Troubleshooting

1. **Ошибка CUDA out of memory:**
   - Уменьшите `gpu_memory_utilization`
   - Используйте меньшую модель
   - Уменьшите `max_model_len`

2. **Модель не загружается:**
   - Проверьте интернет-соединение
   - Убедитесь, что модель существует на HuggingFace
   - Проверьте токен HF_TOKEN для приватных моделей

3. **vLLM API недоступен:**
   - Дождитесь полной загрузки модели (до 5 минут)
   - Проверьте, что порт не занят другим процессом

## 🎮 Тестируемые игры

Бенчмарк автоматически тестирует модели на следующих играх:

1. **Diplomacy** - Дипломатическая стратегия (3 раунда)
2. **Beast** - Логическая игра (4 раунда) 
3. **Spyfall** - Детективная игра (4 раунда)
4. **AskGuess** - Игра в вопросы и ответы (6 раундов)
5. **ToFuKingdom** - Ролевая игра (5 раундов)

## 📈 Метрики оценки

- **Overall Score** - Общий балл модели
- **Success Rate** - Процент успешно завершенных игр
- **Average Response Time** - Среднее время ответа
- **Memory Usage** - Использование памяти GPU/RAM

## 🔗 Дополнительные ресурсы

- [vLLM Documentation](https://docs.vllm.ai/)
- [HuggingFace Models Hub](https://huggingface.co/models)
- [Benchmark Runner Documentation](./scripts/benchmark_runner.py) 