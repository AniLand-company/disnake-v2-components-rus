"""
═══════════════════════════════════════════════════════════════════════════════
                    DISNAKE V2 COMPONENTS COG
                      Все новые компоненты v2
═══════════════════════════════════════════════════════════════════════════════

Использование в main.py:
    bot.load_extension("cogs.v2_components")

Или если файл назван иначе:
    bot.load_extension("имя_папки.имя_файла")
"""

import disnake
from disnake.ext import commands
from typing import Optional


class V2ComponentsCog(commands.Cog):
    """
    Cog со всеми примерами V2 компонентов disnake
    
    Содержит слеш команды для демонстрации:
    - Section (Секции)
    - TextDisplay (Текстовое отображение)
    - Thumbnail (Миниатюры)
    - MediaGallery (Галереи)
    - Separator (Разделители)
    - Container (Контейнеры)
    - File (Файлы)
    - Комбинированные примеры
    """
    
    def __init__(self, bot: commands.Bot):
        self.bot = bot
    
    
    # ═════════════════════════════════════════════════════════════════════════
    # 1️⃣ SECTION КОМАНДЫ
    # ═════════════════════════════════════════════════════════════════════════
    
    @commands.slash_command(
        name="section_demo",
        description="📌 Демонстрация Section компонента с текстом и миниатюрой"
    )
    async def section_demo(self, inter: disnake.ApplicationCommandInteraction):
        """
        Слеш команда для отображения Section компонента.
        Section содержит текст и аксессуар (Thumbnail).
        """
        
        # Создаем Section с текстом и изображением
        section = disnake.ui.Section(
            # Первая строка текста
            disnake.ui.TextDisplay(
                content="**🎨 Красивый заголовок** - Это Section компонент!"
            ),
            # Вторая строка текста
            disnake.ui.TextDisplay(
                content="Изображение справа →"
            ),
            # Аксессуар (миниатюра)
            accessory=disnake.ui.Thumbnail(
                media="https://cdn.discordapp.com/embed/avatars/0.png",
                description="Пример изображения Discord"
            )
        )
        
        # Отправляем с флагом v2 компонентов
        await inter.response.send_message(
            components=[section],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="section_button",
        description="🔘 Section с кнопкой вместо картинки"
    )
    async def section_button(self, inter: disnake.ApplicationCommandInteraction):
        """
        Section с кнопкой как аксессуаром вместо Thumbnail.
        """
        
        section = disnake.ui.Section(
            disnake.ui.TextDisplay(
                content="**💻 Нажми кнопку справа!**"
            ),
            disnake.ui.TextDisplay(
                content="Это Section компонент с кнопкой"
            ),
            # Используем кнопку как аксессуар
            accessory=disnake.ui.Button(
                label="Кликни",
                style=disnake.ButtonStyle.primary,
                custom_id="demo_section_button",
                emoji="👋"
            )
        )
        
        await inter.response.send_message(
            components=[section],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="section_user",
        description="👤 Карточка профиля пользователя через Section"
    )
    async def section_user(
        self, 
        inter: disnake.ApplicationCommandInteraction,
        user: Optional[disnake.User] = None
    ):
        """
        Section команда для отображения информации пользователя.
        
        Параметры:
            user: Пользователь для отображения (опционально)
        """
        
        user = user or inter.author
        
        # Получаем аватар
        avatar_url = user.avatar.url if user.avatar else user.default_avatar.url
        
        # Создаем профильную карточку
        profile = disnake.ui.Section(
            # Имя пользователя
            disnake.ui.TextDisplay(
                content=f"**{user.name}#{user.discriminator}**"
            ),
            # Информация
            disnake.ui.TextDisplay(
                content=f"**ID:** {user.id}\n"
                        f"**Создан:** {user.created_at.strftime('%d.%m.%Y')}\n"
                        f"**Бот:** {'✅ Да' if user.bot else '❌ Нет'}"
            ),
            # Аватар как миниатюра
            accessory=disnake.ui.Thumbnail(
                media=avatar_url,
                description=f"Аватар {user.name}"
            )
        )
        
        await inter.response.send_message(
            components=[profile],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    # ═════════════════════════════════════════════════════════════════════════
    # 2️⃣ TEXTDISPLAY КОМАНДЫ
    # ═════════════════════════════════════════════════════════════════════════
    
    @commands.slash_command(
        name="text_demo",
        description="📝 Демонстрация TextDisplay компонента"
    )
    async def text_demo(self, inter: disnake.ApplicationCommandInteraction):
        """
        Команда для демонстрации TextDisplay компонента.
        Показывает поддержку markdown форматирования.
        """
        
        # Создаем TextDisplay с markdown форматированием
        text = disnake.ui.TextDisplay(
            content="""# 🎉 Добро пожаловать!

Это **TextDisplay** компонент. Он поддерживает markdown:

**✅ Жирный текст** - **жирный**
*✅ Курсив* - *курсив*
__✅ Подчеркнутый__ - __подчеркнутый__
~~✅ Зачеркнутый~~ - ~~зачеркнутый~~

## Заголовки работают
### И это подзаголовок

> Цитаты тоже работают!

```python
# Даже код подсвечивается!
print("Hello, World!")
```

• Списки маркерами
• Второй пункт
• Третий пункт

Можешь использовать 🎨 эмодзи! 🚀"""
        )
        
        await inter.response.send_message(
            components=[text],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="text_formatted",
        description="✨ TextDisplay с красивым форматированием"
    )
    async def text_formatted(self, inter: disnake.ApplicationCommandInteraction):
        """
        Команда с красиво отформатированным текстом.
        """
        
        text = disnake.ui.TextDisplay(
            content="""# 📚 Информация о боте

## Основные команды:
- `/section_demo` - Демо Section компонента
- `/gallery` - Галерея изображений
- `/profile` - Информация профиля

## Возможности:
✅ Поддержка v2 компонентов
✅ Красивое отображение информации
✅ Markdown форматирование
✅ Эмодзи поддержка

**Спасибо за использование! 🙏**"""
        )
        
        await inter.response.send_message(
            components=[text],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    # ═════════════════════════════════════════════════════════════════════════
    # 3️⃣ THUMBNAIL КОМАНДЫ
    # ═════════════════════════════════════════════════════════════════════════
    
    @commands.slash_command(
        name="thumbnail",
        description="🖼️ Демонстрация Thumbnail компонента"
    )
    async def thumbnail(self, inter: disnake.ApplicationCommandInteraction):
        """
        Команда для отображения одного изображения через Thumbnail.
        """
        
        # Создаем Thumbnail
        thumb = disnake.ui.Thumbnail(
            media="https://cdn.discordapp.com/embed/avatars/0.png",
            description="Пример изображения - Thumbnail компонент",
            spoiler=False
        )
        
        await inter.response.send_message(
            components=[thumb],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="thumbnail_spoiler",
        description="🔒 Thumbnail со спойлером"
    )
    async def thumbnail_spoiler(self, inter: disnake.ApplicationCommandInteraction):
        """
        Thumbnail, скрытая спойлером. Пользователь должен кликнуть для раскрытия.
        """
        
        thumb = disnake.ui.Thumbnail(
            media="https://cdn.discordapp.com/embed/avatars/0.png",
            description="Это спойлер! Кликни чтобы раскрыть 👀",
            spoiler=True  # Скрыто спойлером
        )
        
        await inter.response.send_message(
            components=[thumb],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    # ═════════════════════════════════════════════════════════════════════════
    # 4️⃣ MEDIAGALLERY КОМАНДЫ
    # ═════════════════════════════════════════════════════════════════════════
    
    @commands.slash_command(
        name="gallery",
        description="🖼️ Демонстрация MediaGallery - галереи изображений"
    )
    async def gallery(self, inter: disnake.ApplicationCommandInteraction):
        """
        Команда для отображения галереи из нескольких изображений.
        Максимум 10 изображений в одной галерее.
        """
        
        # Создаем галерею с несколькими изображениями
        gallery = disnake.ui.MediaGallery(
            disnake.ui.MediaGalleryItem(
                media="https://cdn.discordapp.com/embed/avatars/0.png",
                description="Первое изображение в галерее"
            ),
            disnake.ui.MediaGalleryItem(
                media="https://cdn.discordapp.com/embed/avatars/1.png",
                description="Второе изображение"
            ),
            disnake.ui.MediaGalleryItem(
                media="https://cdn.discordapp.com/embed/avatars/2.png",
                description="Третье изображение"
            ),
            disnake.ui.MediaGalleryItem(
                media="https://cdn.discordapp.com/embed/avatars/3.png",
                description="Четвертое изображение"
            ),
        )
        
        await inter.response.send_message(
            components=[gallery],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="gallery_spoiler",
        description="🔒 Галерея со спойлерами"
    )
    async def gallery_spoiler(self, inter: disnake.ApplicationCommandInteraction):
        """
        Галерея, где все изображения скрыты спойлерами.
        """
        
        # Создаем список элементов галереи
        items = [
            disnake.ui.MediaGalleryItem(
                media=f"https://cdn.discordapp.com/embed/avatars/{i}.png",
                description=f"Спойлер {i+1}",
                spoiler=True  # Все изображения скрыты
            )
            for i in range(5)
        ]
        
        gallery = disnake.ui.MediaGallery(*items)
        
        await inter.response.send_message(
            components=[gallery],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    # ═════════════════════════════════════════════════════════════════════════
    # 5️⃣ SEPARATOR КОМАНДЫ
    # ═════════════════════════════════════════════════════════════════════════
    
    @commands.slash_command(
        name="separator",
        description="📏 Демонстрация Separator - разделитель"
    )
    async def separator(self, inter: disnake.ApplicationCommandInteraction):
        """
        Команда для демонстрации Separator компонента (видимая линия).
        """
        
        # Первая секция
        section1 = disnake.ui.Section(
            disnake.ui.TextDisplay(content="**📍 Раздел 1**"),
            disnake.ui.TextDisplay(content="Содержимое первого раздела")
        )
        
        # Разделитель
        separator = disnake.ui.Separator(
            divider=True,  # Видимая линия
            spacing=disnake.SeparatorSpacing.medium
        )
        
        # Вторая секция
        section2 = disnake.ui.Section(
            disnake.ui.TextDisplay(content="**📍 Раздел 2**"),
            disnake.ui.TextDisplay(content="Содержимое второго раздела")
        )
        
        await inter.response.send_message(
            components=[section1, separator, section2],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="spacing",
        description="⬜ Separator для добавления пространства"
    )
    async def spacing(self, inter: disnake.ApplicationCommandInteraction):
        """
        Команда для демонстрации Separator как пространство между элементами.
        """
        
        # Текст 1
        text1 = disnake.ui.TextDisplay(
            content="**Текст 1** - находится в начале"
        )
        
        # Пространство
        space = disnake.ui.Separator(
            divider=False,  # Невидимый разделитель
            spacing=disnake.SeparatorSpacing.extra_large  # Большой отступ
        )
        
        # Текст 2
        text2 = disnake.ui.TextDisplay(
            content="**Текст 2** - После большого пространства"
        )
        
        await inter.response.send_message(
            components=[text1, space, text2],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    # ═════════════════════════════════════════════════════════════════════════
    # 6️⃣ CONTAINER КОМАНДЫ
    # ═════════════════════════════════════════════════════════════════════════
    
    @commands.slash_command(
        name="container",
        description="📦 Демонстрация Container компонента"
    )
    async def container(self, inter: disnake.ApplicationCommandInteraction):
        """
        Команда для демонстрации Container компонента.
        Container - это визуально отделенный блок с опциональным цветом.
        """
        
        # Создаем контейнер с цветом
        container = disnake.ui.Container(
            disnake.ui.TextDisplay(content="**📦 Это контейнер!**"),
            disnake.ui.TextDisplay(
                content="Container имеет **синий** акцентный цвет слева"
            ),
            disnake.ui.TextDisplay(
                content="Можно добавлять несколько TextDisplay элементов"
            ),
            accent_colour=disnake.Color.blue()  # Синий цвет слева
        )
        
        await inter.response.send_message(
            components=[container],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="containers_multiple",
        description="🎨 Несколько контейнеров с разными цветами"
    )
    async def containers_multiple(self, inter: disnake.ApplicationCommandInteraction):
        """
        Команда для демонстрации нескольких контейнеров с разными цветами.
        """
        
        # Синий контейнер
        container1 = disnake.ui.Container(
            disnake.ui.TextDisplay(content="**🔵 Синий контейнер**"),
            disnake.ui.TextDisplay(content="Цвет: blue"),
            accent_colour=disnake.Color.blue()
        )
        
        # Зеленый контейнер
        container2 = disnake.ui.Container(
            disnake.ui.TextDisplay(content="**🟢 Зеленый контейнер**"),
            disnake.ui.TextDisplay(content="Цвет: green"),
            accent_colour=disnake.Color.green()
        )
        
        # Красный контейнер
        container3 = disnake.ui.Container(
            disnake.ui.TextDisplay(content="**🔴 Красный контейнер**"),
            disnake.ui.TextDisplay(content="Цвет: red"),
            accent_colour=disnake.Color.red()
        )
        
        # Золотой контейнер
        container4 = disnake.ui.Container(
            disnake.ui.TextDisplay(content="**🟡 Золотой контейнер**"),
            disnake.ui.TextDisplay(content="Цвет: gold"),
            accent_colour=disnake.Color.gold()
        )
        
        await inter.response.send_message(
            components=[container1, container2, container3, container4],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="nested_containers",
        description="🎁 Вложенные контейнеры"
    )
    async def nested_containers(self, inter: disnake.ApplicationCommandInteraction):
        """
        Команда для демонстрации вложенных контейнеров.
        """
        
        # Внутренний контейнер
        inner_container = disnake.ui.Container(
            disnake.ui.TextDisplay(content="**Внутренний контейнер**"),
            disnake.ui.TextDisplay(content="Вложен в основной"),
            accent_colour=disnake.Color.green()
        )
        
        # Внешний контейнер
        outer_container = disnake.ui.Container(
            disnake.ui.TextDisplay(content="**🎁 Основной контейнер**"),
            disnake.ui.TextDisplay(content="Содержит вложенный контейнер:"),
            inner_container,  # Вложенный контейнер
            accent_colour=disnake.Color.blue()
        )
        
        await inter.response.send_message(
            components=[outer_container],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    # ═════════════════════════════════════════════════════════════════════════
    # 7️⃣ КОМБИНИРОВАННЫЕ ПРИМЕРЫ
    # ═════════════════════════════════════════════════════════════════════════
    
    @commands.slash_command(
        name="article",
        description="📰 Пример: Статья с иллюстрацией"
    )
    async def article(self, inter: disnake.ApplicationCommandInteraction):
        """
        Комбинированный пример - красивая статья с иллюстрацией.
        Использует Section, TextDisplay, Thumbnail, Separator и Container.
        """
        
        # Заголовок
        header = disnake.ui.TextDisplay(
            content="# 📰 Интересная статья"
        )
        
        # Основная статья с картинкой
        article_section = disnake.ui.Section(
            disnake.ui.TextDisplay(
                content="""Это начало **замечательной** статьи с интересным контентом.

Статья содержит:
• Важную информацию
• Красивое форматирование
• Иллюстрации и примеры

Спасибо за внимание!"""
            ),
            accessory=disnake.ui.Thumbnail(
                media="https://cdn.discordapp.com/embed/avatars/0.png",
                description="Иллюстрация к статье"
            )
        )
        
        # Разделитель
        separator = disnake.ui.Separator()
        
        # Информация об авторе
        author_info = disnake.ui.Container(
            disnake.ui.TextDisplay(content="### ✍️ Об авторе"),
            disnake.ui.TextDisplay(
                content="Автор: Бот Discord\nДата: 15.09.2024\nКатегория: Примеры"
            ),
            accent_colour=disnake.Color.gold()
        )
        
        await inter.response.send_message(
            components=[header, article_section, separator, author_info],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="portfolio",
        description="🎨 Пример: Портфолио с галереей"
    )
    async def portfolio(self, inter: disnake.ApplicationCommandInteraction):
        """
        Комбинированный пример - портфолио с галереей работ.
        """
        
        # Заголовок портфолио
        title = disnake.ui.Section(
            disnake.ui.TextDisplay(content="# 🎨 Мое Портфолио")
        )
        
        # Описание
        description = disnake.ui.Container(
            disnake.ui.TextDisplay(
                content="Здесь представлены мои лучшие работы. "
                        "Все картинки в галерее ниже."
            ),
            accent_colour=disnake.Color.purple()
        )
        
        # Галерея работ
        gallery = disnake.ui.MediaGallery(
            *[
                disnake.ui.MediaGalleryItem(
                    media=f"https://cdn.discordapp.com/embed/avatars/{i}.png",
                    description=f"Работа #{i+1}"
                )
                for i in range(4)
            ]
        )
        
        # Разделитель
        separator = disnake.ui.Separator()
        
        # Финальное спасибо
        footer = disnake.ui.Section(
            disnake.ui.TextDisplay(
                content="**Спасибо за просмотр! 🙏**"
            )
        )
        
        await inter.response.send_message(
            components=[title, description, gallery, separator, footer],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="event",
        description="🎉 Пример: Объявление события"
    )
    async def event(self, inter: disnake.ApplicationCommandInteraction):
        """
        Комбинированный пример - красивое объявление о событии.
        """
        
        # Баннер события
        banner = disnake.ui.Section(
            disnake.ui.TextDisplay(
                content="# 🎉 БОЛЬШОЕ СОБЫТИЕ!"
            ),
            accessory=disnake.ui.Thumbnail(
                media="https://cdn.discordapp.com/embed/avatars/0.png",
                description="Банер события"
            )
        )
        
        # Детали события
        details = disnake.ui.Container(
            disnake.ui.TextDisplay(
                content="""**📅 Дата:** 25 Сентября 2024
**⏰ Время:** 19:00 МСК
**📍 Место:** Discord сервер

Не пропусти это событие! Это будет **очень интересно**! 🚀"""
            ),
            accent_colour=disnake.Color.purple()
        )
        
        # Разделитель
        separator = disnake.ui.Separator()
        
        # Приглашение
        invite = disnake.ui.TextDisplay(
            content="**Приготовься! 🎊**\n\n"
                    "Регистрация откроется за 1 час до начала."
        )
        
        await inter.response.send_message(
            components=[banner, details, separator, invite],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    @commands.slash_command(
        name="showcase",
        description="✨ ПОЛНЫЙ ШОУКЕЙС всех v2 компонентов"
    )
    async def showcase(self, inter: disnake.ApplicationCommandInteraction):
        """
        Полный шоукейс всех v2 компонентов в одном сообщении.
        """
        
        # ===== ЗАГОЛОВОК =====
        title = disnake.ui.TextDisplay(
            content="# ✨ Полный Шоукейс V2 Компонентов\n\nДемонстрация всех новых компонентов disnake v2.11.0+"
        )
        
        # ===== SECTION =====
        section_demo = disnake.ui.Section(
            disnake.ui.TextDisplay(content="**📌 Section компонент**"),
            disnake.ui.TextDisplay(content="Текст + аксессуар (картинка или кнопка)"),
            accessory=disnake.ui.Thumbnail(
                media="https://cdn.discordapp.com/embed/avatars/0.png",
                description="Пример Thumbnail"
            )
        )
        
        # ===== SEPARATOR =====
        sep1 = disnake.ui.Separator()
        
        # ===== TEXTDISPLAY =====
        text_demo = disnake.ui.Container(
            disnake.ui.TextDisplay(
                content="**📝 TextDisplay компонент**\n\n"
                        "Поддерживает **жирный**, *курсив*, ~~зачеркнутый~~, `код` и многое другое! 🎨"
            ),
            accent_colour=disnake.Color.blue()
        )
        
        # ===== SEPARATOR =====
        sep2 = disnake.ui.Separator()
        
        # ===== GALLERY =====
        gallery_demo = disnake.ui.Container(
            disnake.ui.TextDisplay(content="**🖼️ MediaGallery компонент**"),
            disnake.ui.MediaGallery(
                *[
                    disnake.ui.MediaGalleryItem(
                        media=f"https://cdn.discordapp.com/embed/avatars/{i}.png",
                        description=f"Изображение {i+1}"
                    )
                    for i in range(3)
                ]
            ),
            accent_colour=disnake.Color.green()
        )
        
        # ===== SEPARATOR =====
        sep3 = disnake.ui.Separator()
        
        # ===== КОНТЕЙНЕРЫ =====
        containers = disnake.ui.Container(
            disnake.ui.TextDisplay(content="**📦 Container компоненты**"),
            disnake.ui.TextDisplay(
                content="Контейнеры могут быть вложенными и иметь разные цвета"
            ),
            accent_colour=disnake.Color.red()
        )
        
        # ===== ФИНАЛЬНОЕ СООБЩЕНИЕ =====
        final = disnake.ui.TextDisplay(
            content="\n✅ **Это все новые v2 компоненты!**\n\n"
                    "Используй слеш команды для отдельных примеров:\n"
                    "• `/section_demo` - Section примеры\n"
                    "• `/gallery` - Галереи\n"
                    "• `/container` - Контейнеры\n"
                    "• `/article` - Комбинированный пример\n"
                    "• И многое другое!"
        )
        
        # Отправляем все компоненты вместе
        await inter.response.send_message(
            components=[
                title,
                section_demo,
                sep1,
                text_demo,
                sep2,
                gallery_demo,
                sep3,
                containers,
                final
            ],
            flags=disnake.MessageFlags.is_components_v2
        )
    
    
    # ═════════════════════════════════════════════════════════════════════════
    # 🔧 СЛУЖЕБНЫЕ МЕТОДЫ
    # ═════════════════════════════════════════════════════════════════════════
    
    def cog_load(self):
        """Вызывается при загрузке cog'а"""
        print("✅ V2ComponentsCog загружен успешно!")
    
    def cog_unload(self):
        """Вызывается при выгрузке cog'а"""
        print("❌ V2ComponentsCog выгружен!")


# ═════════════════════════════════════════════════════════════════════════════
# ФУНКЦИЯ ДЛЯ ЗАГРУЗКИ РАСШИРЕНИЯ
# ═════════════════════════════════════════════════════════════════════════════

def setup(bot: commands.Bot):
    """
    Функция для загрузки расширения.
    
    Использование в main.py:
        bot.load_extension("путь.к.этому.файлу")
        # или
        bot.load_extension("cogs.v2_components")
    """
    bot.add_cog(V2ComponentsCog(bot))
    print("🎉 V2 Components Cog добавлен в бота!")


"""
═════════════════════════════════════════════════════════════════════════════════
                    ИНСТРУКЦИЯ ПО ИСПОЛЬЗОВАНИЮ
═════════════════════════════════════════════════════════════════════════════════

1. СОХРАНИ ЭТОТ ФАЙЛ:
   - Создай папку "cogs" в твоем проекте
   - Сохрани этот файл как "v2_components.py" в папке cogs
   - Создай пустой файл "__init__.py" в папке cogs

2. ЗАГРУЗИ В main.py:
   ```python
   from disnake.ext import commands
   
   bot = commands.Bot(command_prefix="/")
   
   @bot.event
   async def on_ready():
       bot.load_extension("cogs.v2_components")
       print(f"{bot.user} вошел в систему!")
   
   bot.run("YOUR_TOKEN")
   ```

3. ИЛИ ЗАГРУЗИ ПОЗЖЕ:
   ```python
   @bot.slash_command()
   async def load_cog(inter: disnake.ApplicationCommandInteraction):
       bot.load_extension("cogs.v2_components")
       await inter.response.send_message("✅ Cog загружен!")
   ```

4. ИСПОЛЬЗУЙ КОМАНДЫ:
   - Все команды начинаются с "/"
   - Примеры: /section_demo, /gallery, /portfolio, /showcase и т.д.

═════════════════════════════════════════════════════════════════════════════════
"""
