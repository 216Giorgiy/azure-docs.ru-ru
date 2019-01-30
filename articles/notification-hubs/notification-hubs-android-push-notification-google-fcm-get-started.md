---
title: Отправка push-уведомлений в приложения Android с помощью Центров уведомлений Azure и Firebase Cloud Messaging | Документация Майкрософт
description: Из этого руководства вы узнаете, как использовать центры уведомлений Azure и Google Firebase Cloud Messaging для отправки push-уведомлений на устройства Android.
services: notification-hubs
documentationcenter: android
keywords: push-уведомления, push-уведомление, push-уведомление android, FCM, Firebase Cloud Messaging
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: d758a46a19dcd109ee5786e25c886dd30b264f7e
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447503"
---
# <a name="tutorial-push-notifications-to-android-devices-by-using-azure-notification-hubs-and-google-firebase-cloud-messaging"></a>Руководство. Отправка push-уведомлений на устройства Android с помощью Центров уведомлений Azure и Google Firebase Cloud Messaging

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

> [!IMPORTANT]
> В этой статье описывается отправка push-уведомлений с помощью Google Firebase Cloud Messaging (FCM). Если вы используете Google Cloud Messaging (GCM), см. сведения в статье [Отправка push-уведомлений в приложения Android с помощью центров уведомлений Azure](notification-hubs-android-push-notification-google-gcm-get-started.md).

В этом руководстве показано, как использовать центры уведомлений Azure и Firebase Cloud Messaging для отправки уведомлений в приложение на платформе Android. Следуя инструкциям этого руководства, вы создадите пустое приложение Android, которое получает push-уведомления с помощью Firebase Cloud Messaging.

Полный код для этого руководства можно скачать на сайте [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).

При работе с этим руководством вы выполните следующие задачи:

> [!div class="checklist"]
> * Создание проекта Android Studio.
> * Создание проекта Firebase с поддержкой Firebase Cloud Messaging.
> * Создание центра уведомлений.
> * Подключение приложения к центру уведомлений.
> * Тестирование приложения.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим учебником необходима активная учетная запись Azure. Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/free/).

* Кроме упомянутой выше действующей учетной записи Azure, для работы с этим руководством вам понадобится последняя версия [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797).
* Android версии 2.3 или более поздней.
* Репозиторий Google версии 27 или более поздней.
* Службы Google Play версии 9.0.2 или более поздней.
* Завершение изучения этого учебника является необходимым условием для работы со всеми другими учебниками, посвященными Центрам уведомлений для приложений Android.

## <a name="create-an-android-studio-project"></a>Создание проекта Android Studio

