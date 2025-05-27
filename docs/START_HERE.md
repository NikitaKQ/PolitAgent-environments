# 🎯 НАЧАТЬ ЗДЕСЬ - vLLM Benchmark

## ⚡ СУПЕР-БЫСТРЫЙ ЗАПУСК (2 минуты)

### 1️⃣ Одна команда для установки
```bash
./run_vllm_benchmark.sh setup
```

### 2️⃣ Одна команда для тестирования
```bash
./run_vllm_benchmark.sh model TinyLlama/TinyLlama-1.1B-Chat-v1.0
```

**Готово!** 🎉 Модель автоматически:
- Скачается из HuggingFace (~1GB)
- Запустится через vLLM
- Протестируется на 5 играх
- Создаст отчеты в `vllm_benchmark_results/`

## 🔥 ЧТО ЭТО ДЕЛАЕТ?

Автоматически тестирует **любую модель с HuggingFace** на 5 играх с ИИ агентами:
- **Diplomacy** - дипломатия и переговоры
- **Beast** - логика и дедукция  
- **Spyfall** - обман и анализ
- **AskGuess** - коммуникация
- **ToFuKingdom** - повествование

## 📊 ДРУГИЕ ВАРИАНТЫ ЗАПУСКА

```bash
# Набор легких моделей (4 модели ~4GB)
./run_vllm_benchmark.sh example

# DialoGPT для диалогов
./run_vllm_benchmark.sh model microsoft/DialoGPT-medium

# Qwen для инструкций
./run_vllm_benchmark.sh model Qwen/Qwen2-0.5B-Instruct

# Мощные модели (нужно >8GB VRAM)
./run_vllm_benchmark.sh advanced

# Справка по всем командам
./run_vllm_benchmark.sh help
```

## 🎮 РЕЗУЛЬТАТЫ

После завершения смотрите:
```bash
# Список всех результатов
ls vllm_benchmark_results/

# Сводный отчет (человекочитаемый)
cat vllm_benchmark_results/vllm_benchmark_summary_*.json

# Подробные результаты конкретной модели
ls vllm_benchmark_results/TinyLlama*/
```

## 💡 РЕКОМЕНДУЕМЫЕ МОДЕЛИ

### Для первого раза (быстро)
- `microsoft/DialoGPT-small` - 5 минут
- `TinyLlama/TinyLlama-1.1B-Chat-v1.0` - 10 минут

### Для экспериментов (средне) 
- `microsoft/DialoGPT-medium` - 15 минут
- `Qwen/Qwen2-0.5B-Instruct` - 10 минут

### Для серьезных тестов (долго)
- `microsoft/DialoGPT-large` - 30 минут
- `Qwen/Qwen2-7B-Instruct` - 60 минут

## ⚠️ ТРЕБОВАНИЯ

**Минимум**: Python 3.8+, 4GB RAM, интернет  
**Оптимально**: Python 3.10+, GPU с 4GB+ VRAM

## 🆘 ЕСЛИ ЧТО-ТО НЕ РАБОТАЕТ

### Нет vLLM?
```bash
pip install vllm torch transformers huggingface_hub
```

### Мало памяти?
```bash
./run_vllm_benchmark.sh model microsoft/DialoGPT-small
```

### Нет GPU?
```bash
# Все равно работает, просто медленнее
./run_vllm_benchmark.sh model microsoft/DialoGPT-small
```

### Другие проблемы?
```bash
# Смотрите логи
tail -f vllm_benchmark.log

# Или читайте подробную документацию
cat README_VLLM.md
```

---

## 📚 ПОЛНАЯ ДОКУМЕНТАЦИЯ

- **[README_VLLM.md](README_VLLM.md)** - Полная документация
- **[QUICK_START.md](QUICK_START.md)** - Быстрый старт  
- **[VLLM_SETUP.md](VLLM_SETUP.md)** - Детальная установка
- **[vllm_config_examples.json](vllm_config_examples.json)** - Примеры конфигов

---

**🚀 Запускайте `./run_vllm_benchmark.sh setup` прямо сейчас!** 