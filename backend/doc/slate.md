# Slate

> Красивая статичная документация API


## Установка в проект
  
  * Копируем к себе и устанавливаем: 

  ```shell
  git clone https://github.com/tripit/slate.git
  cd slate
  bundle install
  ```

  Если не ставится libv8 

    ```shell
    gem install libv8 -v '{{version}}' -- --with-system-v8
    ```

    Если после этого не устанавливается therubyracer
    ```shell
    gem uninstall libv8
    bundle update libv8
    ```

  * Перемещаем в свой проект

  ```shell
  mv {{path_to_slate}} {{path_to_project}}
  rm -rf {{path_to_project}}/slate/.git
  ```

## Настройка

  Добавляем в Gemfile

  ```
    # Live-reloading plugin
    gem 'middleman-livereload', '~> 3.3.0'

    # Deploy plugin
    gem 'middleman-deploy', '~> 1.0'
  ```

  Добавляем в config.rb

  ```ruby
    # Deployment settings
    activate :deploy do |deploy|
      deploy.method = :sftp
      deploy.host = '{{host}}'
      deploy.user = '{{user}}'
      deploy.port = 22
      deploy.path = '{{path}}'
      deploy.build_before = true
    end

  ```

  Запуск из папки slate
  ```
    bundle exec middleman server
  ```

  Если возникает ошибка invalid byte sequence in UTF-8

  ```
  bundle update redcarpet
  ```

  Наш главный файл source/index.md в нем мы описываем базовые вещи и подключаем шаблоны.

  ### Обозначения

  * Раздел #
  * Правая часть странцы >


  ### Deploy

  ```shell
    bundle exec middleman deploy
  ```
