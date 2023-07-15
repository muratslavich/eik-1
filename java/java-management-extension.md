# Java Management eXtension



Такой сервис (пдселенный в виртуальную машину Java) задача которого наблюдать за тем как себя чувствует JVM. Java Management eXtantion с помощью специальных консольных инструментов позволяет обратиться к JVM и узнать ее состояние.

JMX позволяет:

* Наблюдать состояние JVM
* Читать значения атрибутов
* Задавать атрибуты
* Реализуется через MBeans управляемые бобы

JMX создание: (создание бина)

```
public interface ControllerMBean {
    public int getUsers();
}

public class AccountController implements ControllerMBean {
    public AccountController(IAccountServer accountServer) {
        this.accountServer = accountServer;
    }

    public int getUsers() {
        return accountServer.getUsersCount();
    }
}
```

JMX регистрация:

```
AccountControllerMBean serverStatistics = new AccountController(accountServer);

MBeanServer mbs = ManagementFactory.getPlatformMBeanServer();

ObjectName name = new ObjectName("ServerManager:type=AccountServerController");

mbs.registerMBean(serverStatistics, name);
```

JMX. jConsole&#x20;

Стандартная консоль JMX, которая есть в JDK. Ее можно запустить, сказать к какому приложению можно подсоедениться и посмотреть все данные.
