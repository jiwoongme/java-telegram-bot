# java-telegram-bot

자바기반의 텔레그램 봇 실행 튜토리얼 입니다  
Tutorial for implementing a telegram bot based on JAVA

### 전제조건 PreRequisite

먼저, 필요한 것은 
- 자바에서 Maven, Gradle, 라이브러리를 사용하여 프로그래밍을 할 수 있도록 도와주는 IDE (IntelliJ, Eclipse 등)
- Telegram BotFather를 통한 봇 생성 및 토큰 발급

First of all, you need
- IDE that helps your JAVA programming and Maven, Gradle (IntelliJ, Eclipse etc)
- Create bot, get token through 'BotFather'

<div>
<img width="200" src="https://user-images.githubusercontent.com/67405229/91852876-61e9ff80-ec9c-11ea-821d-1a26ec8a0958.png">
<img width="200" src="https://user-images.githubusercontent.com/67405229/91852887-66161d00-ec9c-11ea-8361-bce1ddd3a1f8.png">
<img width="200" src="https://user-images.githubusercontent.com/67405229/91852892-67474a00-ec9c-11ea-89bf-c1e3a77eb2d6.png">
</div>

### 사용법 Usage

1. Maven, Gradle, jar 파일 등을 통해서 라이브러리를 프로젝트에 import 해줍니다.  
Import library into the project.

```xml
    <dependency>
        <groupId>org.telegram</groupId>
        <artifactId>telegrambots</artifactId>
        <version>4.9.1</version>
    </dependency>
```

```gradle
    compile "org.telegram:telegrambots:4.9.1"
```
[Jar file link](https://mvnrepository.com/artifact/org.telegram/telegrambots/4.9.1)

2. 나만의 봇을 만들기 위해서 `org.telegram.telegrambots.bots.TelegramLongPollingBot`를 extend 하여 클래스를 만듭니다.  
In order to create my own bot, make a class using `org.telegram.telegrambots.bots.TelegramLongPollingBot`

```java

import org.telegram.telegrambots.bots.TelegramLongPollingBot;
import org.telegram.telegrambots.meta.api.methods.send.SendMessage;
import org.telegram.telegrambots.meta.api.objects.Update;
import org.telegram.telegrambots.meta.exceptions.TelegramApiException;

public class MyTelegramBot extends TelegramLongPollingBot {
    public void onUpdateReceived(Update update) {
        //메세지 로그 확인 Check message Log
        System.out.print(update);
        
        //받은 메세지 그대로 보내기 Send echo message
        SendMessage sendMessage = new SendMessage();
        sendMessage.setText(update.getMessage().getText());
        sendMessage.setChatId(update.getMessage().getChatId());
        try {
            execute(sendMessage);
        } catch (TelegramApiException e) {
            e.printStackTrace();
        }
    }

    public String getBotUsername() {
        return "봇 이름 Bot Name";
    }

    public String getBotToken() {
        return "토큰 Token";
    }
}

```

3. Main 클래스에서 Custom한 MyTelegramBot 실행  
Running Customed MyTelegramBot class in Main Class


```java

import org.telegram.telegrambots.ApiContextInitializer;
import org.telegram.telegrambots.meta.TelegramBotsApi;
import org.telegram.telegrambots.meta.exceptions.TelegramApiException;

public class Main {
    public static void main(String[] args) {
        ApiContextInitializer.init();
        TelegramBotsApi telegramBotsApi = new TelegramBotsApi();
        try {
            telegramBotsApi.registerBot(new MyTelegramBot());
        } catch (TelegramApiException e) {
            e.printStackTrace();
        }
    }
}

```
