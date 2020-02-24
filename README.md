# Initial page

## test

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



