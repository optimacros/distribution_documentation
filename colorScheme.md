# Подробное описание Цветовых тем и интерфейсов:

Для установок цветовых тем и интерфейсов используются флаги в файле манифеста:

## Интерфейс:

```
... <= Прочий контент файла манифест
    "console": {
      "commands": {
        "frontend-build": {
          "appConfig": {
            "flags": [ <= Флаги
                "fun_loader_message_off", // отключает "веселые" сообщения при первичной загрузке
                "interface_theme_optimacros", // по умолчанию фиолетовая тема с тайтлом Optimacros и своим фавиконом
                "color_scheme_advexcel", // цветовая схема advexcel доступна для выбора в настройках
                "color_scheme_optimacros", // цветовая схема optimacros доступна для выбора в настройках
                "color_scheme_olapsoft", // цветовая схема olapsoft доступна для выбора в настройках
                "color_scheme_corplan", // цветовая схема corplan доступна для выбора в настройках
                "color_scheme_orange", // цветовая схема orange доступна для выбора в настройках
                "interface_language_RU", // руский язык доступен для выбора в настройках
                "interface_language_EN", // английский язык доступен для выбора в настройках
                "support_email_support@optimacros.com", // email для связи support@optimacros.com
            ]
          }
        }
      }
    }
  }
}

```

На данном примере, первой установлен интерфейс optimacros, с помощью свойства `interface_theme_optimacros`

Цветовых тем на данный момент существует всего три: Optimacros, Olapsoft, Advexcel. Устанавливаются они с помощью 
добавления свойств: interface_theme_optimacros, interface_theme_olapsoft, interface_theme_advexcel соответственно.
Сама по себе цветовая тема изменяет основной логотип приложения, допустим при выборе моделей в драйвлендинге можно 
увидеть логотип, меняется фавикон на табах в браузере и копирайты (в подвале) в самом низу приложения, в котором открыта
 модель.
 
  
Логотип моделей в драйвлендинге:

 ![](./pictures/modelsLogo.jpg)


------------------
Фавикон на табе:

 ![](./pictures/tabsLogo.jpg)


------------------ 
Копирайты в подвале:

 ![](./pictures/footerContent.jpg)
 
## Цветовые схемы:

```
... <= Прочий контент файла манифест
    "console": {
      "commands": {
        "frontend-build": {
          "appConfig": {
            "flags": [ <= Флаги
                "fun_loader_message_off", // отключает "веселые" сообщения при первичной загрузке
                "interface_theme_optimacros", // по умолчанию фиолетовая тема с тайтлом Optimacros и своим фавиконом
                "color_scheme_advexcel", // цветовая схема advexcel доступна для выбора в настройках
                "color_scheme_optimacros", // цветовая схема optimacros доступна для выбора в настройках
                "color_scheme_olapsoft", // цветовая схема olapsoft доступна для выбора в настройках
                "color_scheme_corplan", // цветовая схема corplan доступна для выбора в настройках
                "color_scheme_orange", // цветовая схема orange доступна для выбора в настройках
                "interface_language_RU", // руский язык доступен для выбора в настройках
                "interface_language_EN", // английский язык доступен для выбора в настройках
                "support_email_support@optimacros.com", // email для связи support@optimacros.com
            ]
          }
        }
      }
    }
  }
}

```

На данном примере, установлены цветовые схемы advexcel, optimacros, olapsoft, corplan. Добавляются они с помощью 
добавления в манифест файле свойств в разделе флагов: color_scheme_advexcel, color_scheme_optimacros, 
color_scheme_olapsoft, color_scheme_corplan.

Всего на момент апдейта данного раздела мануала (30.12.2021) в системе существует 10.
Ниже представлены все темы с названиями и скриншотами того как они выглядят в системе:

### color_scheme_advexcel:

![](./pictures/advexcel.jpg)

------------------
### color_scheme_optimacros

![](./pictures/optimacros.jpg)

------------------
### color_scheme_olapsoft
![](./pictures/olapsoft.jpg)

------------------
### color_scheme_corplan
![](./pictures/corplan.jpg)

------------------
### color_scheme_ovk
![](./pictures/ovk.jpg)

------------------
### color_scheme_tvel
![](./pictures/tvel.jpg)

------------------
### color_scheme_orange
![](./pictures/orange.jpg)

------------------
### color_scheme_dark
![](./pictures/dark.jpg)

------------------
### color_scheme_domrf
![](./pictures/domrf.jpg)

------------------
### color_scheme_fixprice
![](./pictures/fixprice.jpg)

------------------

[Вернуться к содержанию <](contents.md)

[Вернуться к оглавлению <<](index.md)