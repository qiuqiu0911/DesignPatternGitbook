# 观察者模式

**本模式需要解决的问题**

建立一个气象观测站，根据气象信息\(温度、湿度、气压\)的变化，设计多个**气象布告板**

**未应用设计模式前**

```java
interface Display {
    void update(float temp, float humidity, float pressure);
}

class DisPlay1 implements Display {
    @Override
    public void update(float temp, float humidity, float pressure) {
        System.out.println("display1:" + temp + humidity + pressure);
    }
}

class DisPlay2 implements Display {
    @Override
    public void update(float temp, float humidity, float pressure) {
        System.out.println("display2:" + temp + humidity + pressure);
    }
}

class weatherData {
    float temp;
    float humidity;
    float pressure;

    public void onChange() {
        new DisPlay1().update(temp, humidity, pressure);
        new DisPlay2().update(temp, humidity, pressure);
    }
}
```

缺点：

* 针对具体实现编程，回导致以后在增加或删除布告板`Display`时需要修改`WeatherData`

**应用观察者模式后**

```java
/**
 * 主题
 */
interface Subject {
    /**
     * 注册观察者
     */
    void registerObserver(ObserverV1 observer);

    /**
     * 取消注册观察者
     */
    void removeObserver(ObserverV1 observer);

    /**
     * 通知观察者
     */
    void notifyObservers();
}

/**
 * 所有观察者都必须实现
 */
interface ObserverV1 {
    void update(String temperature, String humidity, String pressure);
}

class DefaultObserver implements ObserverV1 {

    @Override
    public void update(String temperature, String humidity, String pressure) {
        System.out.println(DefaultObserver.class.toGenericString() + ":" + temperature + "   " + humidity + "   " + pressure);
    }
}

class SecondObserver implements ObserverV1 {

    @Override
    public void update(String temperature, String humidity, String pressure) {
        System.out.println(SecondObserver.class.toGenericString()+":气温是:" + temperature + ", 气压是:" + pressure);
    }
}

/**
 * 天气数据
 */
class WeatherDataV1 implements Subject {

    private Set<ObserverV1> observers = new HashSet<>();

    @Override
    public void registerObserver(ObserverV1 observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(ObserverV1 observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        observers.forEach(observer -> observer.update(temperature, humidity, pressure));
    }

    private String temperature;
    private String humidity;
    private String pressure;

    public void setTemperature(String temperature) {
        this.temperature = temperature;
        onKeyChanged();
    }

    public void setHumidity(String humidity) {
        this.humidity = humidity;
        onKeyChanged();
    }

    public void setPressure(String pressure) {
        this.pressure = pressure;
        onKeyChanged();
    }

    public void onKeyChanged() {
        notifyObservers();
    }
}
```

\*\*\*\*

