# PolitAgent Benchmark System

Комплексная система для бенчмаркинга LLM моделей на всех играх PolitAgent environments с параллельным выполнением и детальной аналитикой.

## 🚀 Быстрый старт

### 1. Подготовка
```bash
# Установите зависимости
pip install pandas matplotlib seaborn pyyaml

# Настройте API ключи (опционально)
export OPENAI_API_KEY="your-openai-key"
export MISTRAL_API_KEY="your-mistral-key"

# Или запустите локальную модель Ollama
ollama serve
ollama pull llama2
ollama pull mistral
```

### 2. Запуск бенчмарка
```bash
# Быстрый тест (для проверки)
python run_benchmark.py --profile quick

# Полный бенчмарк
python run_benchmark.py --profile full

# Только OpenAI модели
python run_benchmark.py --profile openai_only

# Только локальные модели
python run_benchmark.py --profile local_only

# Показать доступные профили
python run_benchmark.py --list-profiles
```

### 3. Анализ результатов
```bash
# Базовый анализ последних результатов
python scripts/benchmark_analyzer.py

# Полный анализ с графиками и экспортом
python scripts/benchmark_analyzer.py --generate-plots --export-csv
```

## 📊 Система рейтинга

### Компоненты рейтинга

1. **Базовый скор (0-100)**: Success Rate × 100
2. **Бонус за качество (0-30)**: 
   - Average Quality Score × 20 (до 20 очков)
   - Decision Consistency × 10 (до 10 очков)
3. **Бонус за эффективность (0-10)**: Быстрое выполнение
4. **Штраф за ошибки**: -10 очков за каждую ошибку

### Веса игр (настраиваемые):
- **Diplomacy**: 25% (сложная стратегическая игра)
- **Beast**: 20% (социальная дедукция)
- **Spyfall**: 20% (быстрая дедукция)
- **AskGuess**: 15% (логические вопросы)
- **TofuKingdom**: 20% (ролевая дедукция)

**Итоговый рейтинг** = Σ(Скор_игры × Вес_игры)

## 🎮 Конфигурация

### Файл конфигурации: `configs/benchmark_config.yaml`

#### Настройка моделей:
```yaml
models:
  my_custom_model:
    provider: "openai"
    model_name: "gpt-4-turbo"
    display_name: "My Custom GPT-4"
    temperature: 0.5
    enabled: true
```

#### Настройка игр:
```yaml
games:
  diplomacy:
    enabled: true
    num_games: 3      # Количество игр для усреднения
    max_rounds: 5     # Максимум раундов
    num_players: 7
```

#### Создание профилей:
```yaml
profiles:
  my_profile:
    games:
      diplomacy: {num_games: 2, max_rounds: 4}
      beast: {num_games: 3, max_rounds: 6}
    models:
      - openai_gpt35
      - ollama_llama2
```

## 📈 Метрики и анализ

### Автоматически собираемые метрики:

#### Общие метрики:
- **Success Rate**: Процент успешно завершенных игр
- **Execution Time**: Среднее время выполнения
- **Total Inferences**: Общее количество решений модели
- **Error Rate**: Процент игр с ошибками

#### Специфичные для игр:
- **Diplomacy**: Стратегическая глубина, качество переговоров
- **Beast**: Точность дедукции, социальные навыки
- **Spyfall**: Скорость идентификации, качество вопросов
- **AskGuess**: Эффективность вопросов, логика
- **TofuKingdom**: Ролевая игра, обнаружение обмана

### Типы отчетов:

1. **JSON отчет**: Полные данные для программного анализа
2. **Markdown отчет**: Человеко-читаемый анализ
3. **CSV экспорт**: Данные для Excel/анализа
4. **Графики**: Визуализация производительности

## 🛠 Продвинутое использование

### Параллельность и производительность:
```yaml
benchmark:
  max_parallel_models: 3  # Модели параллельно
  max_parallel_games: 2   # Игры параллельно для одной модели
```

