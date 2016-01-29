# Slate

> Красивая статичная документация API


## Установка в проект
  
* Копируем к себе и устанавливаем: 

```bash
git clone https://github.com/tripit/slate.git
cd slate
bundle install
```

Если не ставится libv8 

```bash
gem install libv8 -v '{{version}}' -- --with-system-v8
```

Если после этого не устанавливается therubyracer
```bash
gem uninstall libv8
bundle update libv8
```

* Перемещаем в свой проект

```bash
mv {{path_to_slate}} {{path_to_project}}
rm -rf {{path_to_project}}/slate/.git
```

## Настройка

Все действия производим в {{path_to_project}}/slate

Добавляем в Gemfile

```
# Live-reloading plugin
gem 'middleman-livereload', '~> 3.3.0'

# Deploy plugin
gem 'middleman-deploy', '~> 1.0'
```

```bash
bundle install
```

Добавляем в config.rb

```ruby
# Deployment settings
activate :deploy do |deploy|
  deploy.method = :sftp
  deploy.host = '{{host}}'
  deploy.user = '{{user}}'
  deploy.port = 22
  deploy.path = '{{path_to_remote_project}}/ss/shared/public/wiki'
  deploy.build_before = true
end

# Build Configuration
configure :build do
  activate :minify_css
  activate :minify_javascript
  set :layout, 'build'
end
```

Добавляем source/layouts/build.erb

```erb
<%#
Copyright 2008-2013 Concur Technologies, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
License for the specific language governing permissions and limitations
under the License.
%>
<% language_tabs = current_page.data.language_tabs %>
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta content="IE=edge,chrome=1" http-equiv="X-UA-Compatible">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <title><%= current_page.data.title || "API Documentation" %></title>

    <link href="wiki/stylesheets/screen.css" rel="stylesheet" type="text/css" media="screen" />
    <link href="wiki/stylesheets/print.css" rel="stylesheet" type="text/css" media="print" />
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
    <% if current_page.data.search %>
      <script src="wiki/javascripts/all.js" type="text/javascript"></script>
    <% else %>
      <script src="wiki/javascripts/all_nosearch.js" type="text/javascript"></script>
    <% end %>

    <% if language_tabs %>
      <script>
        $(function() {
          setupLanguages(<%= language_tabs.map{ |lang| lang.is_a?(Hash) ? lang.keys.first : lang }.to_json %>);
        });
      </script>
    <% end %>
  </head>

  <body class="<%= page_classes %>">
    <a href="#" id="nav-button">
      <span>
        NAV
        <img src="wiki/images/navbar.png" />
      </span>
    </a>
    <div class="tocify-wrapper">
      <img src="wiki/images/logo.png" />
      <% if language_tabs %>
        <div class="lang-selector">
          <% language_tabs.each do |lang| %>
            <% if lang.is_a? Hash %>
              <a href="#" data-language-name="<%= lang.keys.first %>"><%= lang.values.first %></a>
            <% else %>
              <a href="#" data-language-name="<%= lang %>"><%= lang %></a>
            <% end %>
          <% end %>
        </div>
      <% end %>
      <% if current_page.data.search %>
        <div class="search">
          <input type="text" class="search" id="input-search" placeholder="Search">
        </div>
        <ul class="search-results"></ul>
      <% end %>
      <div id="toc">
      </div>
      <% if current_page.data.toc_footers %>
        <ul class="toc-footer">
          <% current_page.data.toc_footers.each do |footer| %>
            <li><%= footer %></li>
          <% end %>
        </ul>
      <% end %>
    </div>
    <div class="page-wrapper">
      <div class="dark-box"></div>
      <div class="content">
        <%= yield %>
        <% current_page.data.includes && current_page.data.includes.each do |include| %>
          <%= partial "includes/#{include}" %>
        <% end %>
      </div>
      <div class="dark-box">
        <% if language_tabs %>
          <div class="lang-selector">
            <% language_tabs.each do |lang| %>
              <% if lang.is_a? Hash %>
                <a href="#" data-language-name="<%= lang.keys.first %>"><%= lang.values.first %></a>
              <% else %>
                <a href="#" data-language-name="<%= lang %>"><%= lang %></a>
              <% end %>
            <% end %>
          </div>
        <% end %>
      </div>
    </div>
  </body>
</html>
```

```bash
bundle exec middleman server
```

Если возникает ошибка invalid byte sequence in UTF-8 в Gemfile ставим версию `redcarpet` 3.2.1

```
gem 'redcarpet', '~> 3.2.1'
```

Наш главный файл source/index.md в нем мы описываем базовые вещи и подключаем шаблоны.

Deploy:

```bash
bundle exec middleman deploy
```

Настроить location на папку в которую вы задеплоили документацию.

Пример:

```nginx
location @frontend-shared {
  root /var/www/{{application_name}}/cs/shared/public;
  try_files $uri $uri/index.html @backend-static @backend-shared;
}

location @backend-shared  {
  root /var/www/{{application_name}}/ss/shared/public;
  try_files $uri $uri/index.html;
}
```

DONE!
