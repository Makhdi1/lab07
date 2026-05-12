# Домашнее задание: Создание hunter-пакета

## Цель
Создать собственный hunter-пакет для библиотеки print.

## Выполнение

### 1. Создание структуры пакета
Создана директория для hunter-пакета:
```bash
mkdir -p $HOME/projects/hunter/cmake/projects/print
```

### 2. Создание hunter.cmake
Создан файл `$HOME/projects/hunter/cmake/projects/print/hunter.cmake`:
```cmake
include(hunter_add_version)
include(hunter_download)
include(hunter_pick_scheme)

hunter_add_version(
    PACKAGE_NAME print
    VERSION "0.1.0.0"
    URL "https://github.com/Makhdi1/lab07/archive/refs/heads/main.tar.gz"
    SHA1 "e166fb7ac01d5a2bf07fa46abe9f9fdf78f88548"
)

hunter_pick_scheme(DEFAULT url_sha1_cmake)
hunter_download(PACKAGE_NAME print)
```

### 3. Создание CMakeLists.txt для пакета
Создан файл `$HOME/projects/hunter/cmake/projects/print/CMakeLists.txt`:
```cmake
cmake_minimum_required(VERSION 3.0)

project(print VERSION 0.1.0.0)

add_library(print STATIC sources/print.cpp)

target_include_directories(print PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
```

### 4. Настройка конфигурации проекта
Обновлен `cmake/Hunter/config.cmake`:
```cmake
hunter_config(GTest VERSION 1.7.0-hunter-9)
hunter_config(print VERSION 0.1.0.0)
```

### 5. Добавление зависимости в проект
В CMakeLists.txt добавлено после `project(print)`:
```cmake
hunter_add_package(print)
```

### 6. Сборка и проверка
```bash
rm -rf _builds
cmake -H. -B_builds -DBUILD_TESTS=ON
cmake --build _builds
```

### Результат
- Hunter-пакет `print` версии `0.1.0.0` успешно создан
- Пакет скачивается из GitHub репозитория lab07
- Пакет собирается в Release и Debug конфигурациях
- Устанавливается в `$HUNTER_ROOT/_Base/.../Install/`
- Включает библиотеку libprint.a, заголовочные файлы и CMake конфигурацию

### Структура hunter-пакета
```
$HOME/projects/hunter/cmake/projects/print/
├── hunter.cmake       # скрипт загрузки пакета
└── CMakeLists.txt     # скрипт сборки пакета
```

### Использование в других проектах
Для использования пакета print в другом проекте достаточно:
1. Подключить HunterGate
2. Добавить в CMakeLists.txt:
```cmake
hunter_add_package(print)
find_package(print CONFIG REQUIRED)
target_link_libraries(myapp print)
```

