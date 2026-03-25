# minGUI
Simple retained mode GUI-library

Максимально простая в использовании GUI-библиотека. Рассчитана в первую очередь на чистоту кода, создающего UI.

1) C++20
2) QT-подобный синтаксис (без сигналов и слотов)
3) Жёсткая логичная архитектура UI
4) Плотная интеграция с любыми системами рендера и ввода(нет собственных систем рендера и ввода)
5) Отсутствуют внешние зависимости, кроме стандартной библиотеки

## В планах
1) Поддержка модулей(C++20)
2) Дальнейшее упрощение архитектуры библиотеки в угоду простоте и логичности
3) Новые классы виджетов

## Реальный пример кода из проекта "Turret_2" (немного упрощён)
```cpp
constexpr Point BTN_SIZE(120.0f, 30.0f);
constexpr Point FORM_SIZE(28.0f, 20.0f);

class FrAudio : public Container {
    Form* master;
    Form* world;
    Form* ui;
    Form* music;
    Checkbox* toggleSound;
public:
    FrAudio(Engine& engine) : Container(Align::centre, Orientation::vertical) {
        auto main = addNode(new Layout(Orientation::horizontal));

        auto clickable = main->addNode(new Layout(Orientation::vertical));
        master = clickable->addNode(new Form(Settings::audio.master, new Uint8Validator(0U, 100U), FORM_SIZE));
        world  = clickable->addNode(new Form(Settings::audio.world,  new Uint8Validator(0U, 100U), FORM_SIZE));
        ui     = clickable->addNode(new Form(Settings::audio.ui,     new Uint8Validator(0U, 100U), FORM_SIZE));
        music  = clickable->addNode(new Form(Settings::audio.music,  new Uint8Validator(0U, 100U), FORM_SIZE));
        toggleSound = clickable->addNode(new Checkbox(Settings::audio.toggleSound));

        auto labels = main->addNode(new Layout(Orientation::vertical));
        labels->addNode(new Label("master"));
        labels->addNode(new Label("world"));
        labels->addNode(new Label("ui"));
        labels->addNode(new Label("music"));
        labels->addNode(new Label("toggle sound"));

        auto lower = addNode(new Layout(Orientation::horizontal));
        lower->addNode(new Button(BTN_SIZE, "Back"))->addCallback([&] { close(); });
        lower->addNode(new Button(BTN_SIZE, "Apply"))->addCallback([&] { applySettings(engine); });
    }

    void applySettings(Engine& engine) {
        using T = decltype(Settings::audio.master);
        Settings::audio.master = validator::to<T>(master->getText()).value_or(50U);
        Settings::audio.world  = validator::to<T>(world ->getText()).value_or(100U);
        Settings::audio.ui     = validator::to<T>(ui    ->getText()).value_or(100U);
        Settings::audio.music  = validator::to<T>(music ->getText()).value_or(100U);
        Settings::audio.toggleSound = toggleSound->getValue();
        Settings::applySettings(engine);
        Settings::writeSettings();
    }
};
```
Как это выглядит на в запущенной игре:

<img width="876" alt="exampleVisual" src="https://github.com/ChancellorIkseew/minGUI/blob/main/dev/fr_audio.png">

