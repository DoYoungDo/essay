# Qt国际化

> 国际化的原理就是将文本显示为指定的语言对应的文本，如：中文下显示`按钮`，英文下显示`Button`
> 
> Qt中默认情况下，可以通过`QTranslator`加载(load)一个`.qm`翻译文件，然后通过`qApp.installTranslator(&translator)`加载翻译文件，
> 
> 使用时直接通过`tr("sourceText")`翻译一遍

```cpp
int main(){
    QApplication a;
    
    QTranslator translator;
    if(translator.load(":/zh.qm"))
    {
        a.installTranslator(&translator)
    }
    
    Widget w;
    w.show();
    
    return a.exec();
}
```

> 由于`qm`文件的格式是`xml`，不太喜欢，想换成`json`格式
> 
> 直接继承`QTranslator`重写国际化文件加载和翻译逻辑
> 
> 翻译需要覆写`virtual QString translate(const char *context, const char *sourceText, const char *disambiguation = nullptr, int n = -1) const`虚函数
> 
> 自定义翻译文件加载可以覆写`load`方法

```json
// 词条 zh.json
{
    "btn": "按钮"
}
```

```json
// 词条 en.json
{
    "btn": "Button"
}
```

```cpp
// Translator.h

#ifndef TRANSLATOR_H
#define TRANSLATOR_H

#include <QLocale>
#include <QMap>
#include <QObject>
#include <QTranslator>

class Translator : public QTranslator
{
    Q_OBJECT
public:
    explicit Translator(QObject *parent = nullptr);

    // QTranslator interface
public:
    virtual QString translate(const char* context, const char* sourceText, const char* disambiguation, int n) const override;
    virtual bool isEmpty() const override;

    QString language() const;

    bool load();
    bool load(QLocale locale);

private:
    QMap<QString,QString> m_words;
    QLocale m_local;
};

#endif // TRANSLATOR_H
```

```cpp
// Translator.cpp

#include "translator.h"

#include <QFile>
#include <QJsonDocument>
#include <QJsonObject>
#include <QDebug>

Translator::Translator(QObject *parent)
    : QTranslator{parent}
    , m_local(QLocale(QLocale::Chinese))
{
    m_words.clear();
}

QString Translator::translate(const char* context, const char* sourceText, const char* disambiguation, int n) const
{
    qDebug() << context << sourceText << disambiguation << n;
    if(m_words.contains(sourceText))
    {
        return m_words.value(sourceText);
    }
    return sourceText;
}

bool Translator::isEmpty() const
{
    return m_words.isEmpty();
}

QString Translator::language() const
{
    return QLocale::languageToString(m_local.language());
}

bool Translator::load()
{
    return load(m_local);
}

bool Translator::load(QLocale locale)
{
    QString language = QLocale::languageToCode(locale.language());
    QString fileName( ":/localized/%1.json");
    QString path = fileName.arg(language);
    m_local = locale;
    if(!QFile::exists(path))
    {
        m_local = QLocale(QLocale::Chinese);
        path = fileName.arg(QLocale::languageToCode(m_local.language()));
        Q_ASSERT(QFile::exists(path));
    }

    QFile file(path);
    Q_ASSERT(file.open(QIODevice::ReadOnly));
    QJsonParseError err;
    QJsonDocument doc = QJsonDocument::fromJson(file.readAll(),&err);
    file.close();

    Q_ASSERT(err.error == QJsonParseError::NoError);;

    QJsonObject words = doc.object();
    for(auto key : words.keys())
    {
        m_words.insert(key,words.value(key).toString());
    }
    return true;
}

```