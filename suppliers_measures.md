**ИАФ.1 Идентификация и аутентификация пользователей, являющихся работниками оператора<br>(мера обязательна для К1, К2 и К3)**

***Требования для реализации меры:***

В соответствии с [Методическим документом ФСТЭК "Меры защиты информации в государственных информационных системах" от 11.02.2014](https://fstec.ru/dokumenty/vse-dokumenty/spetsialnye-normativnye-dokumenty/metodicheskij-dokument-ot-11-fevralya-2014-g) для данной меры определяются следующие технические требования:
1. Пользователи должны однозначно идентифицироваться и аутентифицироваться для всех видов доступа, кроме тех, которые определяются как действия, разрешённые до идентификации и авторизации (соответствующая мера: УПД.11);
2. Аутентификация пользователей должна осуществляться с использованием паролей, аппаратных средств, биометрических характеристик, иных средств или в случае многофакторной аутентификации — определённой комбинации указанных средств;
3. В ИС должна обеспечиваться возможность однозначного сопоставления идентификатора пользователя (один идентификатор — один, соответствующий ему пользователь) с запускаемыми от его имени процессами.

Доступ к различным ресурсам для сотрудников оператора осуществляется в соответствии с матрицей доступа. Матрицу доступа задаёт заказчик.

Правила и процедуры идентификации и аутентификации пользователей должны быть описаны в организационно-распорядительных документах по защите информации у заказчика.

| № | Интерфейс | Назначение | Тип интерфейса | Категории | Справка |
|---|-----------|------------|----------------|-----------|---------|
| 1. | /bin/login | Старт интерактивной сессии.<br>Запускается только в сессии без графического интерфейса. | Команда. | Локальные пользователи.<br><br>Сессия без графического интерфейса. | login(1) |
| 2. | /usr/bin/id<br>/usr/bin/groups | Отображает текущий, реальный или эффективный идентификатор и идентификатор группы, в которые входит пользователь. | Команды. | Локальные пользователи.<br><br>Сетевые ресурсы. | id(1)<br>groups(1) |
| 3. | /bin/su<br>/usr/bin/sudo | Смена пользователей и повышение привилегий. | Команды. | Локальные пользователи.<br><br>Механизмы повышения привилегий. | su(1)<br>sudo(8)<br>runas(1) |
| 4. | /etc/shadow<br>/etc/shadow-<br>/etc/tcb/[user]/shadow<br>/etc/gshadow<br>/etc/gshadow- | БД паролей и атрибутов субъектов доступа. В данном случае атрибутом аутентификационной информации являются пароли для учётных записей пользователей. | Конфигурационные файлы. | Локальные пользователи. | shadow(5) |
| 5. | getresuid()<br>getresgid()<br>getuid()<br>geteuid()<br>setuid()<br>setgid()<br>setegid()<br>setregid() | Получение или установка текущего реального или эффективного идентификатора пользователя или группы, в которой состоит пользователь. Обеспечение связывания пользователь-субъект. | Системные вызовы и библиотечные функции. | Локальные пользователи. | getresuid(2)<br>getresgid(2)<br>getuid(2)<br>geteuid(2)<br>setuid(2)<br>setgid(2)<br>setegid(2)<br>setregid(2) |
| 6. | /usr/sbin/passwd | Обеспечение проверки и манипуляций с аутентификационной информацией. | Команда. | Локальные пользователи. | passwd(1, 8) |
| 7. | /etc/pam.d/\*<br>/usr/lib64/security/pam\*.so | Реализация модулей управления типами и политиками аутентификации и файлы конфигурации этих модулей и политик. | Конфигурационные файлы и разделяемые библиотеки. | Локальные пользователи. | pam(5)<br>pam(8)<br>pam_unix(8)<br>pam_tcb(8) |
| 8. | /etc/sssd/sssd.conf<br>/etc/krb5.conf | Настройка подключения к AD/LDAP, Kerberos и GPO. Кэширование учётных данных, авторизация через GPO/LDAP/HBAC. | Конфигурационные файлы. | Доменные пользователи.<br><br>SSSD. | sssd(8)<br>kerberos(7) |
| 9. | /lib64/security/pam_sss.so<br>/lib64/security/pam_krb5.so<br>/lib64/security/pam_ldap.so | PAM-модули для аутентификации через AD/LDAP/Kerberos, интеграция с GPO. | Модули PAM. | Доменные пользователи.<br><br>SSSD. | pam_sss(8)<br>pam_krb5(5) |
| 10. | /var/lib/sss/db/cache_<домен>.ldb | Кэш авторизационных данных, GPO, тикетов Kerberos. | Внутренний кэш | Доменные пользователи.<br><br>SSSD. | sss_cache(8) |
| 11. | /etc/samba/smb.conf<br>/etc/nsswitch.conf<br>/etc/krb5.conf | Настройка Kerberos-аутентификации, сопоставления SID-UID, поддержка offline входа, GPO (через gpupdate). | Конфигурационные файлы. | Доменные пользователи.<br><br>Сетевые ресурсы.<br><br>Winbind. | smb.conf(5)<br>nsswitch.conf(5)<br>krb5.conf(5) |
| 12. | /lib64/security/pam_winbind.so<br>/usr/lib64/nss_winbind.so | PAM и NSS-интерфейсы, авторизация по SID через RPC и Kerberos/NTLM. | Модули PAM. | Доменные пользователи.<br><br>Сетевые ресурсы.<br><br>Winbind. | pam_winbind(8) |
| 13. | /var/lib/samba/\*.tdb | TDB-файлы с SID, UID/GID, SID-Map (winbindd_cache.tdb, idmap.tdb). | Внутренний кэш. | Доменные пользователи.<br><br>Сетевые ресурсы.<br><br>Winbind. | samba(7) |
| 14. | /usr/bin/gpupdate<br>/usr/sbin/gpoa<br>/lib64/security/pam_oddjob_gpupdate.so | Применение групповых политик доступа (GPO) при входе через Winbind/SSSD. | Команды.<br>Модули PAM. | Доменные пользователи.<br><br>Сетевые ресурсы.<br><br>GPO. | pam_oddjob_gpupdate(8) |
| 15. | /usr/share/lightdm/lightdm.conf.d/\*.conf<br>/etc/lightdm/lightdm.conf.d/\*.conf<br>/etc/lightdm/lightdm.conf | Настройка дисплейного менеджера LightDM. | Конфигурационные файлы. | Графическая среда (MATE). | lightdm(1) |
| 16. | /usr/sbin/control pam-pkcs11-profile<br>/usr/sbin/control pam-pkcs11-param-set<br>/usr/sbin/control pam-pkcs11-mapping | Выбор профиля железа, логики сопоставления пользователей, параметров поведения и сообщений. | Команды.<br>Контролы. | Аутентификация по смарт-картам (PKCS#11).<br><br>Двухфакторная аутентификация. | [Alt Wiki: Pkcs11-profiles](https://www.altlinux.org/Pkcs11-profiles) |
| 17. | /lib64/security/pam_pkcs11.so | PAM-модули для аутентификации по смарт-картам и токенам (PKCS#11, X.509). | Модули PAM. | Аутентификация по смарт-картам (PKCS#11).<br><br>Двухфакторная аутентификация. | pam_pkcs11(8) |
| 18. | /etc/security/pam_pkcs11<br>/etc/security/cacerts<br>/etc/security/crls<br>$HOME/.eid | Директория с конфигурацией pam_pkcs11, включая доверенные корневые сертификаты (CACerts), списки отозванных (CRL), пользовательские сертификаты. | Конфигурационные файлы. | Аутентификация по смарт-картам (PKCS#11).<br><br>Двухфакторная аутентификация. | pam_pkcs11(8)<br>[Alt Wiki: Pkcs11-profiles](https://www.altlinux.org/Pkcs11-profiles) |
| 19. | /usr/bin/pkcs11_inspect<br>/usr/bin/pklogin_finder | Анализ и проверка токенов, генерация соответствий для subject_mapping у pkcs11. | Команды. | Аутентификация по смарт-картам (PKCS#11).<br><br>Двухфакторная аутентификация. | pam_pkcs11(8) |
| 20. | /usr/sbin/control system-auth pkcs11 | Включение аутентификации по смарт-картам в PAM через pam_pkcs11. | Команда. | Аутентификация по смарт-картам (PKCS#11).<br><br>Двухфакторная аутентификация. | [Alt Wiki: двухфакторная аутентификация](https://www.altlinux.org/Двухфакторная_аутентификация) |
| 21. | /etc/security/pam_pkcs11/subject_mapping<br>/etc/security/pam_pkcs11/cn_mapping | Таблицы соответствия: какой сертификат соответствует какому пользователю. | Конфигурационные файлы. | Аутентификация по смарт-картам (PKCS#11).<br><br>Двухфакторная аутентификация. | [Alt Wiki: двухфакторная аутентификация](https://www.altlinux.org/Двухфакторная_аутентификация) |

**Краткое описание технической реализации меры ИАФ.1**

В операционной системе Альт 8 СП идентификация и аутентификация субъектов доступа реализуются с использованием механизмов локальной, доменной (на базе SSSD и Winbind), двухфакторной аутентификации а аутентификации при помощи смарт-карт.

***Общая последовательность действий при получении интерактивного сеанса***

Пользователь инициирует вход в систему через терминал или графический интерфейс.
* При инициализации входа в систему через терминал запускается программа <code>login</code>, либо соответствующая точка входа (<code>sshd</code>, <code>sudo</code>).
* При инициализации входа в систему через графический интерфейс (MATE) запускается <code>LightDM</code>.

Запускается PAM-стек, настроенный в соответствующем файле из <code>/etc/pam.d/*</code>, в частности:
* <code>/etc/pam.d/system-auth</code> — основная системная политика;
* <code>/etc/pam.d/system-auth-sss</code>, <code>/etc/pam.d/system-auth-krb5</code>, <code>/etc/pam.d/system-auth-winbind</code> — доменные политики;
* <code>/etc/pam.d/sshd</code>, <code>/etc/pam.d/sudo</code> — политики для отдельных точек входа.

В зависимости от конфигурации, выполняются модули <code>pam_unix</code>, <code>pam_sss</code>, <code>pam_winbind</code>, <code>pam_pkcs11</code>, <code>pam_oath</code> и другие.
Вводимые пользователем данные (логин, пароль, токен и др.) передаются соответствующим модулям для сверки с БД пользователей, каталогом <code>LDAP</code>, токеном <code>PKCS#11</code> или системой <code>2FA</code>.
При успешной аутентификации происходит авторизация и предоставление интерактивного сеанса.

***Локальная идентификация и аутентификация***

Пользователь вводит имя и пароль через <code>login</code>, <code>su</code>, <code>sudo</code>, <code>sshd</code> или графический вход.
Активируется PAM-стек (<code>system-auth</code>, <code>sshd</code>, <code>sudo</code>), где основным модулем является <code>pam_unix.so</code>.
<code>pam_unix</code> вызывает функцию <code>password()</code> и сверяет введённый пароль с данными в <code>/etc/shadow</code>, <code>/etc/tcb/[user]/shadow</code>.
При успешной проверке пользователь получает сессию, которая связывается с <code>UID</code>, <code>GID</code> через системные вызовы <code>setuid()</code>, <code>getuid()</code>, <code>geteuid()</code> и т.д.
Идентификатор пользователя может быть просмотрен через <code>id</code>, <code>groups</code>, или использоваться в процессе через <code>getresuid()</code> и т.д.

***Доменная аутентификация через SSSD***

Пользователь вводит имя в формате <code>username@REALM</code> и пароль.<br>
PAM-стек использует модули <code>pam_sss.so</code>, <code>pam_krb5.so</code>, <code>pam_ldap.so</code>, указанные в <code>/etc/pam.d/system-auth-sss</code> и других соответствующих политиках.

Модуль <code>pam_sss</code> направляет запрос аутентификации в <code>sssd</code>, который:
* Получает тикет у Kerberos-сервера на основе данных из <code>/etc/krb5.conf</code>;
* Получает атрибуты пользователя из LDAP-каталога (например, AD) на основе настроек в <code>/etc/sssd/sssd.conf</code>;
* Кэширует результат в <code>/var/lib/sss/db/cache_<домен>.ldb</code>.
* При успешной проверке <code>pam_sss</code> разрешает вход.

Авторизация может быть дополнительно проверена через <code>GPO/HBAC</code> (<code>sssd-ad</code>, параметры файла <code>/etc/sssd/sssd.conf</code>: <code>ad_gpo_access_control</code>, <code>ad_gpo_map_*</code>).

***Доменная аутентификация через Winbind***

Пользователь вводит имя в формате <code>DOMAIN\username</code> или <code>username@domain</code> и пароль.<br>
<code>PAM</code> использует модуль <code>pam_winbind.so</code> и NSS-модуль <code>nss_winbind.so</code>, описанные в <code>/etc/pam.d/system-auth-winbind</code>.<br>

<code>Winbind</code> использует конфигурацию <code>/etc/samba/smb.conf</code>, <code>/etc/nsswitch.conf</code>, <code>/etc/krb5.conf</code>:
* Выполняется аутентификация через <code>Kerberos</code> (или <code>NTLM</code>, если включено);
* <code>SID-UID/GID</code> сопоставляются через <code>IDMAP</code>;
* Результаты кэшируются в <code>/var/lib/samba/*.tdb</code>.

При наличии доступа <code>Winbind</code> возвращает положительный ответ PAM-модулю.<br>
Могут быть применены политики через <code>gpupdate</code>, <code>pam_oddjob_gpupdate.so</code>, поддерживается offline-вход.<br>

***Аутентификация по смарт-карте (PKCS#11)***

Пользователь вставляет смарт-карту в считыватель.<br>
При вызове <code>login</code> или <code>sshd</code>, PAM-стек использует модуль <code>pam_pkcs11.so</code>, активируемый через <code>control system-auth pkcs11</code>.<br>

Модуль проверяет:
* Наличие доверенных сертификатов в <code>/etc/security/cacerts</code>;
* Отсутствие отзыва (<code>CRL</code>) через <code>/etc/security/crls</code>;
* Сопоставление пользователя через <code>subject_mapping</code> или <code>cn_mapping</code> в <code>/etc/security/pam_pkcs11</code>.

Для настройки используются:
* <code>control pam-pkcs11-profile</code> — выбор аппаратного профиля;
* <code>control pam-pkcs11-param-set</code> — поведение (тихий режим, отладка);
* <code>control pam-pkcs11-mapping</code> — логика сопоставления сертификатов и пользователей.

Проверка может быть протестирована через <code>pklogin_finder</code> и <code>pkcs11_inspect</code>.<br>
При успешной проверке создаётся пользовательский сеанс.

***Двухфакторная аутентификация (2FA)***

В конфигурацию PAM (<code>/etc/pam.d/system-auth</code>, <code>sshd</code>) добавляется модуль <code>pam_oath.so</code> или <code>pam_google_authenticator.so</code>.<br>

Пользователь вводит сначала логин/пароль, затем одноразовый пароль (<code>OTP/TOTP</code>):
* <code>OTP</code> сравнивается с ключом в <code>/etc/security/pam_oath.conf</code>;
* <code>TOTP</code> — через файл <code>~/.google_authenticator</code>, созданный при инициализации.

Ввод проверяется на соответствие текущему времени или счётчику.<br>
Вход происходит только при успешной верификации всех факторов.

***Хранение и защита данных***

Все пароли хранятся в хешированном виде (<code>/etc/shadow</code>, <code>tcb</code>).<br>
Сертификаты — в <code>/etc/security/cacerts</code>, <code>CRL</code> — в <code>/etc/security/crls</code>.<br>
Конфигурации пользователей и соответствий — в <code>pam_pkcs11/subject_mapping</code>, <code>cn_mapping</code>.<br>
Ввод данных осуществляется без обратной связи.<br>
Все каналы передачи защищены (<code>TLS</code>, <code>SSH</code>).<br>
Перед вводом данных на физическом терминале выполняется принудительный сброс сессии для обеспечения доверенного маршрута.
