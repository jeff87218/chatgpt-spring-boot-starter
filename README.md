[![Maven central](https://maven-badges.herokuapp.com/maven-central/io.github.flashvayne/chatgpt-spring-boot-starter/badge.svg)](https://maven-badges.herokuapp.com/maven-central/io.github.flashvayne/chatgpt-spring-boot-starter)

[中文版文档](https://vayne.cc/2022/12/17/chatgpt-spring-boot-starter)

# chatgpt-spring-boot-starter
This starter is based on OpenAi Official Apis. You can use chatgpt in springboot project easily.  
## Functions:
+ Chat

  You can chat with ChatGPT using many models. Also, multi message is supported, so you can take a series of messages (including the conversation history) as input , and get a response message.

+ Image generation

  Give a prompt and get generated image(s).

## Usage
### 1.Add maven dependency.
```pom
<dependency>
    <groupId>io.github.flashvayne</groupId>
    <artifactId>chatgpt-spring-boot-starter</artifactId>
    <version>1.0.3</version>
</dependency>
```
### 2.Set chatgpt properties in your application.yml

```yml
chatgpt:
  api-key: xxxxxxxxxxx   #api-key. It can be generated here https://platform.openai.com/account/api-keys
# some more properties(model,max-tokens...etc.) have default values. Also you can config them here. 
```
### 3.Inject bean ChatgptService anywhere you require it, and invoke its methods.
#### 3.1 Chat

##### 3.1.1 Single message

```java
@Autowired
private ChatgptService chatgptService;

public void test(){
    String responseMessage = chatgptService.multiChat(Arrays.asList(new MultiChatMessage("user","how are you?")));
    System.out.print(responseMessage); //\n\nAs an AI language model, I don't have feelings, but I'm functioning well. Thank you for asking. How can I assist you today?
}

public void test2(){
    String responseMessage = chatgptService.sendMessage("how are you");
    System.out.print(responseMessage); //I'm doing well, thank you. How about you?
}
```
##### 3.1.2 Multi message. You can take a series of messages (including the conversation history) as input , and return a response message as output.
```java
@Autowired
private ChatgptService chatgptService;

public void testMultiChat(){
    List<MultiChatMessage> messages = Arrays.asList(
            new MultiChatMessage("system","You are a helpful assistant."),
            new MultiChatMessage("user","Who won the world series in 2020?"),
            new MultiChatMessage("assistant","The Los Angeles Dodgers won the World Series in 2020."),
            new MultiChatMessage("user","Where was it played?"));
    String responseMessage = chatgptService.multiChat(messages);
    System.out.print(responseMessage); //The 2020 World Series was played at Globe Life Field in Arlington, Texas.
}
```
+ Tips:
	+ Messages must be an array of message objects, where each object has a role (either "system", "user", or "assistant") and content (the content of the message). Conversations can be as short as 1 message or fill many pages.
	+ The system message helps set the behavior of the assistant. In the example above, the assistant was instructed with "You are a helpful assistant." 
	+ The user messages help instruct the assistant. They can be generated by the end users of an application, or set by a developer as an instruction.
	+ The assistant messages help store prior responses. They can also be written by a developer to help give examples of desired behavior.
	  For more details, please refer to [chat format](https://platform.openai.com/docs/guides/chat/introduction)
	

#### 3.2 Image generation
```java
@Autowired
private ChatgptService chatgptService;

public void testImage(){
    String imageUrl = chatgptService.imageGenerate("A cute baby sea otter");
    System.out.print(imageUrl); //https://oaidalleapip.......
}

public void testImageList(){
    List<String> images = chatgptService.imageGenerate("A cute baby sea otter", 2, ImageSize.SMALL, ImageFormat.URL);
    System.out.print(images.toString());//["https://oaidalleapipr.....ZwA%3D","https://oaidalleapipr....RE0%3D"]
}
```

## Demo project：
[demo-chatgpt-spring-boot-starter](https://github.com/flashvayne/demo-chatgpt-spring-boot-starter)

## Demo online experience
https://vayne.cc/chat/

*This demo does not work now, because my account's request times quota is exceeded.  
You can use your own account api-key to let your demo work.  

# Author Info
Email: flashvayne@gmail.com

Blog: https://vayne.cc
