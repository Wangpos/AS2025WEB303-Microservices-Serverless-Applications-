Step 7: Test the Monolith
Start the application:

docker-compose up --build

![alt text](<screenshots /docker-compose up --build.png>)

# Create a menu item
curl -X POST http://localhost:8080/api/menu \
  -H "Content-Type: application/json" \
  -d '{"name": "Coffee", "description": "Hot coffee", "price": 2.50}'

![alt text](<screenshots /Create a menu item.png>)

# Get menu items
curl http://localhost:8080/api/menu
![alt text](<screenshots /Get menu items.png>)

# Create a user
curl -X POST http://localhost:8080/api/users \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john@example.com"}'

![alt text](<screenshots /create a user.png>)

# Create an order
curl -X POST http://localhost:8080/api/orders \
  -H "Content-Type: application/json" \
  -d '{"user_id": 1, "items": [{"menu_item_id": 1, "quantity": 2}]}'
![alt text](<screenshots /Create an order.png>)


# Get all orders
curl http://localhost:8080/api/orders

![alt text](<screenshots /get all orders.png>)

Menu Service
