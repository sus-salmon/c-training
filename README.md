# Проект Изучения C11

**ОСНОВНОЙ ЯЗЫК ПРОЕКТА: РУССКИЙ**

- **Стандарт:** C11
- **ОС:** macOS 15+
- **Компилятор:** clang
- **Тесты:** Unity и IO Test Framework; санитайзеры включены
- **Стиль:** clang-format (Google) + cppcheck

## Быстрый Старт

```bash
# Форматирование и анализ
scripts/check-format.sh
scripts/check-cppcheck.sh

# Сборка и тестирование всего
make build
make test

# Помощь по доступным командам
make help

# Добавить новую задачу
# Создавайте задачи через натуральный язык в Cursor AI
```

## Установка Инструментов

### Установка через Homebrew

```bash
# Установка инструментов разработки
brew install cppcheck clang-format

# Проверка установки
cppcheck --version
clang-format --version
```

## Фильтрация

```bash
make TOPIC=02-control-flow test
make TOPIC=02-control-flow TASK=02-01 build
```

## Структура Проекта

```
.github/
  copilot-instructions.md
  workflows/
    ci.yml
makefile                      # корневой
scripts/
  add-task.sh
  check-format.sh
  check-cppcheck.sh
src/
  topics/
    01-basics/
      01-01-hello-world/
        build/               # в gitignore
        makefile             # для каждой задачи
        01-01-hello-world.c
        01-01-hello-world.test.c
        .dyn_alloc_allowed   # 0 или 1
third_party/
  unity/ (unity.h, unity.c)   # включено в проект
  io_test/ (io_test.h)        # IO тесты для начинающих
.clang-format
.editorconfig
```

## Тестирование

Проект поддерживает **два вида тестирования**:

### 🟢 IO Tests (для начинающих)

Подходит для студентов, которые еще не готовы к чистым функциям и предпочитают работать с привычными `scanf`/`printf`.

⚠️ **Внимание:** IO тесты используют Unix-специфичные системные вызовы (`fork`, `pipe`, `execl`) и работают только на macOS. Windows не поддерживается.

**Пример использования:**

```c
#include "../../../../third_party/io_test/io_test.h"

int main(void) {
    IO_TEST_BEGIN("Проверка сложения");
    IO_TEST_INPUT("5\n+\n3\n");
    IO_TEST_EXPECT_OUTPUT("Введите число: Введите операцию: Введите число: Результат: 8\n");
    IO_TEST_END();

    IO_TEST_RUN_ALL("./build/program-name");
    return 0;
}
```

### 🔵 Unity Tests (для продвинутых)

Подходит для студентов, понимающих разделение логики и пользовательского интерфейса.

**Пример использования:**

```c
#include "unity.h"

void test_add_function(void) {
    TEST_ASSERT_EQUAL_INT(8, add(5, 3));
}

int main(void) {
    UNITY_BEGIN();
    RUN_TEST(test_add_function);
    return UNITY_END();
}
```

**Выбор типа тестов автоматический** - makefile определяет, какой использовать, по `#include` в тестовом файле.

## 🚀 Первые Шаги

### Шаг 1: Изучение структуры

1. Откройте каталог `src/topics/01-basics/01-01-hello-world/`
2. Изучите файлы:
   - `01-01-hello-world.c` - основная программа (с заглушкой)
   - `01-01-hello-world.test.c` - тесты (уже готовые)
   - `makefile` - конфигурация сборки
   - `.dyn_alloc_allowed` - разрешение динамической памяти (0 = запрещено)

### Шаг 2: Запуск тестов (должны провалиться)

```bash
cd src/topics/01-basics/01-01-hello-world/
make test
```

Тесты провалятся - это нормально! Программа пока не реализована.

### Шаг 3: Реализация программы

1. Откройте `01-01-hello-world.c` в редакторе
2. Найдите комментарии `TODO:` - они подскажут, что делать
3. Реализуйте функциональность согласно подсказкам
4. Сохраните файл

### Шаг 4: Проверка решения

```bash
make test        # Тесты должны пройти
make build       # Создаёт обычный исполняемый файл
make debug       # Создаёт файл для отладки
./build/01-01-hello-world  # Запуск программы вручную
```

### Шаг 5: Отладка в VS Code (опционально)

1. Откройте `01-01-hello-world.c` в VS Code
2. Поставьте точку останова (breakpoint)
3. Нажмите **F5** для отладки
4. Программа остановится на точке останова

### Шаг 6: Форматирование и проверка качества

```bash
make format-fix  # Автоматическое форматирование кода
make cppcheck    # Статический анализ
```

### Рабочий Цикл TDD (Test-Driven Development)

```bash
# 1. Запустить тесты (красный - провал)
make test

# 2. Написать минимальный код для прохождения тестов
# Отредактировать .c файл

# 3. Запустить тесты (зелёный - успех)
make test

# 4. Рефакторинг и улучшение кода
# 5. Повторить цикл
```

**Совет:** Используйте `make help` для просмотра всех доступных команд!
