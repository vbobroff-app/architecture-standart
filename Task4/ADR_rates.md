### Архитектурное решение (ADR) для передачи депозитных ставок в кол-центры

##### Контекст
После запуска MVP открытия депозитов онлайн ожидается:
- Рост звонков в кол-центры на 200% 
- Необходимость консультирования клиентов по актуальным ставкам
- Ограничения партнерского кол-центра (только файловый обмен)


### <a name="_b7urdng99y53"></a>**Название задачи:** 
**Интеграция систем банка для предоставления актуальных депозитных ставок внутреннему и партнерскому кол-центрам** 
### <a name="_hjk0fkfyohdk"></a>**Автор:**
**Разработчик IT отлела**
### <a name="_uanumrh8zrui"></a>**Дата:**
**21.04.2005**

---
### Use Cases
| №  | Действующее лицо           | Use Case                          | Триггер               | Основной поток                          |
|----|----------------------------|-----------------------------------|-----------------------|-----------------------------------------|
| 1  | Оператор кол-центра        | Консультация клиента по ставкам   | Входящий звонок       | 1. Запрос ставок в CRM <br>2. Отображение актуальных данных |
| 2  | Администратор АБС         | Обновление депозитных ставок      | Изменение ставок ЦБ   | 1. Корректировка в АБС <br>2. Запуск выгрузки |
| 3  | Партнерский кол-центр     | Получение файла со ставками       | По расписанию         | 1. Автозагрузка CSV <br>2. Валидация данных |
---
### Функциональные требования
#### Для банковских систем
| ID  | Требование                                                                 | Приоритет |
|-----|---------------------------------------------------------------------------|-----------|
| F1  | Ежедневная выгрузка ставок в CSV с интервалом 15 минут                    | High      |
| F2  | Шифрование файлов PGP (4096-bit)                                          | Critical  |
| F3  | Валидация данных перед выгрузкой (формат, диапазоны значений)             | Medium    |

#### Для кол-центров
| ID  | Требование                                                                 | Приоритет |
|-----|---------------------------------------------------------------------------|-----------|
| F4  | Автоматическая загрузка CSV в CRM при получении файла                     | High      |
| F5  | Визуальное выделение измененных ставок в интерфейсе оператора             | Low       |


---
### Нефункциональные требования
| Категория         | Требование                                                                 | Метрика               |
|-------------------|---------------------------------------------------------------------------|-----------------------|
| Производительность| Задержка между изменением в АБС и обновлением в CRM ≤15 мин               | 95-й перцентиль       |
| Безопасность      | Шифрование файлов при передаче и хранении                                 | AES-256 + PGP         |
| Доступность       | Доступность SFTP-сервера для партнера ≥99.5%                              | SLA за месяц          |
| Совместимость     | Поддержка формата CSV RFC 4180                                            | 100% валидных файлов  |


**Диаграммы C4:**  

