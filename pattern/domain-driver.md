# 领域驱动设计

## 贫血模型和充血模型
贫血模型：常见的java pojo，比如 User，Company 这样，只包含数据，不包含业务逻辑的类，就叫作贫血模型（Anemic Domain Model）。

充血模型：充血模型（Rich Domain Model）正好相反，数据和对应的业务逻辑被封装到同一个类中。因此，这种充血模型满足面向对象的封装特性，是典型的面向对象编程风格。