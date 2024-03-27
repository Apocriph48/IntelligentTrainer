FROM python:3.10.7


SHELL ["/bin/bash/", "-c"]


#set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1


RUN pip install --upgrade pip
RUN apt update 
# за apt update прописываем && apt -qy install и перечисляем 
# всё что необходимо доставить в linux для нормальной работы приложения

RUN useradd -rms /bin/bash [name] && chmod 777 /opt /run
# Команда useradd создаёт пользователя [name] с домашним каталогом /bin/bash
# и флагами -r, -m, -s 
# -m, --create-home
#            Создать домашний каталог пользователя, если он не существует. Файлы и каталоги,
#            содержащиеся в каталоге шаблонов (который можно указать с помощью параметра the -k
#            option), будут скопированы в домашний каталог.

#            По умолчанию, если этот параметр не указан и не задана переменная CREATE_HOME,
#            домашний каталог не создаётся.
# -r, --system
#            Создать системную учётную запись.
# -s, --shell ОБОЛОЧКА
#            Имя регистрационной оболочки пользователя. По умолчанию это поле пусто, что вызывает
#            выбор регистрационной оболочки по умолчанию согласно значению переменной SHELL из
#            файла /etc/default/useradd, или по умолчанию используется пустая строка.
# По команде cmod:  https://losst.pro/komanda-chmod-linux 
# кратко: Даёт все права всем в указанных каталогах
WORKDIR /[name_of_working_catalog]

RUN mkdir /[name_of_working_catalog]/static && /[name_of_working_catalog]/media 
RUN chown -R [name]:[name] /[name_of_working_catalog] && chmod 755 /[name_of_working_catalog]
#создаём каталоги статики и медиа для Django
#О команде chown:  https://losst.pro/komanda-chown-linux
# кратко: меняет пользователя и группу для каталога и всех его подкаталогов и файлов
# Кратко по 755 - все для владельца, остальным только чтение и выполнение;

COPY --chown=[name]:[name] . . 
#копирует все файлы из каталога нашего проекта в WORKDIR применяя при этом группу и пользователя, как владельцев
RUN pip install -r requirements.txt # Устанавливает все зависимости указанные в requirements.txt

USER [name] #переключаемся на созданного нами юзера

CMD ["gunicorn", "-b", "0.0.0.0:8001", "soaqaz.wsgi:application"]
#Запускаем gunicorn