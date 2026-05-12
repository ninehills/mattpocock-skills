# 面向可测试性的接口设计

好的接口使测试变得自然：

1. **接受依赖，不要创建它们**

   ```typescript
   // 可测试
   function processOrder(order, paymentGateway) {}

   // 难以测试
   function processOrder(order) {
     const gateway = new StripeGateway();
   }
   ```

2. **返回结果，不要产生副作用**

   ```typescript
   // 可测试
   function calculateDiscount(cart): Discount {}

   // 难以测试
   function applyDiscount(cart): void {
     cart.total -= discount;
   }
   ```

3. **小的表面面积**
   - 更少的方法 = 需要更少的测试
   - 更少的参数 = 更简单的测试设置