1. В Android Studio создайте новый проект Android Studio.

    ![Android Studio — новый проект](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. Выберите форм-фактор **Phone and Tablet** (Телефон и планшет) и минимальную версию пакета SDK (с помощью параметра **Minimum SDK**), которые нужно поддерживать. Нажмите кнопку **Далее**.

    ![Android Studio — рабочий процесс создания проекта](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Выберите для основного действия значение **Empty Activity** (Пустое действие), нажмите кнопку **Next** (Далее), а затем — **Finish** (Готово).

## <a name="create-a-firebase-project-that-supports-fcm"></a>Создание проекта Firebase с поддержкой FCM

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Настройка центра уведомлений

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Настройка параметров Firebase Cloud Messaging для центра

1. Выберите **Google (GCM)** в категории **параметров уведомления**.
2. Введите ключ API (ключ сервера FCM), скопированный ранее из [консоли Firebase](https://firebase.google.com/console/).
3. На панели инструментов щелкните **Сохранить**.

    ![Центры уведомлений Azure — Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Теперь центр уведомлений настроен для работы с Firebase Cloud Messaging, а у вас есть строки подключения, с помощью которых вы можете зарегистрировать приложение для получения и отправки push-уведомлений.

## <a id="connecting-app"></a>Подключение приложения к центру уведомлений

### <a name="add-google-play-services-to-the-project"></a>Добавление служб Google Play в проект

[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Добавление библиотек Центров уведомлений Azure

1. В файле `Build.Gradle` в классе **app** добавьте следующие строки в раздел **dependencies**.

    ```text
    compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. После раздела **dependencies** добавьте следующий репозиторий:

    ```text
    repositories {
        maven {
            url "http://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```

### <a name="add-google-firebase-support"></a>Добавление поддержки Google Firebase

1. В файле `Build.Gradle` в классе **app** добавьте следующие строки в раздел **dependencies**.

    ```text
    compile 'com.google.firebase:firebase-core:12.0.0'
    ```

2. Добавьте в конец файла следующий подключаемый модуль.

    ```text
    apply plugin: 'com.google.gms.google-services'
    ```

### <a name="updating-the-androidmanifestxml"></a>Обновление файла AndroidManifest.xml

1. Чтобы обеспечить поддержку FCM, следует реализовать службу прослушивания идентификаторов экземпляра в своем коде. Таким образом можно [получать маркеры регистрации](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) с помощью [API идентификаторов экземпляра Google Firebase](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId). В этом руководстве имя класса — `MyInstanceIDService`.

    Добавьте приведенное ниже определение службы внутри тега `<application>` в файле AndroidManifest.xml.

    ```xml
    <service android:name=".MyInstanceIDService">
        <intent-filter>
            <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
    </service>
    ```

2. Получив маркер регистрации в FCM из API идентификатора экземпляра Firebase, используйте его для [регистрации в центре уведомлений Azure](notification-hubs-push-notification-registration-management.md). Регистрация в фоновом режиме выполняется с помощью службы `IntentService` с именем `RegistrationIntentService`. Кроме того, эта служба отвечает за обновление маркера регистрации в FCM.

    Добавьте приведенное ниже определение службы внутри тега `<application>` в файле AndroidManifest.xml.

    ```xml
    <service
        android:name=".RegistrationIntentService"
        android:exported="false">
    </service>
    ```

3. Теперь вам нужно определить получателя уведомлений. Добавьте следующее определение получателя внутри тега `<application>` в файле AndroidManifest.xml. Замените заполнитель `<your package>` фактическим именем своего пакета, отображенным в верхней части файла `AndroidManifest.xml`.

    ```xml
    <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <category android:name="<your package name>" />
        </intent-filter>
    </receiver>
    ```

4. Добавьте следующие разрешения, связанные с FCM, под тегом `</application>`.

    Дополнительные сведения об этих разрешениях см. в статье [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest) (Настройка клиентского приложения GCM для Android) и [Migrate a GCM Client App for Android to Firebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm) (Перенос клиентского приложения GCM для Android в Firebase Cloud Messaging).

    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="adding-code"></a>Добавление кода

1. В представлении проекта разверните узел **app** > **src** > **main** > **java**. Щелкните правой кнопкой мыши папку пакета в **java**, щелкните **New** (Создать) и выберите **Java Class** (Класс Java). Добавьте новый класс с именем `NotificationSettings`.

    ![Android Studio — новый класс Java](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)

    Обязательно обновите эти три заполнителя в следующем коде для класса `NotificationSettings`:

   * **SenderId** — идентификатор отправителя, полученный ранее на вкладке **Cloud Messaging** (Обмен сообщениями в облаке) параметров проекта в [консоли Firebase](https://firebase.google.com/console/).
   * **HubListenConnectionString** — укажите для центра строку подключения **DefaultListenAccessSignature**. Чтобы скопировать эту строку подключения, щелкните пункт **Политики доступа** в своем центре на [портал Azure].
   * **HubName**: используйте имя центра уведомлений, которое отображается на [портал Azure] на странице центра.

     `NotificationSettings` :

    ```java
    public class NotificationSettings {
        public static String SenderId = "<Your project number>";
        public static String HubName = "<Your HubName>";
        public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
    }
    ```

2. Добавьте еще один новый класс с именем `MyInstanceIDService`, следуя инструкциям выше. Этот класс реализует службу прослушивания идентификаторов экземпляра.

    Код для этого класса вызывает службу `IntentService`, чтобы [обновить маркер FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) в фоновом режиме.

    ```java
    import android.content.Intent;
    import android.util.Log;
    import com.google.firebase.iid.FirebaseInstanceIdService;

    public class MyInstanceIDService extends FirebaseInstanceIdService {

        private static final String TAG = "MyInstanceIDService";

        @Override
        public void onTokenRefresh() {

            Log.d(TAG, "Refreshing GCM Registration Token");

            Intent intent = new Intent(this, RegistrationIntentService.class);
            startService(intent);
        }
    };
    ```

3. Добавьте еще один новый класс в проект `RegistrationIntentService`. Этот класс реализует интерфейс `IntentService`, [обновляет маркер FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) и [выполняет регистрацию в центре уведомлений](notification-hubs-push-notification-registration-management.md).

    Используйте для этого класса следующий код:

    ```java
    import android.app.IntentService;
    import android.content.Intent;
    import android.content.SharedPreferences;
    import android.preference.PreferenceManager;
    import android.util.Log;
    import com.google.firebase.iid.FirebaseInstanceId;
    import com.microsoft.windowsazure.messaging.NotificationHub;

    public class RegistrationIntentService extends IntentService {

        private static final String TAG = "RegIntentService";

        private NotificationHub hub;

        public RegistrationIntentService() {
            super(TAG);
        }

        @Override
        protected void onHandleIntent(Intent intent) {

            SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
            String resultString = null;
            String regID = null;
            String storedToken = null;

            try {
                String FCM_token = FirebaseInstanceId.getInstance().getToken();
                Log.d(TAG, "FCM Registration Token: " + FCM_token);

                // Storing the registration ID that indicates whether the generated token has been
                // sent to your server. If it is not stored, send the token to your server,
                // otherwise your server should have already received the token.
                if (((regID=sharedPreferences.getString("registrationID", null)) == null)){

                    NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                            NotificationSettings.HubListenConnectionString, this);
                    Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                    regID = hub.register(FCM_token).getRegistrationId();

                    // If you want to use tags...
                    // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                    // regID = hub.register(token, "tag1,tag2").getRegistrationId();

                    resultString = "New NH Registration Successfully - RegId : " + regID;
                    Log.d(TAG, resultString);

                    sharedPreferences.edit().putString("registrationID", regID ).apply();
                    sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                }

                // Check if the token may have been compromised and needs refreshing.
                else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {

                    NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                            NotificationSettings.HubListenConnectionString, this);
                    Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                    regID = hub.register(FCM_token).getRegistrationId();

                    // If you want to use tags...
                    // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                    // regID = hub.register(token, "tag1,tag2").getRegistrationId();

                    resultString = "New NH Registration Successfully - RegId : " + regID;
                    Log.d(TAG, resultString);

                    sharedPreferences.edit().putString("registrationID", regID ).apply();
                    sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                }

                else {
                    resultString = "Previously Registered Successfully - RegId : " + regID;
                }
            } catch (Exception e) {
                Log.e(TAG, resultString="Failed to complete registration", e);
                // If an exception happens while fetching the new token or updating our registration data
                // on a third-party server, this ensures that we'll attempt the update at a later time.
            }

            // Notify UI that registration has completed.
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(resultString);
            }
        }
    }
    ```

4. Над объявлением класса `MainActivity` добавьте следующие операторы `import`:

    ```java
    import com.google.android.gms.common.ConnectionResult;
    import com.google.android.gms.common.GoogleApiAvailability;
    import com.microsoft.windowsazure.notifications.NotificationsManager;
    import android.content.Intent;
    import android.util.Log;
    import android.widget.TextView;
    import android.widget.Toast;
    ```

5. В верхней части класса добавьте следующие частные члены. Используйте эти поля для [проверки доступности служб Google Play в соответствии с рекомендациями Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

    ```java
    public static MainActivity mainActivity;
    public static Boolean isVisible = false;
    private static final String TAG = "MainActivity";
    private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

6. В классе `MainActivity` добавьте следующий метод проверки доступности служб Google Play:

    ```java
    /**
    * Check the device to make sure it has the Google Play Services APK. If
    * it doesn't, display a dialog that allows users to download the APK from
    * the Google Play Store or enable it in the device's system settings.
    */

    private boolean checkPlayServices() {
        GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
        int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.SUCCESS) {
            if (apiAvailability.isUserResolvableError(resultCode)) {
                apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                        .show();
            } else {
                Log.i(TAG, "This device is not supported by Google Play Services.");
                ToastNotify("This device is not supported by Google Play Services.");
                finish();
            }
            return false;
        }
        return true;
    }
    ```

7. В классе `MainActivity` добавьте следующий код, который проверяет Сервисы Google Play, прежде чем вызывать службу `IntentService`. Таким образом вы получите маркер регистрации в FCM и выполните регистрацию в центре уведомлений.

    ```java
    public void registerWithNotificationHubs()
    {
        if (checkPlayServices()) {
            // Start IntentService to register this application with FCM.
            Intent intent = new Intent(this, RegistrationIntentService.class);
            startService(intent);
        }
    }
    ```

8. В методе `OnCreate` класса `MainActivity` добавьте следующий код, чтобы начать регистрацию при создании действия:

    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mainActivity = this;
        NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
        registerWithNotificationHubs();
    }
    ```

9. Добавьте эти дополнительные методы в класс `MainActivity`, чтобы проверять состояние приложения и отображать в нем полученные данные.

    ```java
    @Override
    protected void onStart() {
        super.onStart();
        isVisible = true;
    }

    @Override
    protected void onPause() {
        super.onPause();
        isVisible = false;
    }

    @Override
    protected void onResume() {
        super.onResume();
        isVisible = true;
    }

    @Override
    protected void onStop() {
        super.onStop();
        isVisible = false;
    }

    public void ToastNotify(final String notificationMessage) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                TextView helloText = (TextView) findViewById(R.id.text_hello);
                helloText.setText(notificationMessage);
            }
        });
    }
    ```

10. Метод `ToastNotify` использует элемент управления `TextView` *Hello World*, чтобы постоянно передавать в приложение сведения о состоянии и уведомления. В файле макета activity_main.xml добавьте следующий идентификатор для этого элемента управления.

    ```java
    android:id="@+id/text_hello"
    ```

11. Затем добавьте подкласс для получателя, определенного в файле AndroidManifest.xml. Добавьте еще один новый класс в проект `MyHandler`.

12. Добавьте в начало файла `MyHandler.java` следующие операторы импорта:

    ```java
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.media.RingtoneManager;
    import android.net.Uri;
    import android.os.Bundle;
    import android.support.v4.app.NotificationCompat;
    import com.microsoft.windowsazure.notifications.NotificationsHandler;
    ```

13. Добавьте в класс `MyHandler` следующий код, чтобы сделать его подклассом класса `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Этот код переопределяет метод `OnReceive` так, чтобы обработчик сообщал о полученных уведомлениях. Кроме того, обработчик отправляет push-уведомление в диспетчер уведомлений Android с помощью метода `sendNotification()` . Метод `sendNotification()` должен выполняться, когда незапущенное приложение получает уведомление.

    ```java
    public class MyHandler extends NotificationsHandler {
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        Context ctx;

        @Override
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String nhMessage = bundle.getString("message");
            sendNotification(nhMessage);
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(nhMessage);
            }
        }

        private void sendNotification(String msg) {

            Intent intent = new Intent(ctx, MainActivity.class);
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);

            mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                    intent, PendingIntent.FLAG_ONE_SHOT);

            Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
            NotificationCompat.Builder mBuilder =
                    new NotificationCompat.Builder(ctx)
                            .setSmallIcon(R.mipmap.ic_launcher)
                            .setContentTitle("Notification Hub Demo")
                            .setStyle(new NotificationCompat.BigTextStyle()
                                    .bigText(msg))
                            .setSound(defaultSoundUri)
                            .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }
    }
    ```

14. В Android Studio в строке меню щелкните **Build** (Сборка)  > **Rebuild Project** (Повторить сборку проекта), чтобы убедиться в отсутствии ошибок.

15. Запустите приложение на устройстве и убедитесь, что регистрация в центре уведомлений успешно выполнена.

    > [!NOTE]
    > Регистрация может завершится сбоем при первом запуске до вызова метода `onTokenRefresh()` службы идентификаторов экземпляра. Чтобы заново начать регистрацию в центре уведомлений, обновите страницу.

## <a name="test-the-app"></a>Тестирование приложения

### <a name="test-send-notification-from-the-notification-hub"></a>Проверка отправки уведомления из центра уведомлений

Отправьте push-уведомления с [портал Azure], выполнив следующие действия.

1. В разделе **Устранение неполадок** выберите **Тестовая отправка**.
2. В качестве **платформы** выберите **Android**.
3. Нажмите кнопку **Отправить**.  Уведомление еще не отображается на устройстве Android, так как не было запущено мобильное приложение. После запуска мобильного приложения нажмите еще раз кнопку **Отправить**, чтобы просмотреть уведомление.
4. Проверьте **результат** операции в списке в нижней части окна.

    ![Центры уведомлений Azure — тестовая отправка](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

### <a name="run-the-mobile-app"></a>Запуск мобильного приложения

Если вы хотите проверить отправку push-уведомлений в эмуляторе, убедитесь, что образ эмулятора поддерживает уровень API Google, выбранный для приложения. Если образ не поддерживает собственные API-интерфейсы Google, создается исключение **SERVICE\_NOT\_AVAILABLE**.

Кроме того, добавьте учетную запись Google в запущенный эмулятор. Для этого щелкните **Параметры** > **Учетные записи**. В противном случае попытки регистрации в GCM могут привести к исключению **AUTHENTICATION\_FAILED**.

1. Запустите приложение и убедитесь, что для успешной регистрации сообщается идентификатор регистрации.

    ![Тестирование на устройстве Android — регистрация канала](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Введите сообщение уведомления для отправки на все устройства Android, которые зарегистрированы в центре.

    ![Тестирование на устройстве Android — отправка сообщения](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. Нажмите кнопку **Send Notification**(Отправить уведомление). На всех устройствах с запущенным приложением отображается экземпляр `AlertDialog` с push-уведомлением. Устройства, на которых приложение не запущено, но которые были ранее зарегистрированы для приема push-уведомлений, получают уведомление в диспетчер уведомлений Android. Уведомления можно просматривать, проводя пальцем вниз от левого верхнего угла.

    ![Тестирование на устройстве Android — уведомления](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Дополнительная информация

Во время работы с этим руководством вы использовали Firebase Cloud Messaging для отправки push-уведомлений на устройства Android. Чтобы узнать, как отправлять push-уведомления с помощью Google Cloud Messaging, перейдите к следующему руководству:

> [!div class="nextstepaction"]
>[Руководство по отправке push-уведомлений на устройства Android с помощью Центров уведомлений Azure и Google Cloud Messaging](notification-hubs-android-push-notification-google-gcm-get-started.md)

<!-- Images. -->

<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: notification-hubs-push-notification-overview.md
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[портал Azure]: https://portal.azure.com
