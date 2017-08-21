# 代码整洁之道 clean code

## 重构switch 语句
```java
public Money calculatePay(Employee e) throws InvalidEmployeeType {
  switch (e.type) {
    case COMMISSIONED:
      return calculateCommissionedPay(e);
    case HOURLY:
      return calculateHourlyPay(e);
    case SALARIED:
      return calculateSalariedPay(e);
    default:
      throw new InvalidEmployeeType(e.type);
  }
}
```

使用抽象工厂进行重构上面的代码
```java
public abstract class Employee {
    public abstract boolean isPayday();
    public abstract Money calculatePay();
    public abstract void deliverPay(Money pay);
}

//===========
public interface EmployeeFactory {
  public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
}

// -----------------
public class EmployeeFactoryImpl implements EmployeeFactory {
  public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
    switch (r.type) {
    case COMMISSIONED:
    return new CommissionedEmployee(r) ;
    case HOURLY:
    return new HourlyEmployee(r);
    case SALARIED:
    return new SalariedEmploye(r);
    default:
    throw new InvalidEmployeeType(r.type);
    }
  }
}
```