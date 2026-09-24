# Лабораторная работа №3: Изучение систем автоматизации сборки проекта на примере CMake

**Студент:** Литошенко Григорий

**GitHub Username:** blyov1301

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

## Структура проекта:

formatter_lib — статическая библиотека для форматирования строк. Содержит локальный CMakeLists.txt для сборки цели formatter.

formatter_ex_lib — расширенная библиотека, зависящая от formatter. Содержит локальный CMakeLists.txt.

solver_lib — библиотека для решения уравнений. Содержит локальный CMakeLists.txt для сборки цели solver.

hello_world_application — приложение, использующее библиотеку formatter.

solver_application — приложение для решения уравнений, использующее библиотеки formatter и solver.

## Создание CMakeLists.txt под каждое задание

Клонирую исходники и подключаю свой репозиторий:

```bash
export GITHUB_USERNAME=ваш_username
cd ~/workspace

git clone https://github.com/tp-labs/lab03.git formatter_project
cd formatter_project

git remote remove origin
git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```
**formatter_lib/CMakeLists.txt:**

```bash
cd formatter_lib
cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.16)
project(formatter LANGUAGES CXX)

add_library(formatter STATIC formatter.cpp)
target_include_directories(formatter PUBLIC \${CMAKE_CURRENT_SOURCE_DIR})
target_compile_features(formatter PUBLIC cxx_std_11)
EOF
```
***formatter_ex_lib/CMakeLists.txt:***

```bash
cd ~/workspace/formatter_project/formatter_ex_lib

cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.16)
project(formatter_ex LANGUAGES CXX)

add_subdirectory(\${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter)

add_library(formatter_ex STATIC formatter_ex.cpp)
target_include_directories(formatter_ex PUBLIC \${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(formatter_ex PUBLIC formatter)
target_compile_features(formatter_ex PUBLIC cxx_std_11)
EOF
```
***hello_world_application/CMakeLists.txt:***

```bash
cd ../hello_world_application
cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.16)
project(hello_world LANGUAGES CXX)

add_subdirectory(\${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex)

add_executable(hello_world hello_world.cpp)
target_link_libraries(hello_world PRIVATE formatter_ex)
target_compile_features(hello_world PRIVATE cxx_std_11)
EOF
```
***solver_lib/CMakeLists.txt:***
```bash
cd ../solver_lib
cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.16)
project(solver_lib LANGUAGES CXX)

add_library(solver_lib STATIC solver.cpp)
target_include_directories(solver_lib PUBLIC \${CMAKE_CURRENT_SOURCE_DIR})
target_compile_features(solver_lib PUBLIC cxx_std_11)
EOF
```
***solver_application/CMakeLists.txt:***

```bash
cd ../solver_application
cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.16)
project(solver LANGUAGES CXX)

add_subdirectory(\${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex)
add_subdirectory(\${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib solver_lib)

add_executable(solver equation.cpp)
target_link_libraries(solver PRIVATE formatter_ex solver_lib)
target_compile_features(solver PRIVATE cxx_std_11)
EOF
```
## Быстрая проверка сборки (выводы)

```bash
# formatter
cd ~/workspace/formatter_project/formatter_lib
cmake -S . -B _build && cmake --build _build
```
*-- The CXX compiler identification is GNU 14.2.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.6s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vboxuser/workspace/formatter_project/formatter_lib/_build
[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter*

```bash
# formatter_ex
cd ../formatter_ex_lib
cmake -S . -B _build && cmake --build _build
```
*-- The CXX compiler identification is GNU 14.2.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vboxuser/workspace/formatter_project/formatter_ex_lib/_build
[ 25%] Building CXX object formatter/CMakeFiles/formatter.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter.a
[ 50%] Built target formatter
[ 75%] Building CXX object CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex.a
[100%] Built target formatter_ex*

```bash
# hello_world
cd ../hello_world_application
cmake -S . -B _build && cmake --build _build
```
*-- The CXX compiler identification is GNU 14.2.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vboxuser/workspace/formatter_project/hello_world_application/_build
[ 16%] Building CXX object formatter_ex/formatter/CMakeFiles/formatter.dir/formatter.cpp.o
[ 33%] Linking CXX static library libformatter.a
[ 33%] Built target formatter
[ 50%] Building CXX object formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 66%] Linking CXX static library libformatter_ex.a
[ 66%] Built target formatter_ex
[ 83%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world*
```bash
./_build/hello_world
```
-------------------------
hello, world!
-------------------------

```bash
# solver
cd ../solver_application
cmake -S . -B _build && cmake --build _build
```
*-- The CXX compiler identification is GNU 14.2.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vboxuser/workspace/formatter_project/solver_application/_build
[ 12%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 25%] Linking CXX static library libsolver_lib.a
[ 25%] Built target solver_lib
[ 37%] Building CXX object formatter_ex/formatter/CMakeFiles/formatter.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter.a
[ 50%] Built target formatter
[ 62%] Building CXX object formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 75%] Linking CXX static library libformatter_ex.a
[ 75%] Built target formatter_ex
[ 87%] Building CXX object CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver*

```bash
./_build/solver
```
9 16 25
-------------------------
error: discriminant < 0
-------------------------
## Коммит и push

```bash
cd ~/workspace/formatter_project

git add formatter_lib/CMakeLists.txt \
        formatter_ex_lib/CMakeLists.txt \
        hello_world_application/CMakeLists.txt \
        solver_lib/CMakeLists.txt \
        solver_application/CMakeLists.txt \
        solver_lib/solver.cpp

git commit -m "added CMakeLists.txt and fixed sqrt in solver"
git push -u origin master
```
