
1. Нажмите кнопку **Службы приложений**, выберите нужную серверную часть мобильных приложений, откройте раздел **Быстрый запуск**, а затем выберите клиентскую платформу (iOS, Android, Xamarin или Cordova).

    ![Портал Azure с выделенным пунктом "Mobile Apps Quickstart" (Быстрый запуск мобильных приложений)][quickstart]

2. Если подключение к базе данных не настроено, создайте его, сделав следующее:

    ![Подключение мобильных приложений к базе данных на портале Azure][connect]

    а. Создайте сервер и базу данных SQL.

    ![Создание базы данных и сервера для мобильных приложений на портале Azure][server]

    b. Подождите, пока подключение к данным не будет успешно создано.

    ![Уведомление портала Azure об успешном создании подключения к данным][notification]

    c. Подключение к данным должно быть успешными.

    ![Уведомление на портале Azure: "Уже имеется подключение к данным"][already-connection]

3. В разделе **2. Создание API таблицы** выберите **язык серверной части** — Node.js. 
 
4. Примите подтверждение и выберите **Создание таблицы TodoItem**.  
    В базе данных будет создана таблица с элементами списка дел. 

    >[!IMPORTANT]
    > При переключении существующей серверной части на Node.js все содержимое перезаписывается. Чтобы вместо этого создать серверную часть .NET, см. раздел [Work with the .NET backend server SDK for Azure Mobile Apps][instructions] (Работа с пакетом SDK для серверной части .NET мобильных приложений Azure).

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
