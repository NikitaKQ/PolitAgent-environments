# 🤖 vLLM Benchmark для PolitAgent Environments

Система автоматического тестирования языковых моделей из HuggingFace через vLLM на играх с ИИ агентами.

## ✨ Особенности

- 🚀 **Автоматическая загрузка** моделей из HuggingFace Hub
- 💾 **Локальное кэширование** моделей для быстрых повторных запусков
- ⚡ **vLLM оптимизация** для максимальной производительности
- 🎮 **5 различных игр** для комплексного тестирования
- 📊 **Подробная аналитика** и сравнительные отчеты
- 🔧 **Гибкая конфигурация** под любые ресурсы
- 📱 **Удобный CLI** с bash-скриптами

## 🚀 Быстрый старт

### Супер-быстрый запуск (3 команды)

```bash
# 1. Установка зависимостей
./run_vllm_benchmark.sh setup

# 2. Запуск легких моделей
./run_vllm_benchmark.sh example

# 3. Просмотр результатов
ls vllm_benchmark_results/
```

### Тестирование одной модели

```bash
# TinyLlama (легкая модель ~1GB)
./run_vllm_benchmark.sh model TinyLlama/TinyLlama-1.1B-Chat-v1.0

# DialoGPT для диалогов
./run_vllm_benchmark.sh model microsoft/DialoGPT-medium

# Qwen для инструкций
./run_vllm_benchmark.sh model Qwen/Qwen2-0.5B-Instruct
```

## 📋 Требования

### Минимальные
- Python 3.8+
- 4GB RAM
- 2GB свободного места
- Интернет-соединение

### Рекомендуемые
- Python 3.10+
- 8GB RAM + 4GB VRAM
- 10GB свободного места
- CUDA-совместимая GPU

## 📦 Установка

### Автоматическая (рекомендуется)
```bash
./run_vllm_benchmark.sh setup
```

### Ручная
```bash
# Установка зависимостей
pip install -r requirements_vllm.txt

# Создание директорий
mkdir -p models vllm_benchmark_results

# Проверка установки
python -c "import vllm; print('vLLM готов!')"
```

## 🎯 Использование

### CLI команды

```bash
# Показать справку
./run_vllm_benchmark.sh help

# Легкие модели (< 2GB каждая)
./run_vllm_benchmark.sh example

# Мощные модели (требует >8GB VRAM)
./run_vllm_benchmark.sh advanced

# Одна модель
./run_vllm_benchmark.sh model <huggingface_model_id>

# Мониторинг ресурсов
./run_vllm_benchmark.sh monitor

# Очистка кэша
./run_vllm_benchmark.sh clean
```

### Python API

```python
from scripts.vllm_benchmark_cli import SequentialVLLMBenchmark, VLLMModelConfig

# Создание бенчмарка
benchmark = SequentialVLLMBenchmark()

# Добавление модели
model_config = VLLMModelConfig(
    model_path="TinyLlama/TinyLlama-1.1B-Chat-v1.0",
    display_name="TinyLlama Test",
    download_to_local=True,
    local_models_dir="./models"
)
benchmark.add_vllm_model(model_config)

# Запуск
import asyncio
summary = asyncio.run(benchmark.run_sequential_benchmark())
```

## 🎮 Тестируемые игры

| Игра | Описание | Раунды | Специализация |
|------|----------|--------|---------------|
| **Diplomacy** | Дипломатическая стратегия | 3 | Переговоры, стратегия |
| **Beast** | Логическая игра | 4 | Дедукция, логика |
| **Spyfall** | Детективная игра | 4 | Обман, анализ |
| **AskGuess** | Вопросы и ответы | 6 | Коммуникация |
| **ToFuKingdom** | Ролевая игра | 5 | Повествование |

## 📊 Метрики оценки

- **Overall Score** - Общий балл модели (0-100)
- **Success Rate** - Процент успешно завершенных игр
- **Response Time** - Среднее время ответа (сек)
- **Game Performance** - Эффективность по играм
- **Memory Usage** - Использование GPU/RAM

## 📂 Структура результатов

