### 1. Reading Data (Queries)

Besides grabbing everything with `findMany()`, you often need specific records.

- **`findUnique()`**: Fetches a single record using a unique identifier (like an `@id` or `@unique` field in your schema).
    
    JavaScript
    
```js
    const singleProduct = await prisma.product.findUnique({
      where: { id: "some-specific-uuid" }
    });
```
    
- **`findFirst()`**: Grabs the very first record that matches your criteria (useful if you don't have a unique ID but want the newest or oldest of something).
    
    JavaScript
    
```js
    const latestOrder = await prisma.order.findFirst({
      where: { payment_status: "PAID" },
      orderBy: { created_at: 'desc' }
    });
```
    

### 2. Creating Data (Mutations)

When a user signs up or places an order, you need to write to the database.

- **`create()`**: Adds one single new record to the table.
    
    JavaScript
    
```js
    const newProfile = await prisma.profile.create({
      data: {
        id: "user-uuid-from-supabase",
        email: "test@example.com",
        role: "USER"
      }
    });
```
    
- **`createMany()`**: Inserts multiple records at the exact same time (great for seeding a database or bulk uploads).
    

### 3. Updating Data (Mutations)

When you need to change existing data, like marking an order as shipped.

- **`update()`**: Updates a specific, single record using its unique ID.
    
    JavaScript
    
```js
    const updatedOrder = await prisma.order.update({
      where: { id: "order-uuid" },
      data: { order_status: "SHIPPED" }
    });
```
    
- **`updateMany()`**: Updates a bunch of records at once based on a condition.
    
- **`upsert()`**: A combination of Update and Insert. It looks for a record; if it finds it, it updates it. If it doesn't exist, it creates a new one.
    

### 4. Deleting Data (Mutations)

- **`delete()`**: Removes a single record by its unique ID.
    
    JavaScript
    
```js
    const deletedItem = await prisma.orderItem.delete({
      where: { id: "order-item-uuid" }
    });
```
    
- **`deleteMany()`**: Deletes multiple records that match a condition (e.g., clearing out abandoned carts or deleting all items from a canceled order).
    

### 5. Math & Aggregation (The Extras)

Prisma isn't just for moving rows around; it can do math for you directly in the database.

- **`count()`**: Tells you how many records match a condition, without actually downloading the records themselves (much faster and uses less memory).
    
    JavaScript
    
```js
    const totalOrders = await prisma.order.count({
      where: { profile_id: "user-uuid" }
    });
```
    
- **`aggregate()`**: Lets you calculate sums, averages, minimums, or maximums. For example, if you wanted to find the total revenue from all orders, you could sum up the `total_amount` field.

**The real magic:** All of these methods allow you to pass in options like `include`, `select`, and `where` so you can pull in related tables (like getting an `Order` and _including_ all of its `OrderItems` at the exact same time) without having to write separate queries!