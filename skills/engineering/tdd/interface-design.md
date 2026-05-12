# 可测试性的接口设计 (Interface Design for Testability)

好的接口使测试变得自然：

> Good interfaces make testing natural:

1. **接受依赖，不要创建它们 (Accept dependencies, don't create them)**

   ```typescript
   // 可测试 (Testable)
   function processOrder(order, paymentGateway) {}

   // 难以测试 (Hard to test)
   function processOrder(order) {
     const gateway = new StripeGateway();
   }
   ```

2. **返回结果，不要产生副作用 (Return results, don't produce side effects)**

   ```typescript
   // 可测试 (Testable)
   function calculateDiscount(cart): Discount {}

   // 难以测试 (Hard to test)
   function applyDiscount(cart): void {
     cart.total -= discount;
   }
   ```

3. **小表面积 (Small surface area)**
   - 更少方法 = 更少需要的测试
     > Fewer methods = fewer tests needed
   - 更少参数 = 更简单的测试设置
     > Fewer params = simpler test setup