[Диаграмма контекста](https://www.plantuml.com/plantuml/png/bLLDR-9M5DtpArwpIgHXKALsCwjHgLgfwbGez6CvOi2TM38sCaPLTr18CnAjqbQH8cLHIglk7GB1cDZv2_VzevvtZSFZuLIP8Ddny_quzznphrzsWuOVzDfEAR03nn9qGH65T2FxkDxX5mgAAQL4_aGBYkK0z_25e0ajOBAWEONoJ1dDyN0FmoL5C4daM9wAp9fcE8u5pR98wxBy6RO8AFjoN2evrlU123pXsszF0j6oVQiPs9vRAdrWkqsdrx9wlYDEWg3JVL6f-8qVdh-rWvFUcrxNyfkU6rXkyBpfjIl7ThFXjxolRAjZEPNgH-MQqt23RrvzMMatke7bu-XrrMjtF5TvT5J59O6_x5gp_Axd7j8LhIdAq3qJ1_gZe6kKag24y42zkkIo4bLjOMcAaRKy5t883b9mbD22pVbeWC0f8aIqCgX3-ACTI7_YdU8wv3Xzv3QFDibtqczGNd7h1d88BB_0EWBdG_W2SxojJK4H2iAsOx5glu7aDo2zWq-2oDi7YfIJXWQlVgSff_-7xbb8CqM5Iciae8Yvova533dW-mg6MM1Qm6m190ob_YDwsJxCKcGmPep_ZjD7p65V1UAG2KxFz-BpeHv_GzQ_Z7NJqaXmIGFypjI977DCr52WTLheXKL0guETShDcAXzDjnE1bFeh8NzLywK2oxwosXaV1ZZXaKlfce4kyqxE6JN3K7HMwzy-4_8TOeuUkbtxlBOBglxPrxNN816UZ4Gzbv6TuQYhhdUJ73d2lolqT9oh73w9KZe3J5ZhDvGM2xC8o8H2BZfHgJTme4Zz58uv1wmwrJQS5wDXbtVlWwDw_4hd0uz5kdmy6CybiERTkStHQReGbEvEBW-P2SDaWy4OP3m9pXyv2_9yJ_3vG1rRZh7rYXRE9OKyoIbaQcm9USgv_eRx6cuZW64jgiMsp7Oaw_TecsulWS4zzpZDPBXP6tWNex8XxbXaHIC7l--iDsMAL1NiEGAabHpecRKDzh120EUQCHOkAGvC5pWUA0JSGAvkPu7gREQ1_s_ijQ_dtWYhX5zy-abL75BCEffpiHz-B3BeHtf1Z-OmvvjRUCFxuDQO6vMmNgrLi2EU7jhKA8lVsFP62hXn3eqh0wMfWobtyvJLaEJBlM3h7UXV0Yo6V8UgLQYLL0YySE1eicbFcak1IohC-nVstbd-fLa-iBoacAVifUMsUctdFm00) (`rates_mvp_context_diagram.puml`)

[Диаграмма контейнеров](https://www.plantuml.com/plantuml/png/VLDDRzD04BtxLomv1OasAYe2Ughgyg6gXIY5S-KwIsF9iIDd3QeGKjEcj58a2eWJ1weWmTaA2NLIalw5sN_4csm33aInbStipFERdJUpgtNbH2gk-fRob2y5VQAK4lgX3x3-mfi86j28XlgGUjId8VKG6b5dHT0vs_1sAT4d3EnYSuNW1QM03tLB7mlTW9bG1-w1e8vXkrpGRu45gJxGBSlovGibL2WYRw-YHDcBfAkyCB2i6rxW-d5Pnf4lAahLwYk5GkIyMjppL2NUZUioSiD0oK0jkc6rKF9S9og7JpnPatx1Nbue-awWdczj5gfENSa8hXqxhDR2WB-eeMg1nx9utTu7ehgp5iP1sOdsy_I14bGxk2Lo4pDtKxm6rZmsyZfU8AFrtJoz1wGDf35oi9v6ZWivb-wxlAClkW46o3PUUq8Viy2i9duf71cPaEV_B7bgize0fBgnG-s_QD1dOzDC26uuBnsnT9TD-cgo0jAa4Moq5ur8zPcW5CtHBMwyFa831vJCFCRsmsT5CF7V0ceoQR4oVUdnWqrnZotwzczvJMpxTCaIYDlB-kIQ_4r6-pnzvj70-tqo6qpr4UOHRzWvfkjV3z7K675QkxGbW4W4rumCMUvpIb3QeHbWOASTA6KmZvbF0TmfF6TG2_2KTGO44YBJE5L9-dz6OLwxDjRNR4CAmI_6ah3Uz9EwSFJ4qXq1sfI689xmpk6RjCGkCcdn4Hfp2bZJt5DAJ0ioNEQ3cMfpA6S6kOCSV9MJ6JEHOSdAc6L0O2Hu2g3L5P_Q74I_0yMg3CguVBy1)
(`rates_mvp_container_diagram.puml`)

[Диаграмма компонентов](https://www.plantuml.com/plantuml/png/dLLhJnjN4_xkNt5UFe7UEYuLlIHIgu2XkSWKnqlRZzPYR_5AwrrhVJOjhIe18IKLQUabKgkgAaalLJykn2wBmUOlpFb7VUOSBruSaqfTnFfSPfwPUUQoDzlI3cNKz7BIbPuZw0ab5DDpjOdt6VvYGJqQK5zjKvTEg4zTN0tew8QW0rxZj4EnscN13ZRdk3oc1E9zjQSU2hM5PKn7EEu9EZ9eftdr1MG1gZRLNYxdEHz98GCHkkiDAUfkwDIa6_YvtFzSl-P5TISAFT6GijM-KIY4ziVNrbtPYDQYjXFM0bywlhnM2vg5Ybkpmthmdkkq7AzGdCkNFTkNxo-N2asxBPqGHzLYq6m5Fckq4BN0ayjP6xXjLXU2oA_RuSOqVKin8ksz8gRI9OUijl2RK5TWaUYukdIczgPcn6SvCNoASCTsVITSN9kcRw3z321cCONLIcZNmFBizHhlw1SWzHbQlm7yKFFUXvLJcFWIHrd-ifRqSn7CD3qpOknXTMJuPwXrr0CwLOyl8-6oN2fOzqhQZwSKutON-TmMb8pSw6fNnfsuoeLVXrh7UFUGu8GIW4yuHZbnTT29eBEHW-p9ekEr6gxXXWwruY4DOBp7zJL0TNNvKAESP3pvFFTY1i29bZxYP5Kl21hel6RlXFeAZcrnwQfDbdg1ghlsVLlClYd-BwnMwFhhOY48v4koCL--0wtAahKgi6I4eTw7pfeExW2s4ouVeMavvvw21wW4mU4Aay4hgIvQ7rHlEOZHba78UD_hL1bkjZaC0H6sDD-oQms7LPk1tqvFwNUM0Vc9QMQAqqJlO7b2ftnufVNohN9rEQX7dWCeRFBg4O1sBaEn6MweseOeelayHzlzYmN_YqqBKR35KBC7OQrjaD0dYSdJlGZ3x5DjwJScLUVmT5J6ED1vHj9JytJsRsLalRjQhbfEU5yRvrqUwgPwy6PhbTjB9V4MocEbvVYMTVibfM4XO_OwCqT_Z3SxGorz8kTq0D_X5jxXNpTBXYYr9wPNLqjllpRNcBcIiFb8DlJeKG_Kjfcrp3qpPVf7xRCPE9kd7Z2_Hlidsl0J6d3zeodF1NLmjJySsjcnhOc_n5o6l-4yHIZJz9FHf3zJnMgniWmpU0iw71kk0tfEWudMueisN5hIx4vkYb9WrqsD_62-OeZYc2EZNbhxQGVRijRGyVv8Psdh8rxjaVvq8RehOZgWcALDnsNZESzUK6pog7UFWBaF9hEV3WZC6F2AuqsUu6QGt5rSA1eirC6ncHB88lrD7LqOiwyBxcNg0o-5qvhZSs9nTKNatqcPkEFdcqup23U4ePZwy6p7J8QqHZA8Os3PAJ2niTd4tAlNtyYlkL9KhFaCL5Ptr11MiIoYbeSKJWq7owZYCyeN2jc4wxuKhSZpM1Vq7V4Sf2IRsonbuunbSIVbDyLlcvfvYgxQnNtCpN4yAZMs8MRLPe4wkdGV0_-cuzUZflSF) (`rates_mvp_component_diagram.puml`):


* Детализация АБС:
  - Показано разделение на модуль ставок (PL/SQL) и транзакционный модуль (Delphi)

  - Подчеркнуто отсутствие прямого REST API

* Сервис ставок:

  - 4 основных компонента с указанием технологий

* Схема обработки: API → CSV → PGP → SCP

* Безопасность:

  - Указаны алгоритмы шифрования (4096-bit RSA)

  - Отмечено использование HSM/Vault для ключей

* Интеграции:

  - Разные режимы работы с CRM:

  - Банковский колл-центр: автоимпорт

* Партнер: ручная загрузка

Диаграмма покрывает все use cases из ADR и показывает внутреннюю структуру критически важных компонентов.

####  **Обоснования решения**

**Решение с использованием SFTP-сервера и CSV-файлов**

1. Соответствие ограничениям партнерского кол-центра
   * Партнер технически не поддерживает API-интеграцию (REST/SOAP), но имеет инфраструктуру для приема файлов через SFTP.

   * CSV-формат — стандарт де-факто для файлового обмена:

     - Поддерживается всеми CRM-системами (включая legacy-решения партнера).

     - Не требует сложного парсинга (в отличие от XML).

2. Безопасность и контроль данных

| **Аспект**       | **Реализация в SFTP+CSV**                          | **Альтернатива (API)**                     |
|------------------|--------------------------------------------------|-------------------------------------------|
| Шифрование      | End-to-end через PGP + TLS в SFTP                | HTTPS (только транспортное)               |
| Аудит           | Логирование всех загруженных файлов              | Зависит от реализации API                 |
| Контроль версий | Возможность хранить исторические файлы           | Требует отдельного механизма              |


3. Простота внедрения в MVP

   Использование существующих технологий:

   - В банке уже есть SFTP-сервер для обмена с СМС-шлюзом.

   - Поддержка CSV встроена в текущую АБС (экспорт через PL/SQL).

   - Нулевые затраты на лицензии (в отличие от ESB-решений).

4. Гибкость для будущих изменений

   Легкий переход на API:

   - CSV-файлы могут стать fallback-механизмом при сбоях API.

   - Формат файла совместим с будущей DataLake.

   Масштабируемость:

   - Добавление новых кол-центров требует только настройки SFTP-аккаунтов.
  

### **Недостатки решения**

| **Недостаток**              | **Компенсирующие меры**                                |
|-----------------------------|-------------------------------------------------------|
| Задержка до 15 мин          | Для консультаций по ставкам это допустимо              |
| Риск устаревших данных      | Валидация файлов перед отправкой                       |
|Нет механизма подтверждения получения|Мониторинг через checksum-файлы|
| Ручное управление ключами PGP | Автоматизация через HashiCorp Vault (в планах)        |


### **Альтернативы**

| **Вариант**                | **Преимущества**                      | **Недостатки**                                  |
|---------------------------|---------------------------------------|-----------------------------------------------|
| REST API для партнера     | Реальное время обновления             | Не поддерживается партнером (+6 мес задержки) |
| Общее облачное хранилище  | Простота реализации                   | Не соответствует политике безопасности        |
| Прямой доступ к АБС       | Нет задержек                          | Риск перегрузки основной БД. Не соответствует политике безопасности|


#### **Выводы:** 
Решение SFTP+CSV — оптимальный баланс между:

- Скоростью внедрения (2-3 недели vs 2+ месяца для API)

- Безопасностью (PGP + TLS)

- Совместимостью со всеми участниками процесса.