```
vllm_benchmark_results/
├── TinyLlama_1.1B_Chat/                    # Результаты модели
│   ├── results_20240101_123456.json       # Подробные данные
│   └── report_20240101_123456.md          # Человекочитаемый отчет
├── DialoGPT_Medium/
│   ├── results_20240101_134567.json
│   └── report_20240101_134567.md
├── vllm_benchmark_summary_1704123456.json # Сводка всех моделей
└── vllm_benchmark.log                     # Логи выполнения
```

## 🔧 Конфигурация

### Предустановленные наборы

| Набор | Описание | Требования |
|-------|----------|------------|
| `example` | Легкие модели | 2GB VRAM |
| `advanced` | Мощные модели | 8GB+ VRAM |

### Кастомная конфигурация

Создайте JSON файл с конфигурацией:

```json
{
  "model_path": "your-org/your-model",
  "display_name": "Ваша Модель",
  "tensor_parallel_size": 1,
  "gpu_memory_utilization": 0.8,
  "max_model_len": 4096,
  "download_to_local": true,
  "local_models_dir": "./models"
}
```

См. `vllm_config_examples.json` для больших примеров.

## 🛠 Оптимизация производительности

### Для слабых систем
```bash
# Используйте маленькие модели
./run_vllm_benchmark.sh model microsoft/DialoGPT-small

# Уменьшите gpu_memory_utilization в конфиге до 0.4-0.6
```

### Для мощных систем
```bash
# Запускайте продвинутые модели
./run_vllm_benchmark.sh advanced

# Увеличьте max_model_len для лучшего контекста
```

### Экономия памяти
- Запускайте модели последовательно (не параллельно)
- Используйте `tensor_parallel_size=1`
- Очищайте кэш между запусками

## 🔍 Мониторинг

### Во время выполнения
```bash
# Запустите в отдельном терминале
./run_vllm_benchmark.sh monitor

# Или используйте nvidia-smi
watch -n 1 nvidia-smi
```

### Анализ результатов
```python
import json

# Загрузка результатов
with open('vllm_benchmark_results/summary.json') as f:
    data = json.load(f)

# Анализ
for result in data['results']:
    if result['status'] == 'completed':
        print(f"{result['model_name']}: {result['ratings'][0]['overall_score']}")
```

## 🆘 Troubleshooting

### Частые проблемы

**CUDA out of memory**
```bash
# Решение: уменьшите gpu_memory_utilization
# Отредактируйте конфиг или используйте меньшую модель
```

**Модель не загружается**
```bash
# Проверьте интернет и HuggingFace токен
export HF_TOKEN="your_token_here"
```

**vLLM API недоступен**
```bash
# Дождитесь полной загрузки (до 5 минут)
# Проверьте логи: tail -f vllm_benchmark.log
```

**Нет GPU**
```bash
# vLLM автоматически переключится на CPU
# Производительность будет ниже, но функциональность сохранится
```

## 📈 Рекомендуемые модели

### Для начинающих
- `microsoft/DialoGPT-small` (~300MB)
- `TinyLlama/TinyLlama-1.1B-Chat-v1.0` (~1GB)

### Для экспериментов
- `microsoft/DialoGPT-medium` (~800MB)
- `Qwen/Qwen2-0.5B-Instruct` (~500MB)

### Для серьезной работы
- `microsoft/DialoGPT-large` (~3GB)
- `Qwen/Qwen2-1.5B-Instruct` (~1.5GB)
- `Qwen/Qwen2-7B-Instruct` (~7GB)

### Русскоязычные
- `ai-forever/rugpt3small_based_on_gpt2`
- `IlyaGusev/saiga_mistral_7b_gguf`

## 🤝 Интеграция

### С существующим benchmark_runner
```python
# vLLM бенчмарк интегрируется с основной системой
from scripts.benchmark_runner import BenchmarkRunner
# Автоматически используется при запуске
```

### С другими системами
```python
# Экспорт результатов в различные форматы
# JSON, CSV, Excel поддерживаются
```

## 📚 Дополнительные ресурсы

- 📖 [Подробная документация](VLLM_SETUP.md)
- 🚀 [Быстрый старт](QUICK_START.md)
- ⚙️ [Примеры конфигураций](vllm_config_examples.json)
- 🐛 [Отчеты об ошибках](https://github.com/your-repo/issues)

## 📄 Лицензия

MIT License - используйте свободно для исследований и коммерческих проектов.

---

**🎉 Готово к использованию!** Запустите `./run_vllm_benchmark.sh setup` для начала работы. 