### Добавление новых моделей:
```python
# В benchmark_runner.py
runner = BenchmarkRunner()
runner.add_model("anthropic", "claude-3", "Claude 3", temperature=0.7)
```

### Кастомизация весов:
```python
runner.game_weights = {
    "diplomacy": 0.4,    # Больше веса для Diplomacy
    "beast": 0.3,
    "spyfall": 0.15,
    "askguess": 0.1,
    "tofukingdom": 0.05
}
```

## 📋 Примеры команд

### Различные режимы запуска:
```bash
# Тест одной модели на одной игре (для отладки)
python run_benchmark.py --profile quick

# Сравнение температур GPT-3.5
python run_benchmark.py --profile openai_only

# Benchmark только локальных моделей
python run_benchmark.py --profile local_only

# Полный production тест
python run_benchmark.py --profile full
```

### Анализ и визуализация:
```bash
# Быстрый анализ
python scripts/benchmark_analyzer.py

# Полный анализ с графиками
python scripts/benchmark_analyzer.py --generate-plots --export-csv

# Анализ конкретного файла
python scripts/benchmark_analyzer.py -f benchmark_results/benchmark_results_20241215_143022.json

# Сохранение в кастомную директорию
python scripts/benchmark_analyzer.py -o my_analysis --generate-plots
```

## 📁 Структура результатов

```
benchmark_results/
├── benchmark_results_20241215_143022.json    # Полные результаты
├── benchmark_report_20241215_143022.md       # Человеко-читаемый отчет
├── openai_gpt-3.5-turbo_diplomacy_games.json # Результаты игр
├── openai_gpt-3.5-turbo_diplomacy_metrics.json # Метрики
└── ...

analysis_output/
├── summary_report.md           # Сводный анализ
├── model_comparison.csv        # CSV с данными
└── plots/
    ├── overall_ranking.png     # Общий рейтинг
    ├── success_vs_time.png     # Скорость vs точность
    └── games_heatmap.png       # Производительность по играм
```

## 🔧 Устранение неполадок

### Частые проблемы:

1. **"Не найдено доступных моделей"**
   - Проверьте API ключи
   - Убедитесь, что Ollama запущен (для локальных моделей)

2. **Ошибки импорта**
   - Установите зависимости: `pip install -r requirements.txt`

3. **Медленное выполнение**
   - Уменьшите `num_games` и `max_rounds` в конфиге
   - Используйте профиль `quick`

4. **Ошибки в играх**
   - Проверьте логи в `benchmark.log`
   - Убедитесь, что модели корректно отвечают

### Логирование:
```bash
# Просмотр логов
tail -f benchmark.log

# Логи конкретной игры
grep "diplomacy" benchmark.log
```

## 🎯 Рекомендации по использованию

### Для разработки:
1. Используйте профиль `quick` для быстрых итераций
2. Тестируйте новые модели сначала на одной игре
3. Следите за логами для отладки

### Для исследований:
1. Используйте профиль `full` для точных сравнений
2. Запускайте несколько итераций для статистической значимости
3. Анализируйте детальные метрики по играм

### Для продакшена:
1. Настройте веса игр под ваши задачи
2. Добавьте специфичные для домена метрики
3. Автоматизируйте регулярное тестирование

## 🤝 Расширение системы

### Добавление новой игры:
1. Создайте класс игры в `environments/`
2. Добавьте метрики в `metrics/`
3. Обновите `benchmark_config.yaml`
4. Зарегистрируйте в `benchmark_runner.py`

### Добавление новых метрик:
1. Расширьте соответствующий класс в `metrics/`
2. Обновите `calculate_model_ratings()` для учета новых метрик
3. Добавьте в отчеты

---

**💡 Совет**: Начните с профиля `quick` для ознакомления, затем переходите к `full` для серьезного тестирования! 